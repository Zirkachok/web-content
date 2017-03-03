+++
title      = "LFCS : Navigation dans un système Linux"
date       = "2017-02-14T16:14:27+01:00"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++

<!-- Avant même de commencer notre série d'articles concernant Linux, son fonctionnement et son administration, nous allons passer en revue les bases de Linux qui vous seront nécessaires pour vous exercer en suivant mes tutoriels.


# Mettre en place une Sandbox

Au cours de nos tests et exercices, nous allons donc être amenés à réaliser des manipulations plus ou moins critiques et à altérer le fonctionnement du système. Pour ne citer que quelques exemples, nous verrons comment modifier des paramètres système et réseau, changer des droits, créer et supprimer des utilisateurs, installer un nouveau noyau, etc. Il n'est donc pas envisageable de faire ces manipulations directement depuis son environnement de tous les jours. Une autre raison pour laquelle il est préférable de réaliser ses tests sur un autre système est d'éviter que le système lui-même (ses configurations courantes, les droits affectés, les installations réalisées) n'altère le résultat de nos tests. Donc, pour faire les choses proprement, je vais avant toutes choses vous proposer quelques solutions pour mettre en place un espace de test dédié et isolé : une Sandbox.

P -->

<!-- ## Virtualisation vs. IaaS -->

<!-- Console/Terminal -->

<!-- # Navigation dans un système Linux -->

Pour bien débuter notre série d'articles concernant Linux, son fonctionnement et son administration, nous allons passer en revue les bases de Linux qui vous seront nécessaires pour vous orienter et naviguer au sein du système de fichiers.

# Chemins absolus et relatifs

Linux repose sur une arborescence pour organiser les fichiers au sein du système[^1]. Chaque fichier ou dossier se situe donc virtuellement dans une succession de répertoires, remontant jusqu'à la racine du système de fichiers (dénommée " / "): c'est ce qu'on appelle le **chemin absolu**. Par simplicité, cette arborescence est souvent représentée comme un arbre inversé, avec la racine au sommet. Le dossier dans lequel vous vous trouvez est quand à lui appelé **dossier courant** (ou "Working directory", et il est aussi possible de représenter un chemin à partir de cet endroit: c'est alors un **chemin relatif**.

Dans cet exemple, le <span style="color:blue">chemin absolu vers le dossier "rainbow"</span> est " _/home/zirka/somewhere/over/rainbow_ " . Comme le <span style="color:magenta">dossier courant</span> est "zirka" (dont le chemin absolu est " */home/zirka* "), le <span style="color:red">chemin relatif vers le dossier "*rainbow*"</span> est " *somewhere/over/rainbow* " .

[^1]: Cette arborescence suit elle même une norme (le "Filesystems Hierarchy Standard", FHS), que nous verrons plus en détail dans un article dédié.

Dans la pratique, un certain nombre de commandes nous permettent de nous orienter et nous déplacer dans le système de fichier, en ligne de commande. Commençons par découvrir l'arborescence :

 - **pwd** (pour "*Print Working Directory*") : retourne le chemin absolu du dossier courant
 - **ls** (pour "*LiSt directory content*") : liste les fichiers & dossiers situés dans le dossier courant
 - **tree** : retourne une vue graphique de l'arborescence à partir du chemin courant

Pour toutes ces commandes, il est aussi possible de passer en argument un chemin donné, pour que l'opération soit effectuée sur ce dossier.

Enfin, c'est la commande **cd** (pour "Change Directory") qui nous permet de nous déplacer. Si elle n'est pas complétée d'un argument, elle nous amènera directement au répertoire par défaut. Le plus souvent, il s'agit du répertoire personnel de l'utilisateur courant, dont le chemin par défaut est " */home/username* " avec *username* l'identifiant de l'utilisateur. Il est aussi possible de donner à *cd* un chemin absolu ou relatif au dossier courant pour s'y déplacer.

À noter que le répertoire personnel de l'utilisateur courant peut être abrégé par " *~* " dans le système, le dossier courant par " *.* ", et le dossier parent (celui qui contient le dossier courant) par " *..* ".

