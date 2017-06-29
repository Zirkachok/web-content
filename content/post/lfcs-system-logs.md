+++
title      = "LFCS : Les logs système"
date       = "2017-06-26T15:29:11+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

Que l'on gère son ordinateur ou l'ensemble des serveurs d'une entreprise, il est essentiel de savoir ce qui se passe sur le système et identifier les éventuels problèmes et leurs causes. Heureusement, Linux conserve un historique des derniers évènements majeurs survenus sur le système dans ce que l'on appelle des _fichiers de log_.

Lorsqu'arrive le moment de lire ces _logs_, par exemple suite à une défaillance du système ou à une perte de données, il est essentiel de savoir y trouver les informations essentielles rapidement et efficacement. Malheureusement, tous les _logs_ ne sont pas enregistrés au même endroit dans le système. En fonction de leur rôle et origine, ils peuvent se retrouver dans des fichiers différents, à des emplacements différents. Pour complexifier d'avantage les choses, ces fichiers de logs sont loin d'être de _simples_ fichiers texte, et il peut s'avérer difficile de les interpréter.

Aujourd'hui, nous allons donc découvrir comment localiser, configurer et exploiter ces _logs_ pour en tirer le meilleur parti.

# Localiser et exploiter les _logs_ sous Linux

## Connections et déconnections

Par défaut, les fichiers de log sont enregistrés dans le répertoire **/var/log**, dans de multiples fichiers. En particulier, **utmp** contient le registre des connections et déconnections au système (_log-in_/_log-out_), le temps de démarrage du système et son statut, etc. Le fichier **wtmp** contient quant à lui un historique des données de _utmp_, et **btmp** un registre des tentatives de connections échouées uniquement. 

Ces fichiers ne sont cependant pas lisibles directement. Pour en exploiter le contenu, il faut utiliser les commandes _last_ et _who. **last** renvoie une liste des derniers utilisateurs connectés au système, avec les dates de connections et de déconnections (lus à partir du fichier _wtmp_). Avec l'option **-f**, _last_ permet aussi de lire un fichier _wtmp_, _utmp_ ou _btmp_ donné en argument. La commande **who** quant à elle permet de retourner quels utilisateurs sont actuellement connectés au système. Enfin, la commande **lastlog** permet d'avoir la date de derniere connection pour chaque utilisateur du système.

La plupart des autres fichiers de log sont directement lisibles via votre lecteur favori (avec cat, head ou tail par exemple).

	$ last 
	zirka    tty7         :0               Mon Jun 26 08:26    gone - no logout
	reboot   system boot  3.13.0-93-generi Mon Jun 26 10:26   still running
	zirka    pts/6        :0.0             Thu Jun 22 11:50 - 15:49 (1+03:58)
	zirka    pts/5        :0.0             Mon Jun 19 10:56 - 15:49 (4+04:52)
	zirka    pts/4        :0.0             Mon Jun 19 10:47 - 15:49 (4+05:01)
	zirka    pts/1        :0.0             Mon Jun 19 10:46 - 15:49 (4+05:02)
	zirka    pts/0        :0.0             Mon Jun 19 08:38 - 15:49 (4+07:10)
	zirka    tty7         :0               Mon Jun 19 08:38 - down  (4+07:12)
	reboot   system boot  3.13.0-93-generi Mon Jun 19 10:32 - 15:50 (4+05:18)

	zirka    pts/13       :0.0             Fri Jun  2 09:42 - 15:59  (06:17)
	zirka    pts/3        :0.0             Fri Jun  2 09:03 - 15:59  (06:55)

	wtmp begins Fri Jun  2 09:03:42 2017

	$ sudo last -f /var/log/btmp
	root     ssh:notty    172.16.16.24     Tue Jun 27 11:30    gone - no logout
	zirka    tty8         :1               Mon Jun 26 14:09    gone - no logout
	root     ssh:notty    172.16.16.24     Mon Jun 26 11:30 - 11:30 (1+00:00)
	root     ssh:notty    172.16.16.24     Fri Jun 23 11:30 - 11:30 (2+23:59)
	zirka    tty8         :1               Fri Jun 23 08:36 - 14:09 (3+05:32)
	root     ssh:notty    172.16.16.24     Fri Jun  2 11:30 - 11:30 (5+00:00)
	zirka    tty8         :1               Fri Jun  2 09:48 - 15:15  (05:27)
	zirka    tty8         :1               Thu Jun  1 14:28 - 09:48  (19:19)
	root     ssh:notty    172.16.16.24     Thu Jun  1 11:30 - 11:30  (23:59)

	btmp begins Thu Jun  1 11:30:25 2017

	$ who
	zirka    tty7         2017-06-26 08:26 (:0)
	zirka    pts/0        2017-06-26 08:31 (:0.0)
	zirka    pts/1        2017-06-26 08:39 (:0.0)
	zirka    pts/4        2017-06-26 09:00 (:0.0)
	zirka    pts/5        2017-06-26 10:22 (:0.0)

	$ lastlog
	...
	lightdm                                    **Jamais connecté**
	colord                                     **Jamais connecté**
	hplip                                      **Jamais connecté**
	pulse                                      **Jamais connecté**
	zirka           pts/27   192.168.XX.XX     ven. oct. 14 16:27:44 +0200 2016
	jetty                                      **Jamais connecté**
	mysql                                      **Jamais connecté**
	systemd-timesync                           **Jamais connecté**
	systemd-network                            **Jamais connecté**
	systemd-resolve                            **Jamais connecté**
	systemd-bus-proxy                          **Jamais connecté**
	...

## Logs du noyau Linux

