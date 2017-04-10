+++
title      = "LFCS : Navigation dans un système Linux"
date       = "2017-02-14T16:14:27+01:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

Maintenant que l'utilisation de la ligne de commande n'a plus de secret pour nous, il est temps de découvrir l'arborescence d'un système Linux et apprendre comment s'y orienter et naviguer dedans.

<!-- Avant toute chose, je recommande d'utiliser une Sandbox (un espace de test dédié et isolé) pour réaliser les exercices et manipulations de cette série. Comme nous allons être amenés à réaliser des manipulations plus ou moins critiques et à altérer le fonctionnement du système, le faire depuis son environnement de tous les jours peut s'avérer risqué si vous ne savez pas exactement ce que vous faites. -->

<div class="warning">Il s'agit ici d'une introduction destinée aux débutants. Pour ceux d'entre vous déjà familiers avec la ligne de commande, vous y apprendrez peut-être quelque chose, ou peut-être pas, mais dans tous les cas vous pouvez passer sans scrupules à un article plus avancé.</div>

# L'arborescence

Linux repose sur une arborescence pour organiser les fichiers au sein du système[^1]. Chaque fichier ou dossier se situe donc virtuellement dans une succession de répertoires, remontant jusqu'à la racine du système de fichiers (dénommée " / "): c'est ce qu'on appelle le **chemin absolu**. Par simplicité, cette arborescence est souvent représentée comme un arbre inversé, avec la racine au sommet. Le dossier dans lequel vous vous trouvez est quand à lui appelé **dossier courant** (ou "Working directory"), et il est aussi possible de représenter un chemin à partir de cet endroit: c'est alors un **chemin relatif**.



Dans cet exemple, le <span style="color:blue">chemin absolu vers le dossier "rainbow"</span> est " _/home/zirka/somewhere/over/rainbow_ " . Comme le <span style="color:magenta">dossier courant</span> est "zirka" (dont le chemin absolu est " */home/zirka* "), le <span style="color:red">chemin relatif vers le dossier "*rainbow*"</span> est " *somewhere/over/rainbow* " .

[^1]: Cette arborescence suit elle même une norme (le "Filesystems Hierarchy Standard", FHS), que nous passerons au crible dans un article dédié.

## S'orienter et se déplacer

Dans la pratique, un certain nombre de commandes nous permettent de nous orienter et nous déplacer dans le système de fichier, en ligne de commande. Commençons par découvrir l'arborescence :

 - **pwd** (pour "*Print Working Directory*") : retourne le chemin absolu du dossier courant
 - **ls** (pour "*LiSt directory content*") : liste les fichiers & dossiers situés dans le dossier courant
 - **tree** : retourne une vue graphique de l'arborescence à partir du chemin courant

Pour toutes ces commandes, il est aussi possible de passer en argument un chemin donné, pour que l'opération soit effectuée à partir de ce dossier.

Au passage, il est possible avec ls et bien d'autres commandes (find, file, etc.) de donner des **motifs "abstraits"** plutôt qu'un nom précis [^1]. Sans entrer dans les détails, il est possible d'utiliser le caractère __*__ pour désigner "tout". Par exemple, la commande " _ls *.bin_ " listera tous les fichiers dont le nom finit par _.bin_ .

Enfin, c'est la commande **cd** (pour "*Change Directory*") qui nous permet de nous déplacer. Si elle n'est pas complétée d'un argument, elle nous amènera directement au répertoire par défaut. Le plus souvent, il s'agit du répertoire personnel de l'utilisateur courant, dont le chemin par défaut est " */home/username* " avec *username* l'identifiant de l'utilisateur. Il est aussi possible de donner à *cd* un chemin absolu ou relatif au dossier courant pour s'y déplacer.

À noter qu'il existe aussi certains raccourcis, comme :

 - " *~* " pour le répertoire personnel de l'utilisateur courant
 - " *.* " pour le répertoire courant
 - " *..* " pour le répertoire parent (celui qui contient le dossier courant)
 - " *-* " pour le répertoire précédent (celui où vous étiez avant le dernier déplacement)