Cela nous donne donc via le terminal :

   	zirka@Epinet:~$ pwd
	/home/zirka

	zirka@Epinet:~$ tree 
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	zirka@Epinet:~$ ls
	somewhere

	zirka@Epinet:~$ ls somewhere/over/rainbow
	high  up  way

	zirka@Epinet:~$ ls ~/somewhere/over/rainbow
	high  up  way

	zirka@Epinet:~$ cd /home/zirka/somewhere/over/rainbow
	zirka@Epinet:~/somewhere/over/rainbow$ ls
	high  up  way

<!-- <span style="color:red">!!! **Important !!!**</span> Lorsque vous avez un doute sur l'utilisation d'une commande, la commande **man** suivie du nom de celle recherchée (e.g. *man pwd* ) vous affichera une page d'aide, avec son utilisation, ses options, etc. -->

# Créer et détruire
Nous avons vu comment naviguer dans l'arborescence, reste maintenant à créer, déplacer, éditer et détruire des fichiers. Commencons par la création et le déplacement. Cela se passe avec les commandes suivantes :

 - **mkdir** : permet de créer un répertoire avec pour chemin celui donné en argument
 - **touch** : crée un fichier avec pour chemin celui donné en argument [^2]
 - **cp** : permet de copier un fichier d'un chemin vers un autre.
  - *-r* : avec l'option " *-r* " (pour recursive), copie un dossier et tout son contenu.
 - **mv** : déplace un fichier d'un chemin vers un autre. À noter que mv peut aussi être utilisé pour renommer un fichier/dossier.
  - *-r* : avec l'option " *-r* ", permet de déplacer un dossier et tout son contenu.

[^2]: En fait, " *touch* " permet de changer les dates d'un fichier (création, modification, etc.). Mais si ce fichier n'existe pas, il va le créer. À ma connaissance, il n'existe pas de commande dédiée pour créer un fichier.

Cela nous donne donc via le terminal :

	zirka@Epinet:~$ tree
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	zirka@Epinet:~$ mkdir highway
	zirka@Epinet:~$ mkdir highway/to
	zirka@Epinet:~$ touch highway/to/hell.txt
	zirka@Epinet:~$ cp highway/to/hell.txt somewhere/over/rainbow
	zirka@Epinet:~$ mv somewhere/over/rainbow/up.mk highway/to/hell.txt

	zirka@Epinet:~$ tree
	.
	├── config.xml
	├── highway
	│   └── to
	│       ├── hell.txt
	│       └── up.mk
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── hell.txt
	│           ├── high.c
	│           └── way.c

Voyons maintenant comment détruire des fichiers ou dossiers. Pour cela rien de plus simple, avec les commandes suivantes :

 - **rm** : supprime définitivement un fichier via le chemin passé en argument.
  - *-r* : avec l'option *-r*, efface un dossier et tout son contenu
 - **rmdir** : efface définitivement un dossier via le chemin passé en argument.

En pratique, cela nous donne :

	zirka@Epinet:~$ tree
	.
	├── config.xml
	├── highway
	│   └── to
	│       ├── hell.txt
	│       └── up.mk
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── hell.txt
	│           ├── high.c
	│           └── way.c

	zirka@Epinet:~$ rm highway/to/hell.txt
	zirka@Epinet:~$ rm highway/to/up.mk
	zirka@Epinet:~$ rmdir highway/to
	zirka@Epinet:~$ rm -r somewhere/over
	zirka@Epinet:~$ touch highway/to/hell.txt
	zirka@Epinet:~$ cp highway/to/hell.txt somewhere/over/rainbow
	zirka@Epinet:~$ mv somewhere/over/rainbow/up.mk highway/to/hell.txt

	zirka@Epinet:~$ tree
	.
	├── config.xml
	└── somewhere

# Conclusion

Nous avons maintenant vu comment se repérer et déplacer dans le système de fichiers Linux, ainsi que comment créer, détruire, copier et déplacer des fichiers et répertoires, terminant ainsi notre premier article d'une longue série dédiée aux certifications de la fondation Linux. Le prochain article sera quand à lui dédié aux droits et permissions.
