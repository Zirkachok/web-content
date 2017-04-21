+++
title      = "LFCS : Utilisateurs et droits dans un système Linux"
date       = "2017-04-13T08:46:32+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô û -->

Je vous ai déjà parlé en détails des fichiers et leur organisation sous Linux ("Tout est fichier" après tout), et aujourd'hui, je vais en ajouter une couche en abordant le thème des droits et permissions. Comme tout système d'exploitation (sauf rares exceptions, comme certains mainframe), Linux est capable de gérer plusieurs utilisateurs, et chacun d'entre eux n'a le droit d'accéder qu'à certains fichiers. Vous ne voudriez pas que votre famille et vos collègues puissent consulter et modifier _vos_ fichiers, n'est-ce pas?

# Utilisateurs et groupes

Plusieurs utilisateurs[^1] cohabitent donc dans le système, chacun ayant un identifiant unique (**UID** pour User ID) utilisé par le système, un nom (**username**) qui est une représentation plus lisible associée à l'UID, un **mot de passe**, et un répertoire personnel dans l'arborescence, qui contient les données propres à l'utilisateur (et dont le chemin par défaut est le plus souvent "*/home/username*").

[^1]: À noter que sous Linux, il existe des utilisateurs autres que les personnes physiques utilisant le système. Il peut s'agit de [démons](https://fr.wikipedia.org/wiki/Daemon_(informatique)), de processus fonctionnant en arrière-plan, etc.

Cependant, gérer les droits de chaque fichier indépendemment pour chaque utilisateur serait lourd et complexe. Sous Linux, une alternative a donc été trouvée via les groupes. Un groupe liste l'ensemble des utilisateurs qui lui appartiennent. Comme pour les utilisateurs, chaque groupe a un identifiant unique (le **GID** ou Group ID), un nom, et des permissions propres. Chaque utilisateur faisant partie d'un groupe en partagera aussi les droits, en plus des siens. À noter qu'un utilisateur peut appartenir à plusieurs groupes, multipliant d'autant plus les possibilités de gestion.


## Le cas particulier de l'administrateur