Cela nous donne donc via le terminal (les commandes que nous entrons sont ici précédées du symbole "$") :

	$ pwd
	/home/zirka

	$ tree 
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	$ ls
	somewhere

	$ ls somewhere/over/rainbow
	high.c  up.mk  way.c

	$ ls ~/somewhere/over/rainbow
	high.c  up.mk  way.c

	$ cd /home/zirka/somewhere/over/rainbow
	$ ls
	high.c  up.mk  way.c

	$ ls *.c
	high.c  way.c


<!-- <span style="color:red">!!! **Important !!!**</span> Lorsque vous avez un doute sur l'utilisation d'une commande, la commande **man** suivie du nom de celle recherchée (e.g. *man pwd* ) vous affichera une page d'aide, avec son utilisation, ses options, etc. -->

## Créer et détruire
Nous avons vu comment naviguer dans l'arborescence, reste maintenant à créer, déplacer, éditer et détruire des fichiers. Commencons par la création et le déplacement. Cela se passe avec les commandes suivantes :

 - **mkdir** (pour "*Make Directory*") : permet de créer un répertoire avec pour chemin celui donné en argument
 - **touch** : crée un fichier avec pour chemin celui donné en argument [^2]
 - **cp** (pour "*Copy*") : permet de copier un fichier d'un chemin vers un autre.
  - *-r* : avec l'option " *-r* " (pour recursive), copie un dossier et tout son contenu.
 - **mv** (pour "*Move*") : déplace un fichier d'un chemin vers un autre. À noter que mv peut aussi être utilisé pour renommer un fichier/dossier.
  - *-r* : avec l'option " *-r* ", permet de déplacer un dossier et tout son contenu.

[^2]: En fait, " *touch* " permet de changer les dates d'un fichier (création, modification, etc.). Mais si ce fichier n'existe pas, il va le créer. À ma connaissance, il n'existe pas de commande dédiée pour créer un fichier.

Cela nous donne donc via le terminal :

	$ tree
	.
	├── config.xml
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── high.c
	│           ├── up.mk
	│           └── way.c

	$ mkdir highway
	$ mkdir highway/to
	$ touch highway/to/hell.txt
	$ cp highway/to/hell.txt somewhere/over/rainbow
	$ mv somewhere/over/rainbow/up.mk highway/to/hell.txt

	$ tree
	.
	├── config.xml
	├── highway
	│   └── to
	│       ├── hell.txt
	│       └── up.mk
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── hell.txt
	│           ├── high.c
	│           └── way.c

Voyons maintenant comment détruire des fichiers ou dossiers. Pour cela rien de plus simple, avec les commandes suivantes :

 - **rm** (pour "*Remove*") : supprime définitivement un fichier via le chemin passé en argument.
  - *-r* : avec l'option *-r*, efface un dossier et tout son contenu
 - **rmdir** (pour "*Remove Directory*") : efface définitivement un dossier via le chemin passé en argument.

En pratique, cela nous donne :

	$ tree
	.
	├── config.xml
	├── highway
	│   └── to
	│       ├── hell.txt
	│       └── up.mk
	├── somewhere
	│   └── over
	│       └── rainbow
	│           ├── hell.txt
	│           ├── high.c
	│           └── way.c

	$ rm highway/to/hell.txt
	$ rm highway/to/up.mk
	$ rmdir highway/to
	$ rm -r somewhere/over
	$ touch highway/to/hell.txt
	$ cp highway/to/hell.txt somewhere/over/rainbow
	$ mv somewhere/over/rainbow/up.mk highway/to/hell.txt

	$ tree
	.
	├── config.xml
	└── somewhere

# Conclusion

Nous arrivons maintenant à la fin de notre premier acticle d'une longue série dédiée aux certifications de la fondation Linux. Nous avons abordé une première partie des commandes essentielles à connaître pour utiliser le terminal, en passant en revue les moyens de se repérer, créer et détruire dans l'arborescence Linux. Le prochain article complètera ce thème en abordant la question des liens, de la manipulation de fichiers, du manuel, et bien plus encore.



<!--     Command Line

x   1. The Shell
x   2. pwd (Print Working Directory)
x   3. cd (Change Directory)
x   4. ls (List Directories)
x   5. touch
    6. file
    7. cat
    8. less
    9. history
x   10. cp (Copy)
x   11. mv (Move)
x   12. mkdir (Make Directory)
x   13. rm (Remove)
    14. find
    15. help
    16. man
    17. whatis
    18. alias
    19. exit

 -->