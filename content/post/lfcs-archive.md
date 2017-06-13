+++
title      = "LFCS : Archivage et compression"
date       = "2017-06-06T11:06:04+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Aucun système, qu'il soit pour un ordinateur personnel ou pour un réseau d'entreprise avec de nombreux noeuds et utilisateurs, n'est a l'abri d'une défaillance. Un disque dur peut tomber en panne, le système peut être compromis, un administrateur ou utilisateur peut faire une erreur, etc., et il est alors nécessaire de revenir en arrière, à un point de sauvegarde (**snapshot**) sûr. Il est donc indispensable de mettre en place des sauvegardes régulières (**backup**) de l'état du système, et d'être capable d'y basculer le cas échéant (**restore**). Nous allons donc aujourd'hui voir comment archiver simplement et efficacement des données, et comment programmer des _backup_ réguliers de votre système.  

Les archives peut être vu comme des paquets contenant des fichiers et répertoires, et servent à conserver facilement des données dans un unique fichier. Par défaut, une archive resulte d'une simple concaténation des fichiers, avec quelques éventuelles informations sur la hiérarchie des fichiers, leurs droits, etc. Une archive peut cependant aussi être compressée, c'est-à-dire qu'un algorithme sera appliqué sur les fichiers pour les compresser (i.e. en réduire la taille). Nous allons voir ici les méthodes les plus courantes pour créer une archive, compressée ou non.

# Archiver avec _cpio_

La commande **cpio** (pour _copy in and out_) date des débuts d'UNIX, et était prévue pour la sauvegarde de données sur des bandes magnétiques. Bien que d'autres commandes d'archivage aient vu le jour, _cpio_ reste présent dans la quasi-totalité des systèmes UNIX/Linux, notamment du fait de sa légèreté.

_cpio_ permet d'archiver, extraire et lister le contenu d'archives, avec les options suivantes :

- **--create** (ou -o) : Crée une archive
- **--extract** (ou -i) : Extrait le contenu d'une archive
- **--list** (ou -t) : Liste le contenu d'une archive

Le contenu à archiver peut être spécifié avec l'option **-I** ou une redirection de la ligne de commande, et l'archive en sortie peut être spécifiée avec l'option **-O** ou également une redirection. Par l'exemple, cela nous donne :

	$ tree 
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	$ find . -depth | cpio -o > test.cpio

	$ ls
	config.xml 	somewhere 	test.cpio

	$ cat test.cpio | cpio -t
	somewhere/over/rainbow/high.c
	somewhere/over/rainbow/up.mk
	somewhere/over/rainbow
	somewhere/over
	somewhere
	config.xml
	test.cpio
	.
	13 blocs

# Archiver et compresser avec _tar_

La commande **tar** permet aussi l'archivage, mais est plus récente et plus simple d'utilisation que _cpio_. Elle gère par défaut des archives du même nom (aussi appelées tarballs), avec les options suivantes :

- **--create** ou **-c** : Permet de créer une nouvelle archive à partir des fichiers donnés en paramètre.
- **--extract** ou **-x** : Permet d'extraire le contenu d'une archive.
- **--list** ou **-t** : Liste le contenu d'une archive.
- **--file ARCH** ou **-f ARCH** : Désigne le fichier (archive, device, ...) à utiliser pour réaliser l'opération choisie.

Cela nous donne par exemple :

**Archivage**

	$ tree
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	$ tar -cf test.tar somewhere

	$ tar -tf test.tar
	somewhere/
	somewhere/over/
	somewhere/over/rainbow/
	somewhere/over/rainbow/high.c
	somewhere/over/rainbow/up.mk
	somewhere/over/rainbow/way.c

	$ rm -r somewhere

	$ tree
	.
	├── config.xml
	└── test.tar

	$ tar -xf test.tar

	$ tree
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c


_tar_ a aussi d'autres atouts dans sa manches, qui rendent son utilisation particulièrement intéressante pour les _backup_. Il est par exemple possible :

