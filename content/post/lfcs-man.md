+++
title      = "LFCS : La documentation passée au crible"
date       = "2017-05-22T09:21:55+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô û -->

Aussi expérimenté que vous soyez, il est simplement impossible de retenir toutes les commandes disponibles sous Unix/Linux, leur rôle, leur fonctionnement, leurs paramètres et leurs arguments. Heureusement, il existe une documentation simple d'accès en ligne de commande et très fournie : le manuel. Aujourd'hui, nous allons donc nous plonger dans ce sujet 

# Le manuel en ligne de commande

La commande de référence pour consulter une entrée du manuel est **man**, suivie de la commande pour laquelle vous avez besoin d'informations[^1]. Une page s'ouvrira alors dans le terminal pour afficher toutes les informations essentielles sur cette commande[^2] : son rôle, son fonctionnement, ses options et paramètres, ... (Pour une description en une seule ligne, utiliser la commande **whatis**).

Info alternative à man, ...

[^1]: Il existe une alternative à _man_, **info**, qui sert de principale outil de documentation pour les Systèmes basés sur [GNU](https://en.wikipedia.org/wiki/GNU). Cependant, bien que mieux organisée (avec des hyperliens par exemple), _info_ est souvent moins bien fournie.

[^2]: Si plusieurs entrées existent pour cette commande, il n'affichera que la première.

Il est aussi possible de chercher des commandes à partir d'un mot clé, avec la commande **apropos**. Celle-ci retourne toutes les commandes (et leur description) pour lesquelles le mot-clé est présent dans leur nom ou descriptions. Par exemple, pour cherchées toutes les commandes liées à l'algorithme de compression bzip2 : 

	$ apropos bzip
	bzcmp (1)            - compare bzip2 compressed files
	bzdiff (1)           - compare bzip2 compressed files
	bzegrep (1)          - search possibly bzip2 compressed files for a regular expression
	bzfgrep (1)          - search possibly bzip2 compressed files for a regular expression
	bzgrep (1)           - search possibly bzip2 compressed files for a regular expression
	bzip2 (1)            - a block-sorting file compressor, v1.0.6
	bzip2recover (1)     - recovers data from damaged bzip2 files
	bzless (1)           - file perusal filter for crt viewing of bzip2 compressed text
	bzmore (1)           - file perusal filter for crt viewing of bzip2 compressed text

La commande _man_ n'est qu'un outil de visualisation des pages de manuel, qui ne contient pas lui-même toutes les descriptions disponibles (appelées _entrées_). En fait, _man_ consulte une base de données d'indexation des pages de manuel, créée ou mise-à-jour avec la commande **mandb**. Cette base de données est construite à partir des emplacements et options spécifiés dans le fichier **/etc/manpath.config** (à consulter pour mieux comprendre le fonctionnement du manuel).

# Créer sa propre entrée de manuel

Il est bien sûr possible de créer de toute pièce une entrée de manuel (par exemple, pour une commande "_maison_" que vous avez créée). Les entrées de manuel suivent un format spécique, permettant à un système de traitement de documents nommé **troff** (ou plutôt à l'un de ses packages, _groff an.tmac_) d'en déduire la mise-en-page du document. Ce format fonctionne par l'intermédiaire de _Macros_ définissant les sections, les en-tête et pieds de page, etc.

Pour comprendre comment fonctionne le formatage d'une entrée de documentation, le mieux est encore d'en lire une. Succintement, voici quelques macros utilisées :

- **NAME** : Le nom de la commande ou fonction, suivi d'une description en une ligne de son fonctionnement
- **SYNOPSIS** : Une description plus complète de son fonctionnement et des options disponibles
- **EXAMPLES** : Quelques exemples de son utilisation générale
- **BUGS** : Les bugs connus liés à cette commande
- **SEE ALSO**, **AUTHOR**, **COPYRIGHT**, **DESCRIPTION**, ...

Une fois la documentation réalisée, il suffit de la placer dans un des emplacements définis dans _/etc/manpath.config_, de recréer la base de données d'indexation avec la commande _mandb_, et le tour est joué.

<!-- https://www.cyberciti.biz/faq/linux-unix-creating-a-manpage/ -->



# Références sur Internet

Le manuel Linux est incroyablement bien fourni et vous aidra à trouver la réponse à une grande partie de vos questions. Mais pour toutes les autres, il reste à se tourner vers Internet. De manière générale, le [**wiki d'Archlinux**](https://wiki.archlinux.org) est une mine d'or en matière d'information et de résolution de problèmes. Une excellente alternative est la [**documentation en ligne de Red Hat**](https://access.redhat.com/documentation), ou encore le [**Projet de Documentation Linux (TLDP)**](http://www.tldp.org/). Enfin, le forum [**stackexchange**](https://stackexchange.com/), et en particulier son groupe [**serverfault**](https://serverfault.com/), permet de résoudre une vaste majorité des problèmes rencontrés dans la vie de tous les jours avec Linux.

<!-- https://www.youtube.com/watch?v=59jnWX_EzTY&index=123&list=WL -->

