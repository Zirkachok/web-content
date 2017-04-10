+++
title      = "LFCS : Effectuer des recherches dans l'arborescence"
date       = "2017-04-10T13:48:16+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô -->

À ce stade là, nous savons déjà faire beaucoup de choses pour manipuler l'arborescence. Reste que lorsqu'on cherche un fichier précis sans savoir où il se trouve exactement, les choses se compliquent. C'est là que la commande **locate** entre en jeu. Avec en argument un ou plusieurs motif(s), elle permet de lister tous les chemins contenant un des motifs en question. Quelques options permettent d'affiner ces recherches :

 - **-i** ou **\-\-ignore-case** : De base, locate est sensible à la casse. L'option *-i* enlève cette restriction.
 - **-c** ou **\-\-count** : Plutôt que de lister les fichiers trouver, *locate* en renverra le nombre.
 - **-b** ou **\-\-basename** : Liste uniquement les fichiers dont le *nom* contient le motif (et non plus le chemin).
 - **-A** ou **\-\-all**  : Liste les chemins contenant *tous* les motifs donnés (et non ceux en contenant au moins un).

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

Par contre, pour des recherches plus précises et dans un dossier en particulier, on se tournera vers la commande **find**. Elle prends en argument le chemin du dossier à partir duquel la recherche sera effectuée, et listera les fichiers présents dans le dossier et ses sous-dossiers. Il est aussi possible de lui spécifier des filtres avec les options, comme les quelques exemples suivants (plus de détails dans le manuel) :

 - **-maxdepth _x_** : Ne garde que les fichiers contenus dans une suite d'au plus _x_ dossiers (i.e. ayant une profondeur de _x_). 
 - **-name** : Fichiers dont le 
 - **-type** : Filtre les fichiers par type
  - **-type d** pour les dossiers
  - **-type f** pour les fichiers
  - **-type l** pour les liens
  - etc.
 - **-ctime** : Filtre les fichiers par date de création. Il existe aussi les commandes similaires **-mtime** et **-atime**, qui filtrent respectivement par date de dernière modification ou de dernier accès.
  - **-ctime _n_** pour les fichiers créés il y a exactement _n_ jours.
  - **-ctime _+n_** pour les fichiers créés il y a plus de _n_ jours.
  - **-ctime _-n_** pour les fichiers créés il y a moins de _n_ jours.
 - **-size** : Comme pour _-ctime_, mais filtre par taille de fichiers.

Et dans la pratique, cela donne:
	
	$ pwd
	/home/zirka/web-content

	$ find .
	.
	./content
	./content/test.txt
	./content/post
	./content/post/lfcs-navigate.md
	./content/post/lfcs-commandline.md
	./content/post/hello-world.md
	./content/post/lfcs-search.md
	./content/post/lfce-lfcs.md
	./content/post/pourquoi-golang.md

	$ find . -maxdepth 2
	./content/test.txt

	find . -type d
	.
	./content
	./content/post

	$ find . -mtime -1
	./content
	./content/post
	./content/post/lfcs-commandline.md
	./content/post/lfcs-search.md


Pour rappel, il est possible avec find et bien d'autres commandes (ls, file, etc.) de donner des **motifs "abstraits"**  plutôt qu'un nom précis [^1]. Sans entrer dans les détails, il est possible d'utiliser le caractère __*__ pour désigner "tout". Par exemple, la commande " _ls *.bin_ " listera tous les fichiers dont le nom finit par _.bin_ .

[^1]: C'est ce que l'on appelle des " expressions régulières ". Plus à ce sujet dans un prochain article.
