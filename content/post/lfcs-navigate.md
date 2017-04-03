+++
title      = "LFCS : Navigation dans un système Linux"
date       = "2017-02-14T16:14:27+01:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
+++

Pour bien débuter ma série d'articles concernant Linux, son fonctionnement et son administration, je vais passer en revue les commandes de bases nécessaires pour s'orienter et naviguer au sein du système de fichiers. Je parle bien de "commandes", car ici comme dans toute la suite, nous passerons par la ligne de commande.

Avant toute chose, je recommande d'utiliser une Sandbox (un espace de test dédié et isolé) pour réaliser les exercices et manipulations de cette série. Comme nous allons être amenés à réaliser des manipulations plus ou moins critiques et à altérer le fonctionnement du système, le faire depuis son environnement de tous les jours peut s'avérer risqué si vous ne savez pas exactement ce que vous faites.

Ceci étant dit, plongeons nous dans cette fameuse ligne de commande, et voyons commant nous repérer et naviguer dans un système Linux. 

<div class="warning">Il s'agit ici d'une introduction destinée aux débutants. Pour ceux d'entre vous déjà familiers avec la ligne de commande, vous n'apprendrez probablement rien de nouveau dans cet article.</div>

# L'interpréteur de commandes

L'interpréteur de commandes (aussi appelé "ligne de commande") permet d'accéder aux fonctions propres aux système d'exploitation par le biais de commandes données en entrée (dans notre cas, tapées au clavier). Il existe de nombreux interpréteurs, les plus courant étant le Bourne Shell (*sh*), le Bourne-Again Shell (*bash*), le Z shell (*zsh*), etc., le plus répendu actuellement (et par défaut sur de nombreuses distributions de Linux) étant bash.

En mode graphique, la ligne de commande est accessible sur les distributions Linux par défaut via les applications dédiées ("Console", "Terminal", etc.,). Avec l'expérience il est souvent bien plus simple et rapide de gérer son système Linux de cette manière que graphiquement. Dans le cas des serveurs, qui n'ont pas de mode graphique, il s'agit même d'un passage obligé, alors autant s'y mettre dès maintenant.


# L'arborescence

Linux repose sur une arborescence pour organiser les fichiers au sein du système[^1]. Chaque fichier ou dossier se situe donc virtuellement dans une succession de répertoires, remontant jusqu'à la racine du système de fichiers (dénommée " / "): c'est ce qu'on appelle le **chemin absolu**. Par simplicité, cette arborescence est souvent représentée comme un arbre inversé, avec la racine au sommet. Le dossier dans lequel vous vous trouvez est quand à lui appelé **dossier courant** (ou "Working directory"), et il est aussi possible de représenter un chemin à partir de cet endroit: c'est alors un **chemin relatif**.



Dans cet exemple, le <span style="color:blue">chemin absolu vers le dossier "rainbow"</span> est " _/home/zirka/somewhere/over/rainbow_ " . Comme le <span style="color:magenta">dossier courant</span> est "zirka" (dont le chemin absolu est " */home/zirka* "), le <span style="color:red">chemin relatif vers le dossier "*rainbow*"</span> est " *somewhere/over/rainbow* " .

[^1]: Cette arborescence suit elle même une norme (le "Filesystems Hierarchy Standard", FHS), que nous verrons plus en détail dans un article dédié.

# S'orienter et se déplacer

Dans la pratique, un certain nombre de commandes nous permettent de nous orienter et nous déplacer dans le système de fichier, en ligne de commande. Commençons par découvrir l'arborescence :

 - **pwd** (pour "*Print Working Directory*") : retourne le chemin absolu du dossier courant
 - **ls** (pour "*LiSt directory content*") : liste les fichiers & dossiers situés dans le dossier courant
 - **tree** : retourne une vue graphique de l'arborescence à partir du chemin courant

Pour toutes ces commandes, il est aussi possible de passer en argument un chemin donné, pour que l'opération soit effectuée à partir de ce dossier.

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
	high  up  way

	$ ls ~/somewhere/over/rainbow
	high  up  way

	$ cd /home/zirka/somewhere/over/rainbow
	$ ls
	high  up  way

<!-- <span style="color:red">!!! **Important !!!**</span> Lorsque vous avez un doute sur l'utilisation d'une commande, la commande **man** suivie du nom de celle recherchée (e.g. *man pwd* ) vous affichera une page d'aide, avec son utilisation, ses options, etc. -->

# Créer et détruire
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