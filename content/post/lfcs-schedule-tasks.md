+++
title      = "LFCS : Programmer des tâches"
date       = "2017-06-15T08:50:34+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Lorsque l'on administre un système Linux, il est souvent nécessaire d'effectuer certaines tâches régulièrement. Pour n'en citer que quelques unes, il peut s'agir d'un backup régulier du système (voir en complément [mon article sur l'archivage]({{< ref "lfcs-archive.md" >}})), de maintenance (recherche de mises-à-jour), etc. Bien sûr, il est bien plus pratique d'automatiser ces tâches plutôt que de les effectuer manuellement. Sous Linux, il est facile de le faire, en programmant des tâches spécifiques pour s'exécuter à une date/heure donnée, et la commande qui va nous le permettre est **cron**.

# Programmer une tâche avec _cron_

**cron** est un **démon** (ou _daemon_), c'est à dire une tâche exécutée en arrière plan (ou _background_), qui va vérifier régulièrement la date/heure du système et lancer si nécessaire des tâches préprogrammées (appelées **crontab**) contenues dans le fichier **/etc/crontab**[^1].

[^1]: Pour les systèmes basés sur _Debian_ (_Ubuntu_ par exemple), des sous-fichiers /etc/cron.daily, /etc/cron.weekly et /etc/cron.monthly sont gérés par le système, mais cela n'a pas d'impact majeur sur la manière de programmer des tâches.

Comme souvent, il est préférable de ne pas modifier ce fichier directement, mais plutôt d'utiliser la commande **crontab -e**, qui le fera sans risque de mauvais formatage. Une tâche _cron_ se décrit donc en une ligne dans le fichier _/etc/crontab_. Chaque ligne comprends 5 paramètres (chacun séparé par un espace ou une tabulation) : les minute, heure, jour du mois, mois et jour de la semaine (date/heure d'exécution de la tâche), ainsi que la commande à exécuter[^2]. Dans la date/heure, un symbole **\*** permet de dire "tous" (tous les jours, tous les mois, etc.).

La commande _crontab_ peut aussi prendre d'autres options, notamment :

- **-l** : Retourne la liste des tâches programmées
- **-r** : Supprime la _crontab_ en cours
- **-u** : Spécifie l'utilisateur dont les _crontab_ seront utilisées/modifiées
- **-i** : Active une demande de confirmation avant de supprimer une _crontab_

[^2]: Lors de l'écriture directe du fichier _/etc/crontab_, un champs s'ajoute entre la date/heure et la commande : l'utilisateur. Avec la commande _crontab_, l'utilisateur en cours sera choisi par défaut.

Voyons par exemple comment programmer une tâche tous les 6 du mois à 6h25, et permettant d'archiver le répertoire _/home_ :

	$ crontab -l
	no crontab for zirka

	$ crontab -e
	25 6 1 * * tar -cf /tmp/backup.tar /home

	no crontab for zirka - using an empty one
	crontab: installing new crontab

	$ crontab -l
	25 6 1 * * tar -cf /tmp/backup.tar /home

	$ crontab -r
	$ crontab -l
	no crontab for zirka

Lors de la création d'une tâche _cron_, il est aussi possible de remplacer les champs de date/heure par ces raccourcis :

- **@reboot** : Effectue la tâche en question à chaque redémarrage du système
- **@yearly** : Effectue la tâche en question tous les ans (équivalant à 0 0 1 1 *)
- **@monthly** : Effectue la tâche en question tous les mois (équivalant à 0 0 1 * *)
- **@daily** : Effectue la tâche en question tous les jours (équivalant à 0 0 * * *)
- **@hourly** : Effectue la tâche en question tous les mois (équivalant à 0 * * * *)
- **@weekly**, **@midnight**, ...

# Vérifier la complétion de tâches

Lorsqu'une tâche est exécutée automatiquement, _cron_ écrit un ou plusieurs _logs_[^3] pour le signaler. Ces logs sont écrits par défaut dans le fichier **/var/log** ou /var/log/syslog[^4]. Par défaut, _cron_ n'enregistre que le début d'exécution des tâches (i.e. le fait qu'elles aient été lancées), mais il est possible d'indiquer à la commande _cron_ de logguer plus ou moins de choses. Pour cela, il suffit de l'appeler avec l'option **-L** suivie d'une somme des valeurs suivantes (en fonctions des logs que nous voulons voir apparaitre) :

[^3]: Un [_log_](https://en.wikipedia.org/wiki/Logfile) est un événement enregistré par le système pour historisation. Voir [mon article sur le sujet]({{< ref "lfcs-system-logs.md" >}}) pour plus de détails.
[^4]: Nous verrons plus en détail dans un prochain article comment analyser les logs système.

- **1** : log le début d'exécution de toute tâche _cron_
- **2** : log la fin d'exécution de toute tâche _cron_
- **4** : log toute tâche _cron_ ayant échoué
- **8** : log l'identifiant du processus[^5] de toute tâche _cron_

Pour activer ces niveaux de logs, il suffit de modifier le fichier **/etc/init/cron.conf**, qui régit l'initialisation de _cron_ lors du démarrage du système. Remplacez ainsi _exec cron_ par _exec cron -L 7_ par exemple.

[^5]: Nous aborderons la question des processus et threads dans un prochain article.

Pour nous faire une idée du fonctionnement des logs, programmons une tâche toutes les heures, qui ajoute une ligne à un fichier temporaire :

	$ crontab -l
	no crontab for zirka

	$ crontab -e
	@hourly echo "Test en cours" >> /tmp/test.txt

	$ crontab -l
	@hourly echo "Test en cours" >> /tmp/test.txt

Après quelques heures, regardons les logs :

	$ cat /tmp/test.txt
	Test en cours
	Test en cours
	Test en cours
	Test en cours
	Test en cours

	$ cat /var/log/syslog | grep -i "cron"
	Jun 16 08:58:51 Epinet crontab[22604]: (zirka) LIST (zirka)
	Jun 16 08:49:54 Epinet crontab[21967]: (zirka) BEGIN EDIT (zirka)
	Jun 16 08:50:10 Epinet crontab[21967]: (zirka) REPLACE (zirka)
	Jun 16 08:50:10 Epinet crontab[21967]: (zirka) END EDIT (zirka)
	Jun 16 08:58:51 Epinet crontab[22604]: (zirka) LIST (zirka)
	Jun 16 09:00:01 Epinet CRON[22702]: (zirka) CMD (echo "Test en cours" >> /tmp/test.txt)
	Jun 16 10:00:01 Epinet CRON[27080]: (zirka) CMD (echo "Test en cours" >> /tmp/test.txt)
	Jun 16 11:00:01 Epinet CRON[31111]: (zirka) CMD (echo "Test en cours" >> /tmp/test.txt)
	Jun 16 12:00:01 Epinet CRON[2688]: (zirka) CMD (echo "Test en cours" >> /tmp/test.txt)
	Jun 16 13:00:01 Epinet CRON[7165]: (zirka) CMD (echo "Test en cours" >> /tmp/test.txt)

	$ crontab -l
	@hourly echo "Test en cours" >> /tmp/test.txt

	$ crontab -r

	$ crontab -l
	no crontab for zirka


# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un article d'Ubuntu-fr sur le sujet](https://doc.ubuntu-fr.org/cron) (FR)
- [Une liste d'exemples](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/)
- [Un article sur la vérification](http://bencane.com/2011/11/02/did-my-cronjob-run/)
