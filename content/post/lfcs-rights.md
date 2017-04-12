+++
title      = "LFCS : Utilisateurs et droits dans un système Linux"
date       = "2017-04-11T15:59:20+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++




# Utilisateurs et superutilisateur



# Gérer les utilisateurs et groupes

/etc/passwd : stores usernames, home directories, etc.

 - sudo **adduser _x_** : 
 - sudo **user del _x_** :
 - sudo **passwd _x_** :

Dans les articles précédents, nous avons vu certaines . 

 - **ls -l**
 - **find -user _x_** : Liste les fichiers appartenant à l'utilisateur _x_.
 - **find -perm** : Liste les fichiers selon leurs permissions:
  - **-perm _-n_** : Ne liste que les fichiers dont les permissions sont exactement _n_.
  - **-perm _/n_** : 

Et en pratique, cela nous donne :

	$ ls -l
	-rw-rw-r-- 1 zirka zirka  7771 févr. 17 15:59 hello-world.md
	-rw-rw-r-- 1 mary  mary   5769 mars  24 14:26 lfce-lfcs.md
	-rw-rw-r-- 1 zirka zirka  7201 avril 10 15:51 lfcs-commandline.md
	-rw-rw-r-- 1 jul   jul    8710 avril 10 15:47 lfcs-navigate.md
	-rw-rw-r-- 1 zirka zirka  1236 avril 10 15:49 lfcs-rights.md
	-rw-rw-r-- 1 jul   jul    3647 avril 10 14:45 lfcs-search.md
	-rw-rw-r-- 1 zirka zirka 10384 mars  24 09:49 pourquoi-golang.md

	$ find . -user jul
	./lfcs-navigate.md
	./lfcs-search.md

	$ find . -perm XXXXXXXXXXXX



<!-- sudo -->

<!-- uname -a -->
<!-- Linux Epinet 3.13.0-93-generic #140-Ubuntu SMP Mon Jul 18 21:21:05 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux -->
<!--   A      B         C                                D                          E      F      G      H      -->
<!-- A = kernel name ; B = network node hostname ; C = kernel release ; D = kernel version ; E = machine hardware name -->
<!-- F = processor type (non-portable) ; G = hardware platform (non-portable) ; H = operating system -->
<!-- shutdown -->
<!-- logout -->
