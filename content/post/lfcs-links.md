+++
title      = "LFCS : Système de fichiers et liens"
date       = "2017-04-11T15:59:20+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô û -->

On dit souvent que sous Unix et Linux[^1], "tout est fichier", car les répertoires, fichiers réguliers, etc. y sont représentés par des fichiers dont seul le type et le contenu changent. Dans cet article, nous allons vois plus en détail ce qu'il en est vraiment, et découvrir un autre type de fichier, les liens.

Pour illustrer ces concepts pas toujours évidents à appréhender, je vous ai aussi préparé une analogie simple et facile à retenir : la bibliothèque.

[^1]: Dans cet article, je généraliserai Unix et Linux en ne désignant que Linux, mais tous deux suivent la même philosophie.

<!-- En fait, tout est représenté pour l'utilisateur par des fichiers. Nous verrons plus en détail ce que cela implique dans un prochain article.  -->

# Tout est fichier

## Représentation des fichiers

Sous Linux, un fichier est représenté par son nom et ce qu'on appelle un **Inode**. Un Inode est un espace mémoire contenant les informations essentielles du fichier (ses attributs), à savoir :

 - son **type** : _-_ pour un fichier régulier, _d_ pour un dossier, _l_ pour un lien, etc.
 - le **nombre de ses liens physiques** : il s'agit du nombre de noms désignant cet Inode.
 - sa **taille** : l'espace mémoire que le contenu du fichier lui-même occupe.
 - l'**addresse mémoire** se son contenu.

