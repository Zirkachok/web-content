+++
title      = "LFCS : Manipuler et chercher des fichiers"
date       = "2017-04-03T08:37:02+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++

<!-- ê î ô -->

Avant de s'atteler à des sujets précis pour notre préparation à la LFCS, je vais vous présenter en vrac quelques commandes essentielles qui nous seront utiles par la suite pour bricoler sur notre système. Pas d'exemples concrets ici, à vous de vous essayer à ces nouveautés pour mieux les découvrir.

<div class="warning">Il s'agit ici d'une introduction destinée aux débutants. Pour ceux d'entre vous déjà familiers avec la ligne de commande, vous n'apprendrez probablement rien de nouveau dans cet article.</div>

# Manipuler les fichiers

## Visualiser du contenu

Je vous ai présenté dans mon précédent article comment créer, lister et détruire des fichiers, l'étape suivante est donc d'apprendre à visualiser leur contenu. Pour cela, il existe de nombreuses commandes, chacune ayant ses spécificités. En voici les principales :

 - **file** : retourne le type d'un "fichier" (répertoire, exécutable, données, etc.)
 - **cat** (pour "*ConcATenate*") : concatène et affiche le contenu du/des fichier(s) texte donné(s) en argument(s). Idéal pour lire les petits fichiers
 - **less** : affiche le contenu d'un fichier texte de manière paginée, en entrant dans une interface dédiée. Cette interface réagit à des commandes clavier spécifiques :
  - **q** : permet de quitter l'interface de *less*
  - **g** ou **<** : se déplace à la première ligne du fichier
  - **G** ou **>** : se déplace à la dernière ligne du fichier
  - **/XYZ** : cherche la chaîne de caractères *XYZ* dans le fichier. **n** permet d'afficher la prochaine occurrence
  - **h** : affiche l'aide de *less* et résume les commandes les plus importantes

Pour éditer ces fichiers, il nous faudra utiliser une application spécifique, ma préférence allant pour **vim**, mais cela fera l'objet d'un autre article. Nous verrons aussi plus tard quelques méthodes de manipulation avancée sur ces fichiers (trier et filtrer les données, etc.).

# Chercher dans l'arborescence

À ce stade là, nous savons déjà faire beaucoup de choses pour manipuler l'arborescence. Reste que lorsqu'on cherche un fichier précis sans savoir où il se trouve exactement, les choses se compliquent. C'est là que la commande **locate** entre en jeu. Avec en argument un ou plusieurs motif(s), elle permet de lister tous les chemins contenant un des motifs en question. Quelques options permettent d'affiner ces recherches :

 - **-i** ou **--ignore-case** : De base, locate est sensible à la casse. L'option *-i* enlève cette restriction.
 - **-c** ou **--count** : Plutôt que de lister les fichiers trouver, *locate* en renverra le nombre.
 - **-b** ou **--basename** : Retourne uniquement les fichiers dont le *nom* contient le motif (et non plus le chemin)
 - **-A** ou **--all**  : Liste les chemins contenant *tous* les motifs donnés (et non ceux en contenant au moins un)

Et par l'exemple, cela donne :

	$ locate zirka
	/home/jul/Work/zirka.jpg
	/home/jul/Desktop/zirka-listing.doc
	/home/jul/Work/zirka/test1.data
	/home/jul/Work/zirka/test2.data
	[...]

	$ locate -b zirka
	/home/jul/Work/zirka.jpg
	/home/jul/Desktop/zirka-listing.doc
	[...]

	$ locate zirka test
	/home/jul/Work/zirka.jpg
	/home/jul/Desktop/zirka-listing.doc
	/home/jul/Work/zirka/test1.data
	/home/jul/Work/zirka/test2.data
	/home/jul/Work/tmp/test.txt
	[...]

	$ locate -A zirka test
	/home/jul/Work/zirka/test1.data
	/home/jul/Work/zirka/test2.data

Par contre, pour des recherches plus précises et dans un dossier en particulier, on se tournera vers la commande **find**. Elle prends en argument le chemin du dossier à partir duquel la recherche sera effectuée, et listera les fichiers présents dans le dossier et ses sous-dossiers. Il est aussi possible de lui spécifier des recherches particulières avec les options, comme les quelques exemples suivants (plus de détails dans le manuel) :

 - **-name** : fichiers dont le 
 - **-type** : 
 - **-ctime**/**-mtime**/**-atime** : 
 - **-size** : 
 - **-perm** : [^1]

[^1]: La gestion des droits et permissions sous Linux fera bientôt l'objet d'un article séparé.

Il en existe beaucoup, je vous laisserai donc les découvrir en lisant le manuel (voir plus bas) et en testant : **-name**, **-type**, **-perm**, **-user**, **-ctime**, **-mtime**, **-atime**, **-size**, etc.[^]

C'est là que la commande **find** intervient. Elle prends en argument le chemin du dossier à partir duquel la recherche sera effectuée, et listera les fichiers présents dans le dossier et ses sous-dossiers. Il est aussi possible de spécifier des recherches particulières avec les options. Il en existe beaucoup, je vous laisserai donc les découvrir en lisant le manuel (voir plus bas) et en testant : **-name**, **-type**, **-perm**, **-user**, **-ctime**, **-mtime**, **-atime**, **-size**, etc.

Au passage, il est possible avec find et bien d'autres commandes (ls, file, etc.) de donner des " *motifs* " plutôt qu'un nom précis, grâce à ce que l'on appelle des " expressions régulières " (plus à ce sujet dans un prochain article). Sans entrer dans les détails, il est possible d'utiliser le caractère __*__ pour désigner "tout". Par exemple, la commande " _ls *.bin_ " listera tous les fichiers dont le nom finit par _.bin_ .

<!-- # Maîtriser la ligne de commande

## Afficher l'aide

Il est juste impossible de mémoriser toutes les commandes, et il n'est pas rare que l'on ait besoin d'une aide pour en connaitre le fonctionnement et les options. Pour cela, il existe le manuel, accessible par la commande **man** suivie de la commande cherchée. On entre alors dans une interface identique à celle de *less*, avec une explication complète de la commande en question. N'hésitez pas à chercher la page de manuel des commandes que nous avons vu jusque là, pour vous familiariser avec cet outil. 

De manière générale, ayez le réflèxe de lire le manuel pour toute nouvelle commande. Vous pourrez constater certaines similitudes (fonctionnement, arguments, etc.), et elles deviendront vite plus facile à retenir.

## Historique et auto-complétion

Sous Linux, les dernières commandes entrées sont mémorisées, et peuvent être parcourues à l'aide des flèches *haut* et *bas*. Il est aussi possible de parcourir l'historique, via la commande **history**. Encore plus pratique, appuyer sur **Ctrl-r** nous fait entrer dans un mode spécifique, qui lorsque l'on tape une commande parcours cet historique et propose la dernière commande entrée similaire. Ce mode est particulièrement pratique lorsque l'on répète régulièrement les mêmes commandes.

Une autre fonctionnalité intéressante est l'auto-complétion. Lorsque l'on entre une commande, il est possible d'appuyer sur la touche **Tab** pour que celle-ci soit automatiquement complétée. En appuyant deux fois sur *Tab*, la liste des complétions possibles est retournée.

Pour finir, on pourrait citer la commande **clear**, qui nettoie le terminal en mettant la ligne courante tout en haut. Il est aussi possible d'utiliser le raccourci **ctrl-L** pour cette action. Bien pratique pour y voir plus clair. -->