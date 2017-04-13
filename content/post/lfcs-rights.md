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

Je vous ai déjà parlé en détails des fichiers et leur organisation sous Linux ("Tout est fichier" après tout), et aujourd'hui, je vais en ajouter une couche en abordant le thème des droits et permissions. Comme tout système d'exploitation (sauf rares exceptions comme certains mainframe), Linux est capable de gérer plusieurs utilisateurs, et chacun d'entre eux n'a le droit d'accéder qu'à certains fichiers. Vous ne voudriez pas que votre famille et vos collègues puissent consulter et modifier _vos_ fichiers, n'est-ce pas?

# Utilisateurs et groupes

Plusieurs utilisateurs[^1] cohabitent donc dans le système, chacun ayant un identifiant unique (**UID** pour User ID) utilisé par le système, un nom (**username**) qui est une représentation plus lisible associée à l'UID, un **mot de passe**, et un répertoire personnel dans l'arborescence, qui contient les données propres à l'utilisateur (et dont le chemin par défaut est le plus souvent "*/home/username*"). L'un des utilisateurs les plus importants est le **superutilisateur** (aussi appelé _root_ ou _superuser_, dont l'UID est 0), qui a accès à n'importe quel fichier[^2]. La commande **sudo** (pour SuperUser DO) permet d'exécuter une commande donnée après elle comme superutilisateur[^3], et **su** (pour SuperUser) permet de change l'utilisateur courant pour le superutilisateur (ou tout autre utilisateur donné en argument). Vous l'aurez compris, effectuer des actions via le superutilisateur peut s'avérer dangereux (détruire des fichiers système par exemple), donc soyez vigilents.

[^1]: À noter que sous Linux, il existe des utilisateurs autres que les personnes physiques utilisant le système. Il peut s'agit de [démons](https://fr.wikipedia.org/wiki/Daemon_(informatique)), de processus fonctionnant en arrière-plan, etc.

[^2]: Il a aussi le droit de démarrer ou terminer n'importe quel processus, mais nous aborderons ce sujet dans un autre article.

[^3]: Ou plutôt, permet d'exécuter une commande sous n'importe quel utilisateur, le superutilisateur étant choisi par défaut.


Cependant, gérer les droits de chaque fichier pour chaque utilisateur serait lourd et complexe. Sous Linux, une alternative a donc été trouvée via les groupes. Un groupe liste l'ensemble des utilisateurs qui lui appartiennent. Comme pour les utilisateurs, chaque groupe a un identifiant unique (le **GID** ou Group ID), un nom, et des permissions propres. Chaque utilisateur faisant partie d'un groupe en partagera aussi les droits, en plus des siens. À noter qu'un utilisateur peut appartenir à plusieurs groupes, multipliant les possibilités de gestion. <!-- Ainsi, si un utilisateur _lambda_ fait partie du groupe _g_, il aura accès aux fichiers autorisés pour _u_ et pour _g_. -->

En pratique, la gestion des utilisateurs passe par quelques commandes :

 - **id** : Retourne les informations de l'utilsateur courant (son UID, GID, et les groupes auquel il appartient). Il est aussi possible de donner le nom d'un autre utilisateur pour en obtenir les informations.
 - sudo **adduser _u_** : Crée un compte utilisateur _u_ dans le système. Cela vous demandera des informations complémentaires, comme le mot de passe.
 - sudo **userdel _u_** : Supprime un compte utilisateur _u_ et les fichiers associés du système.
 - sudo **usermod _u_** : Permet de modifier un compte utilisateur _u_ (son dossier personnel par exemple).
 - sudo **passwd _u_** : Modifie le mot de passe de l'utilisaeur _u_

Et pour les groupes :

 - sudo **groupadd _g_** : Crée un groupe _g_ dans le système.
 - sudo **groupdel _g_** : Supprime un groupe _g_ du système.
 - sudo **adduser _u_ _g_** : Ajoute l'utilisateur _u_ au groupe _g_.
 - sudo **deluser _u_ _g_** : Supprime l'utilisateur _u_ du groupe _g_.


Enfin, vous pouvez consulter la liste des utilisateurs et avoir leurs informations dans le fichier **/etc/passwd** (avec leur UID, répertoire courant, etc.). Bien entendu, les mots de passe des utilisateurs sont regroupés dans un autre fichier, **/etc/shadow** (qui ne peut être lu que par le superutilisateur), et ne sont pas disponibles en clair[^4]. De la même manière, le fichier **/etc/group** contient toutes les informations sur les groupes (dont leur GID). Les mots de passe

[^4]: Il s'agit en fait d'un [hash du mot de passe](https://www.aychedee.com/2012/03/14/etc_shadow-password-hash-formats/). Cette opération est irréversible (il n'est pas possible d'obtenir le mot de passe à partir du hash).

# Les permissions

Nous l'avons jusque-là sous-entendu, les fichiers sous Linux sont dotés de permissions (que nous appellerons aussi "_droits_"). Entrons à présent dans les détails pour découvrir ce que cela implique. 3 opérations seulement sont permises sur les fichiers, la lecture, l'écriture, et l'exécution, chacune de ces opérations disposant de droits indépendants. La commande **ls** avec l'option **-l** nous listera les fichiers avec certaines de leurs principales propriétés.

	$ ls -l
	drwxr-xr-x   2   zirka   admin    4096   mars   1 11:45   annexes
	-r--r--r--   1   mary    mary     5769   mars  24 14:26   lfce-lfcs.md
	-rwxrw-r--   1   zirka   admin    7201   avril 10 15:51   lfcs-commandline.md
	-rw-rw-r--   1   jul     author   8710   avril 10 15:47   lfcs-navigate.md
	-rwxrw-r--   1   zirka   admin    1236   avril 10 15:49   lfcs-rights.md
	-rw-rw-r--   1   jul     author   3647   avril 10 14:45   lfcs-search.md
	-rwxrw-r--   1   zirka   admin   10384   mars  24 09:49   pourquoi-golang.md

On y retrouve dans la dernière colonne le nom de nos fichiers, précédé de sa date de dernière modification et de sa taille. En seconde position, un chiffre indique son nombre de liens, et est suivi de son **groupe propriétaire** et de son **utilisateur propriétaire**.

En première position, on retrouve une chaine de caractères particulière. Le premier caractère désigne le **type de fichier** (_-_ pour les fichiers réguliers, _d_ pour les répertoires, _l_ pour les liens symboliques). Les 9 autres caractères représentent les droit d'accès, avec **r** signifiant "accesible en lecture" (readable), **w** pour "accessible en écriture" (writable) et **x** pour "accessible en exécution" (executable). Chaque groupe de 3 caractères représente les droits pour certains utilisateurs :

 - Les **trois premiers** indiquent les droits pour **l'utilisateur propriétaire** (colonne 3 avec "_ls -l_").
 - Les **trois suivants** indiquent les droits pour les membres du **groupe propriétaire** (colonne 4 avec "_ls -l_").
 - Les **trois derniers** indiquent les droits pour les **autres utilisateurs**.

Ainsi, dans notre exemple précédent, le fichier "lfcs-navigate.md" sera totalement accessible pour l'utilisateur "zirka", mais seulement en lercture/écriture pour les membres du groupe "admin" et seulement en lecture pour les autres. N'oublions pas que le superutilisateur a les droits d'accès complets (rwx) sur tous les fichiers de son système.

## Gérer les droits

Bien sûr, les permissions et propriétaires d'un fichier peuvent être changées. La commande **chmod _x_ _y_** permet de modifier les droits au fichier _y_. _x_ prends alors la forme d'
une chaine de cactère avec un premier caractère **u**, **g** ou **o** pour désigner la cible de ces droits, un second caractère **+** ou **-** signifiant "ajouter" ou "retirer" les droits à la cible, et une suite de caractères **r** et/ou **w** et/ou **x** pour désigner les droits concernés.

Une autre méthode consiste à fournir la liste complète des droits à affecter par une suite de trois chiffres les représentants. Chaque chiffre est alors la somme des droits voulus, avec :

 - 4 : accès en lecture
 - 2 : accès en écriture
 - 1 : accès en exécution


Par l'exemple cela donne :

	$ ls -l
	-rwxrwxr--   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ chmod o+w test.txt
	$ ls -l
	-rwxrwxrw-   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ chmod g-wx test.txt
	$ ls -l
	-rwxr--rw-   1   moi   mongroupe   921   avril 10 15:47   test.txt

	$ chmod 754 test.txt
	$ ls -l
	-rwxr-xr--   1   moi   mongroupe   921   avril 10 15:47   test.txt




[^5]: Une troisième méthode consiste à remplacer la liste complète des droits à affecter par [une suite de nombres les représentants](https://linuxjourney.com/lesson/modifying-permissions).
<!-- 
File permissions is displayed as following;

    first character is - or l or d, d indicates a directory, a line represents a file, l is a symlink (or soft link) - special type of file
    three sets of characters, three times, indicating permissions for owner, group and other:
        r = readable
        w = writable
        x = executable

In your example -rwxrw-r--, this means the line displayed is:

    a regular file (displayed as -)
    readable, writable and executable by owner (rwx)
    readable, writable, but not executable by group (rw-)
    readable but not writable or executable by other (r--)

 -->

<!-- 
As we learned previously, files have different permissions or file modes. Let's look at an example:

$ ls -l Desktop/

drwxr-xr-x 2 pete penguins 4096 Dec 1 11:45 .

There are four parts to a file's permissions. The first part is the filetype, which is denoted by the first character in the permissions, in our case since we are looking at a directory it shows d for the filetype. Most commonly you will see a - for a regular file.

The next three parts of the file mode are the actual permissions. The permissions are grouped into 3 bits each. The first 3 bits are user permissions, then group permissions and then other permissions. I've added the pipe to make it easier to differentiate.

d | rwx | r-x | r-x 

Each character represent a different permission:

    r: readable
    w: writable
    x: executable (basically an executable program)
    -: empty

So in the above example, we see that the user pete has read, write and execute permissions on the file. The group penguins has read and execute permissions. And finally, the other users (everyone else) has read and execute permissions. 
-->


# Recherches par utilisateurs et permissions

Je vous ai montré dans un article comment lister et trouver les fichiers dans le système, en proposant quelques filtres possibles. En voici donc quelques autres permettent de filtrer en fonction des utilisateurs, groupes et permissions :


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