et d'autres informations comme :

 - l'identifiant du propriétaire du fichier (UID) et du du groupe auquel appartient le fichier (GID).
 - ses droits d'accès : plus à ce sujet dans mon articles sur les _utilisateurs et droits_.
 - son horodatage (dernier accès _atime_, modification _mtime_, et modification de l'inode _ctime_).

Ainsi, un répertoire est considéré comme tout autre fichier régulier, à ceci près que son _type_ est différent (_d_ dans ce cas). Aussi, le "contenu" d'un répertoire est une table d'association entre des noms (les fichiers contenus dans le répertoire) et des inodes. De la même manière, un dispositif de communication (un périphérique ou du matériel, par exemple) sera lui aussi représenté par un fichier (de type _p_), etc. Il est possible d'utiliser la commande **ls** avec l'option **-i** pour obtenir le numéro d'inode d'un fichier.

Pour l'utilisateur, tout est donc représenté par des fichiers[^2], qui suivent une certaine organisation : le système de fichier.

[^2]: Le noyau Linux quant à lui gère les choses différemment, mais c'est une toute autre question.

## Le système de fichiers

Le système de fichier est une représentation de l'arborescence, stockée en mémoire selon une certaine norme. Sous Linux, le système de fichiers contient un index de l'ensemble des inodes, suivi du contenu de contenu de chaque inode. L'inode 2 est la racine de l'arborescence[^3], et il est facile à partir de là d'en déduire récursivement l'ensemble de l'arborescence. C'est le principe défini par la norme **Ext** (pour _Extended File System_), dont la version la plus récente est l'**Ext4**.

Il existe bien d'autres normes de systèmes de fichiers, chacune ayant ses spécificités. Les plus courantes sont **FAT** (pour _File Allocation Table_), **NTFS** (pour _New Technology File System_, utilisé par les systèmes Windows) et bien sûr **Ext2**, **Ext3** et **Ext4**.

Plusieurs systèmes de fichiers peuvent être "_montés_" sur dans l'arborescence, et peuvent être listés à l'aide de la commande **df -T**. Nous verrons plus en détail dans un autre article comment monter, démonter et gérer ces différents systèmes de fichiers.

	$ df -T
	Sys. de fichiers             Type       blocs de 1K    Utilisé Disponible Uti% Monté sur
	udev                         devtmpfs       1942644          0    1942644   0% /dev
	tmpfs                        tmpfs           391780       6224     385556   2% /run
	/dev/sda7                    ext4          57542652   47442952    7153652  87% /
	[...]

<!--
Ref : http://www.linux-france.org/article/dalox/unix02.htm
      http://zero202.free.fr/cs3-svsf/html/ar01s02.html
      https://doc.ubuntu-fr.org/systeme_de_fichiers
      http://www.linux-france.org/article/kafkafr/node19.html
      http://formation-debian.via.ecp.fr/filesystem.html
      https://fr.wikipedia.org/wiki/N%C5%93ud_d%27index
      https://forum.ubuntu-fr.org/viewtopic.php?id=282989
      http://www.funix.org/fr/unix/fichiers.htm
-->



# Les liens

Maintenant qua nous avons vu ce que sont les inodes et comment ils sont gérés, nous pouvons aborder le concept de **liens**. Un lien est un type de fichier spécifique, qui sert d'alias pour un autre. Il fait donc référence à ce dernier, situé ailleurs dans le système de fichiers. En pratique, il existe deux types de liens : les **liens physiques** (_hard links_ en Anglais), et les **liens symboliques** (_soft links_, _symbolic links_ ou _symlinks_ en Anglais). Voyons-en les particularités.

## Liens physiques

Un fichier de lien physique et le fichier régulier auquel il est lié sont tous deux associés au **même inode**. Ainsi, la suppression d'un de ces deux fichiers ne détruira pas l'autre (l'inode sera toujours accessible tant qu'un nom de fichier y est associé), et la modification de l'un entrainera la modification de l'autre (car ce sont les données dont l'addresse est contenue dans l'inode qui sont alors modifiées).

Dans la pratique, c'est la commande **ln** qui permet de créer des liens (les lignes commencant par **#** sont des commentaires) :

	$ cat hell.txt
	Living easy, living free
	Season ticket on a one-way ride
	[...]

	$ ln hell.txt hell_link.txt

	$ ls -i
	666 hell.txt  666 hell_link.txt

	cat hell_link.txt
	Living easy, living free
	Season ticket on a one-way ride
	[...]

	# Imagineons que l'on change maintenant le contenu de hell.txt
	$ cat hell.txt
	No stop signs, speed limit
	Nobody is gonna slow me down
	[...]

	$ cat hell_link.txt
	No stop signs, speed limit
	Nobody is gonna slow me down
	[...]

	$ rm hell.txt

	$cat hell_link.txt
	No stop signs, speed limit
	Nobody gonna slow me down
	[...]

## Liens symboliques

Au contraire, un lien symbolique ressemble plutôt à un raccourci. Un fichier de lien sybolique symbolique aura son inode distinct, dont l'addresse vers le contenu désignera l'inode du fichier auquel il est lié, et non plus son contenu. Ainsi, détruire le fichier original rendra la lecture ou la modification du lien symbolique impossible.

Là encore, c'est la commande **ln** qui crée des liens, qui seront des liens symboliques avec l'option **-s** :

	$ cat hell.txt
	Living easy, living free
	Season ticket on a one-way ride
	[...]

	$ ln -s hell.txt hell_link.txt

	$ ls -i
	666 hell.txt  426 hell_link.txt

	cat hell_link.txt
	Living easy, living free
	Season ticket on a one-way ride
	[...]

	# Imagineons que l'on change maintenant le contenu de hell.txt
	$ cat hell.txt
	No stop signs, speed limit
	Nobody is gonna slow me down
	[...]

	$ cat hell_link.txt
	No stop signs, speed limit
	Nobody is gonna slow me down
	[...]

	$ rm hell.txt

	$cat hell_link.txt
	cat: hell_link.txt: Aucun fichier ou dossier de ce type


# L'analogie de la bibliothèque

Un moyen simple de comprendre le fonctionnement d'un système de fichiers est d'en faire l'analogie avec un bibliothèque. La bibliothèque dispose d'un index classant les fiches de chaque livre disponible. Une fiche décrit sommairement le livre (le genre littéraire, le nombre de pages) et en contient l'emplacement dans la bibliothèque. Vous l'aurez compris, notre bibliothèque représente le système de fichiers, et les fiches sont nos inodes (dont le contenu est d'ailleurs similaire).

Un même livre peut être désigné par deux fiches distinctes. Dans ce cas, la destruction d'une des fiche n'impacte pas l'autre (le livre n'a pas changé de place ou de contenu, l'autre fiche est donc toujours valide). C'est sur ce modèle que fonctionnent les liens physiques.
Une fiche peut aussi renvoyer à une autre fiche existante. Dans ce cas, la destruction de cette dernière rendra la première invalide (car à partir de la référence, on ne pourra plus trouver le livre). C'est ainsi que fonctionnent les liens symboliques.



<!-- 
un symlink c'est different dans le sens ou, le fichier original va pointer sur l'inode contenant la data,
et le symlink va pointer lui sur un inode dont la data correspond au chemin du fichier original !
ce qui fais que lorsque tu détruit le fichier original, le symlink est cassé !  -->

<!-- uname -a -->
<!-- Linux Epinet 3.13.0-93-generic #140-Ubuntu SMP Mon Jul 18 21:21:05 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux -->
<!--   A      B         C                                D                          E      F      G      H      -->
<!-- A = kernel name ; B = network node hostname ; C = kernel release ; D = kernel version ; E = machine hardware name -->
<!-- F = processor type (non-portable) ; G = hardware platform (non-portable) ; H = operating system -->
<!-- shutdown -->
<!-- logout -->
