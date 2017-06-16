+++
title      = "LFCS : Limiter les ressources système"
date       = "2017-06-14T13:31:54+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Nous avons déjà vu comment limiter, pour chaque système de fichier, le volume de données et le nombre de fichiers qu'un utilisateur ou groupe a à sa disposition. Aujourd'hui, nous allons explorer un peu plus la question en limitant cette fois-ci les ressources système disponibles pour un utilisateur. 

L'utilisation de ces ressources, telles que la taille maximale des fichiers, le nombre maximal de fichiers ouverts, le nombre de processus, etc., sont autant de failles courantes dans les systèmes multi-utilisateurs. Imaginons un tel système sans limites : Tout utilisateur peut alors créer des processus à l'infini[^1], ouvrir autant de fichiers que possible, etc., rendant de fait le système inutilisable. Il est donc essentiel par sécurité d'introduire des limites strictes à ce niveau, comme pour les quotas d'utilisation disque.

[^1]: C'est ce que l'on appelle une [fork bomb](https://www.admin-linux.fr/?p=7530).

Linux permet d'y parvenir facilement, via le fichier **/etc/security/limits.conf**. Ce fichier est chargé à l'ouverture d'une session dans le système par _PAM_[^2], et définit l'ensemble des limites apposées pour un grand nombre de paramètres système, à chaque utilisateur. Pour changer ces paramètres, plutôt que d'écrire directement dans le fichier, nous utiliserons la commande **ulimit** avec les options suivantes (entre autres) :

- **-a** : Affiche les limites pour chaque ressource système concernée
- **-f** : Affiche (si seul) ou fixe (si suivi d'un nombre) la **taille maximale d'un fichier** créé
- **-n** : Affiche ou fixe le **nombre maximum de fichiers ouverts** simultanément
- **-t** : Affiche ou fixe le **temps CPU maximal** pour un processus (i.e. son utilisation des ressources processeur)
- **-u** : Affiche ou fixe le **nombre maximal de processus** simultanément
- **-v** : Affiche ou fixe la **quantité maximale de mémoire virtuelle** disponible pour un processus

Comme pour les quotas disque, _ulimit_ gère des limites strictes (**hard limits**) avec l'option **-H**, et non strictes (**soft limits**) avec l'option **-S**[^3]. Sans aucune de ces deux options, les deux types de limites sont motifiées simultanément.

[^2]: Nous aborderons **PAM** (_Pluggable Authentication Modules_) dans un prochain article.
[^3]: À noter que la limite stricte doit forcément être supérieure à la limite non-stricte.

D'autres limites ne sont modifiables qu'en écrivant directement dans le fichier _/etc/security/limits.conf_. On retrouve entre autres les champs :

- maxlogins : nombre maximal de connections en simultané à chaque utilisateur (le superutilisateur n'étant pas concerné)
- maxsyslogins : nombre maximal d'utilisateurs connectés en simultané au système
- nice : priorité maximale des processus de l’utilisateur
- ...

Le fichier _/etc/security/limits.conf_ permet aussi une gestion fine des limites, en fonction des utilisateurs et groupes (faire varier les limites en fonction des utilisateurs, par exemple).

Dans la pratique, cela nous donne :

	$ ulimit -u
	30353

	$ ulimit -u 300

	$ ulimit -u
	300

	$ ulimit -a
	core file size          (blocks, -c) 0
	data seg size           (kbytes, -d) 1000000
	scheduling priority             (-e) 0
	file size               (blocks, -f) unlimited
	pending signals                 (-i) 772466
	max locked memory       (kbytes, -l) 64
	max memory size         (kbytes, -m) 1000000
	open files                      (-n) 1024
	pipe size            (512 bytes, -p) 8
	POSIX message queues     (bytes, -q) 819200
	real-time priority              (-r) 0
	stack size              (kbytes, -s) 8192
	cpu time               (seconds, -t) unlimited
	max user processes              (-u) 30353
	virtual memory          (kbytes, -v) unlimited
	file locks                      (-x) unlimited


## Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- L'[entrée de manuel](https://ss64.com/bash/ulimit.html) de _limits.conf_ (**man limits.conf**)
- [Documentation Red Hat](https://access.redhat.com/solutions/61334) (EN)
- [Un excellent article sur la question](https://www.admin-linux.fr/controle-des-ressources-systemes-ulimit/) (FR)
- [Un résumé sur le sujet](http://www.linuxhowtos.org/Tips%20and%20Tricks/ulimit.htm) (EN)
- [Un bel article de Tecmint](https://www.tecmint.com/set-limits-on-user-processes-using-ulimit-in-linux/) (EN)