- de n'ajouter que les fichiers plus récents qu'une certaine date avec l'option **--newer DATE** (ou **-N DATE**)
- de n'ajouter que les fichiers plus récents que ceux contenus dans l'archive avec l'option **--update** (ou **-u**)
- d'exclure les fichiers dont le nom suit un certain motif avec l'option **--exclude=MOTIF** ou **-X=MOTIF**
- de compresser les données, par exemple avec l'option **--gzip** (ou **-z**)
- d'obtenir les différences entre le contenu de l'archive et le contenu sur le disque, avec l'option **--diff** (ou **-d**)
- ...


# Archivage et backup

J'ai effleuré la question en introduction, il est donc temps de rentrer dans le vif du sujet : l'archivage (ici, avec _tar_) est largement utilisé pour effectuer du _backup_. Il existe principalement 2 méthodes de backup : le **backup complet**, où un archivage des données est réalisé régulièrement, le **backup incrémental**, où seules les différences par rapport au dernier archivage sont conservées. Bien sûr, les _backup complets_ sont plus faciles à récupérer (pour un backup incrémental, on doit revenir à la dernière archive complète et appliquer un à un les archives incrémentales), mais les _backup incrémentaux_ sont évidemment plus légers, un juste équilibre des deux est donc souvent la meilleure approche.

_tar_ est parfaitement adapté aux _backup complets_ (avec notamment la compression), mais aussi aux _backup incrémentaux_ (avec les options _newer_, _update_, _diff_, ...). Il comporte d'ailleurs une option spécifique pour ces derniers (utlisée pour l'archivage comme pour l'extraction) : **--listed-incremental=SNAP** (ou **-g SNAP**), avec _SNAP_ un fichier de travail pour réaliser cette opération (le même doit être utilisé pour chaque prochain backup incrémental).

Cela nous donne :

	$ tree
	├── somewhere
	│   └── over
	│       ├── 
	│       └── rainbow bigfile.txt
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	$ ls -al somewhere
	total 204816
	drwxrwxr-x 3 julien julien      4096 juin  13 11:54 .
	drwxrwxr-x 3 julien julien      4096 juin  13 11:54 ..
	-rw-rw-r-- 1 julien julien 209715200 juin  13 11:54 bigfile.txt
	drwxrwxr-x 3 julien julien      4096 juin   7 11:09 over

	$ tar --create --file=increm1.tar --listed-incremental=save.list somewhere/

	$ ls -al increm1.tar
	-rw-rw-r-- 1 julien julien 209725440 juin  13 11:56 increm1.tar

	$ echo "Hello there" > somewhere/over/rainbow/way.c

	$ tar --create --file=increm2.tar --listed-incremental=save.list somewhere/
	-rw-rw-r-- 1 julien julien 10240 juin  13 11:58 increm2.tar

Comme on peut le voir, la première itération de _tar_ pour notre backup incrémental crée une archive volumineuse, alors que la seconde est extrèmement légère, car peu de contenu a été changé entre temps.

Nous avons vu un des premiers aspects du _backup_, l'archivage et l'extraction. Reste qu'il est fastidieux (et dangereux...) de faire ces opérations à la main, régulièrement. Nous verrons donc dans un prochain article comment automatiser ces tâches.



<!-- tar is easier to use than cpio:

    When creating a tar archive, for each directory given as an argument, all files and subdirectories will be included in the archive.
    When restoring it reconstitutes directories as necessary.
    It even has a --newer option that lets you do incremental backups.
    The version of tar used in Linux can also handle backups that do not fit on one tape or whatever device you use.


Below are a few example of how to use tar for backups:



    Create an archive using -c or --create:

    $ tar --create --file /dev/st0 /root
    $ tar -cvf /dev/st0 /root

    You can specify a device or file with the -f or --file options.
     
    Create with multi volume option using -M or --multi-volume if your backup won't fit on one device:

    $ tar -cMf /dev/st0 /root

    You will be prompted to put next tape when needed.
     
    Verify files with compare option using -d or --compare:

    $ tar --compare --verbose --file /dev/st0
    $ tar -dvf /dev/st0

    After you make a backup, you can make sure that it is complete and correct using the above verification option. -->