L'un des utilisateurs les plus importants est l'**administrateur** (aussi appelé _superutilisateur_ ou **root**, dont l'UID est 0). Grossièrement, l'administrateur a tous les droits, alors que ceux des utilisateurs lambda sont limités : il a plein accès a n'importe quel fichier, peut créer et détruire des utilisateurs, modifier leurs droits, etc.

Vous l'aurez compris, effectuer des actions en tant qu'administrateur peut s'avérer dangereux (détruire des fichiers système par exemple), mais est aussi bien souvent nécessaire, car les actions possibles pour un utilisateur lambda sont assez limitées. Deux approches sont possibles pour "devenir" administrateur :  

 - Via la commande **sudo** (pour SuperUser DO), qui permet d'exécuter une autre commande donnée après elle comme administrateur.
 - Via la commande **su** (pour SuperUser), qui permet de changer l'utilisateur courant pour l'administrateur[^2].

À noter que la commande **id** permet d'obtenir les informations (UID, GID, groupes auquel il appartient) de l'utilsateur courant (ou un autre donné en argument).

[^2]: _sudo_ et _su_ sont en fait plus générales, et permettent de réaliser des actions pour n'importe quel utilisateur, l'administrateur étant celui choisi par défaut.


## Gérer les utilisateurs et groupes

En pratique, la création d'un nouvel utilisateur passe par la commande **adduser** suivie du nom de l'utilisateur. Cela effectuera les actions configurées dans " _/etc/adduser.conf_ "[^5], notamment (en général):

 - générer un mot de passe (demandé par la commande lors de son exécution)
 - créer un répertoire personnel associé à l'utilisateur (/home/_user_ par défaut)
 - créer un groupe portant le même nom que cet utilisateur

De manière similaire, la commande **deluser** permettra de supprimer un utilisateur, et fera les actions listées dans " _/etc/deluser.conf_ ". La commande **passwd** peut aussi s'avérer utile: elle permet de changer le mot de passe de l'utilisateur courant (ou un autre donné en argument).

Les groupes fonctionnent de manière identique, du moins dans leur gestion. Les commandes **groupadd**, **groupdel** permettent ainsi respectivement de créer et supprimer un groupe. La commande **groups** permet elle de lister les groupes existants.

	$ ls /home
	zirka

	$ id testusr
	id: «testusr» : utilisateur inexistant

	$ sudo adduser testusr
	$ ls /home
	zirka  testusr

	$ id testusr
	uid=1001(testusr) gid=1001(testusr) groupes=1001(testusr)

	$ sudo deluser testusr

	$ id testusr
	id: «testusr» : utilisateur inexistant

[^5]: Il est aussi possible d'utiliser la commande **useradd**/**userdel**/**usermod**, qui créera/détruira/modifiera (on dossier personnel par exemple) elle aussi un utilisateur, mais sans effectuer cette liste d'actions.


## Fichiers systèmes

La gestion des utilisateurs et groupes passe (encore) par des fichiers, qui sont automatiquement modifiés par les commandes que nous venont de voir. Il n'est pas recommandé de les modifier soi-même (comme pour tous les fichiers système), mais on peut en lire le contenu pour obtenir des informations :

 - **/etc/passwd** contient la liste des utilisateurs et leurs informations. Chaque ligne contient :
  - le nom d'utilisateur, suivi d'un _x_ indiquant si le compte est protégé par un mot de passe
  - l'UID et GID associés à l'utilisateur
  - des commentaires sur l'utilisateur (nom et coordonnées)
  - le répertoire personnel et l'interpréteur par défaut (voir [mon article sur le shell]({{< ref "lfcs-commandline.md" >}}))
 - **/etc/shadow** contient les mots de passe des utilisateurs, et ne sont pas disponibles en clair (le fichier n'est d'ailleurs lisible que par l'administrateur)[^4]
 - **/etc/group** contient toutes les informations sur les groupes. Chaque ligne contient :
  - le nom du groupe, suivi d'un _x_ indiquant si le groupe est protégé par un mot de passe
  - le GID et les membres du groupe



[^4]: Il s'agit en fait d'un [hash du mot de passe](https://www.aychedee.com/2012/03/14/etc_shadow-password-hash-formats/). Cette opération est irréversible (il n'est pas possible d'obtenir le mot de passe à partir du hash).


# Les permissions

Nous l'avons jusque-là sous-entendu, les fichiers sous Linux sont dotés de permissions (que nous appellerons aussi "_droits_"). Entrons à présent dans les détails pour découvrir ce que cela implique.

3 opérations seulement sont permises sur les fichiers, **la lecture, l'écriture, et l'exécution**, chacune de ces opérations disposant de droits indépendants. La commande **ls** avec l'option **-l** nous listera les fichiers avec certaines de leurs principales propriétés.

	$ ls -l
	drwxr-xr-x   2   zirka   admin    4096   mars   1 11:45   annexes
	-r--r--r--   1   mary    mary     5769   mars  24 14:26   lfce-lfcs.md
	-rwxrw-r--   1   zirka   admin    7201   avril 10 15:51   lfcs-commandline.md
	-rw-rw-r--   1   jul     author   8710   avril 10 15:47   lfcs-navigate.md
	-rwxrw-r--   1   zirka   admin    1236   avril 10 15:49   lfcs-rights.md
	-rw-rw-r--   1   jul     author   3647   avril 10 14:45   lfcs-search.md
	-rwxrw-r--   1   zirka   admin   10384   mars  24 09:49   pourquoi-golang.md

On y retrouve dans la dernière colonne le nom de nos fichiers, précédé de sa date de dernière modification et de sa taille. En seconde position, un chiffre indique son nombre de liens, et est suivi de son **groupe propriétaire** et de son **utilisateur propriétaire**.

En première position, on trouve une chaine de caractères particulière. Le premier caractère désigne le **type de fichier** (_-_ pour les fichiers réguliers, _d_ pour les répertoires, _l_ pour les liens symboliques). Les 9 autres caractères représentent les droit d'accès, avec **r** signifiant "accesible en lecture" (readable), **w** pour "accessible en écriture" (writable) et **x** pour "accessible en exécution" (executable). Chaque groupe de 3 caractères représente les droits pour certains utilisateurs :

 - Les **trois premiers** indiquent les droits pour **l'utilisateur propriétaire** (user, colonne 3 avec "_ls -l_").
 - Les **trois suivants** indiquent les droits pour les membres du **groupe propriétaire** (group, colonne 4 avec "_ls -l_").
 - Les **trois derniers** indiquent les droits pour les **autres utilisateurs** (others).

Ainsi, dans notre exemple précédent, le fichier "lfcs-navigate.md" sera totalement accessible pour l'utilisateur "zirka", mais seulement en lercture/écriture pour les membres du groupe "admin" et seulement en lecture pour les autres. N'oublions pas que le superutilisateur a les droits d'accès complets (rwx) sur tous les fichiers de son système.

## Gérer les droits

Bien sûr, les permissions et propriétaires d'un fichier peuvent être changées. La commande **chmod _p_ _f_** permet de modifier les droits au fichier _f_. _p_ prends alors la forme d'
une chaine de cactère avec un premier caractère **u**, **g** ou **o** pour désigner la cible de ces droits (user, group ou others), un second caractère **+** ou **-** signifiant "ajouter" ou "retirer" les droits à la cible, et une suite de caractères **r** et/ou **w** et/ou **x** pour désigner les droits concernés.

	$ ls -l
	-rwxrwxr--   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ sudo chmod o+w test.txt
	$ ls -l
	-rwxrwxrw-   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ sudo chmod g-wx test.txt
	$ ls -l
	-rwxr--rw-   1   moi   mongroupe   921   avril 10 15:47   test.txt

Une autre méthode[^6], dite numérique, consiste à fournir la liste complète des droits à affecter par une suite de trois chiffres les représentants. Chaque chiffre est alors la somme des droits voulus, avec :

 - 4 : accès en lecture
 - 2 : accès en écriture
 - 1 : accès en exécution


Par l'exemple cela donne :

	$ ls -l
	-rwxr--rw-   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ sudo chmod 754 test.txt
	$ ls -l
	-rwxr-xr--   1   moi   mongroupe   921   avril 10 15:47   test.txt



[^6]: Une troisième méthode consiste à remplacer la liste complète des droits à affecter par [une suite de nombres les représentants](https://linuxjourney.com/lesson/modifying-permissions).


Nous avons vu aujourd'hui les aspects fondamentaux de la gestion des droits dans un système Linux. Cependant, il reste bien des aspects et des commandes importantes que nous avons passé sous silence. Notre prochain article reviendra donc sur la question, avec de nouvelles commandes et quelques conseils sur la question.


<!--
Ref : https://linuxjourney.com/lesson/file-permissions
-->



<!-- sudo -->

<!-- uname -a -->
<!-- Linux Epinet 3.13.0-93-generic #140-Ubuntu SMP Mon Jul 18 21:21:05 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux -->
<!--   A      B         C                                D                          E      F      G      H      -->
<!-- A = kernel name ; B = network node hostname ; C = kernel release ; D = kernel version ; E = machine hardware name -->
<!-- F = processor type (non-portable) ; G = hardware platform (non-portable) ; H = operating system -->
<!-- shutdown -->
<!-- logout -->