Il est aussi possible d'obtenir les évènements générés par le noyau Linux via la commande **dmesg** (pour "_display message_", ou la lecture du fichier **/var/log/dmesg). Ces logs sont particulièrement intéressants pour trouver les _drivers_ lancés, les périphériques présents et la manière dont ils ont été découverts, etc.


## Syslog

Le mécanisme d'historisation (_logs_) repose en grande partie sur un démon (ou _daemon_)[^1], nommé **rsyslog**. Son rôle est de récupérer les message de _log_ des différentes parties du système, et de les diriger vers le fichier de log approprié dans le répertoire _/var/log_, ou vers un autre serveur Linux. Ce démon prends sa configuration dans le fichier **/etc/rsyslog.conf**, qui nous renvoie vers un autre fichier contenant les paramètres par défaut (_/etc/rsyslog.d/50-default.conf_ sur Ubuntu), notamment une liste des fichiers dans lesquels sont écrits les messages de _logs_ en fonction de leur origine.

[^1]: Un démon un programme exécuté "en arrière plan" (voir [ici](https://fr.wikipedia.org/wiki/Daemon_(informatique)))

Chaque ligne de ce fichier comporte deux parties, l'une sur les évènements concernés (à gauche), et l'autre sur le fichier où les enregistrer. Les évènements sont eux même divisés en deux parties, à savoir leur origine et leur criticité.

Voici une liste des origines des évènement reconnus par Linux :

- **auth** ou **authpriv**: Messages provenant d'évènements liés à des authorisations et aux sécurités du système
- **kern** : Tout message provenant du noyau Linux
- **mail** : Messages générés par le sous-système mail
- **cron** : Messages liés aux tâches [_Cron_]({{< ref "lfcs-schedule-tasks.md" >}})
- **daemon** : Messages provenant des _démons_ du système
- **news** : Messages provenant du sous-système _network news_ coming from network news subsystem
- **lpr** : Messages liés à l'impression
- **user** : Messages provenant des programmes utilisateurs
- **local0** à **local7** : Réservés à une utilisation locale

Et pour les criticités, nous avons (par ordre croissant) : **debug**, **info**, **notice**, **warn**, **err**, **crit**, **alert**, **emerg**. À noter que lorsqu'une priorité est écrite dans une règle, elle signifie "toute criticité supérieure à".

Dans ces champs, le symbole "**\***" signifie **tout**, le symbole "**=**" après un point signifie **uniquement**, et le symbole "**!**" après un point signifie **tout sauf**. Différentes origines/crititictés peuvent êtres mises sur une même ligne avec le séparateur "**,**". De même, plusieurs règles peuvent êtres aggrégées avec le séparateur "**;**" Voici quelques exemples :

- _kern.*      /var/log/kern.log_ : log tous les évènements du noyau dans _/var/log/kern.log_
- _mail,news.err      /var/log/misc.err_ : log tous les évènements d'erreur ou supérieur de mail et news dans _/var/log/misc.err_
- _mail.=info      /var/log/mail.info_ : log tous les mails de criticité _info_ uniquement dans _/var/log/mail.info_
- _mail.!info      /var/log/mail.info_ : log tous les mails de criticité inférieure à info dans _/var/log/mail.info_
- _mail.!=info      /var/log/mail.info_ : log tous les mails de criticité autre que info dans _/var/log/mail.info_

Il est aussi possible d'utiliser **logrotate** plutôt que _rsyslog_. _logrotate_ écrit les logs systèmes dans les fichiers comme un buffer tournant en les compressant (les messages les plus anciens sont remplacés par les nouveaux), au lieu de constamment écrire au fur et à mesure. Le fichier de configuration de _logrotate_ est **/etc/logrotate.conf**

# Créer et tester ses propres logs

Nous avons passé en revue les moyens d'exploiter les logs du système efficacement. Cependant, lorsque l'on crée ses propres applications (ou son propre Linux de zéro, comme en _informatique embarquée_), il peut être essentiel de créer ses propres logs et ainsi garder traces de leur bon déroulement et des éventuels problèmes. Pour ce faire, nous allons utiliser les "origines" de messages **local0** à **local7** (voir plus haut), en ajoutant une entrée dans le fichier de configuration de _Syslog_ (_/etc/rsyslog.conf_). Ensuite, après avoir rechargé le fichier de configuration[^2], il nous suffira d'utiliser la commande **logger**, avec l'option **-p VAL** pour affecter l'origine et la criticité du message, suivis du message voulu.

[^2] : Cela peut se faire par un redémarrage du système entier, ou un redémarrage du service _rsyslog_ avec la commande **/etc/init.d/rsyslog restart**.

Cela nous donne par exemple :

	$ vim /etc/rsyslog.conf
	...
	local3.alert                                             /var/log/loc3testalert.log
	local3.=info                                            /var/log/loc3testinfo.log
	...

	$ /etc/init.d/rsyslog restart

	$ logger -p local3.info "Everything is going fine..."

	$ ls /var/log
	...
	loc3testalert.log
	loc3testinfo.log
	...

	$ cat loc3testinfo.log
	Jun  29 13:20:56 Epinet zirka:  Everything is going fine...


# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un article intéressant sur les logs de DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-and-centos) (EN)
- [Un autre, à propos de Logwatch](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-logwatch-log-analyzer-and-reporter-on-a-vps) (EN)
- [Un article dédié à wtmp, utmp et btmp](http://www.linuxnix.com/read-view-utmp-wtmp-btmp-file-linuxunix/)
- [La page wikipedia dédiée à syslog](https://en.wikipedia.org/wiki/Syslog)


<!-- â ê î ô û -->
<!-- é è ù à -->