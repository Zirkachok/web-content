+++
title      = "LFCS : Utilisateurs et droits, le retour"
date       = "2017-04-18T10:44:28+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô û -->

Je vous ai présenté [la fois précédente]({{< ref "lfcs-rights.md" >}}) comment gérer les permissions et les utilisateurs au sein de votre système, mais le sujet est bien trop vaste pour être plié en un seul post de blog. Je vais donc revenir à la charge aujourd'hui avec de nouvelles commandes et quelques conseils appréciables pour optimiser leur gestion.

# Gestion avancée des permissions

## Changer de propriétaire

En plus de pouvoir changer les permissions d'un fichier, il est aussi possible d'en modifier l'utilisateur et le groupe propriétaire, respectivement avec les commandes **chown** et **chgrp** (qui nécessitent les droits administrateur). Ces commandes prennent en premier paramètre l'utilisateur/le groupe, suivi du fichier concerné. Il est aussi possible de changer à la fois l'utilisateur et le groupe propriétaires du fichier avec _chown_, en combinant les deux séparés par " _:_ ".

Par l'exemple, cela nous donne :

	$ ls -l
	-rwxrwxr--   1   calvin   humains   921   avril 10 15:47   test.txt

	$ sudo chown hobbes test.txt
	$ ls -l
	-rwxrwxr--   1   hobbes   humains   921   avril 10 15:47   test.txt

	$ sudo chgrp tigres test.txt
	$ ls -l
	-rwxrwxr--   1   hobbes   tigres    921   avril 10 15:47   test.txt

	$ sudo chown calvin:humains test.txt
	$ ls -l
	-rwxrwxr--   1   calvin   humains   921   avril 10 15:47   test.txt


## Modifier les droits par défaut

Chaque fichier créé se voit attribuer à sa création un ensemble de permissions par défaut, mais il est possible de modifier ces droits via la commande **umask**[^1].


<!-- Par défaut, les permissions d'un nouveau fichier sont 022 ou 002 -->

_umask_ utilise la variante numérique de modification des droits (voir l'article précédent, avec 4 pour l'accès en lecture, 2 en écriture et 1 en exécution), mais part de permissions complètes (donc _777_) et y retranche celles données en argument. Avec _umask 021_ par exemple, on obtiendra les droits rwxr-xrw- (777 - 021 = 756). Dans la plupart des distributions, _umask_ est mis à 022 par défaut (soit rwxr-xr-x ou 755).

	$ touch test.txt
	-rwxrwxr--   1   calvin   humains   921   avril 10 15:47   test.txt

[^1]: À noter que la modification des droits par défaut n'est pas persistante (elle ne sera plus valable après redémarrage de l'appareil par exemple). Pour l'affecter de manière permanente, il faudra modifier le profil de démarrage (fichier _.profile_), mais j'aborderai ce point dans un autre article.

<!--
When you run the umask command it will give that default set of permissions on any new file you make. However, if you want it to persist you'll have to modify your startup file (.profile), but we'll discuss that in a later lesson. -->

## Cas spécifique des SUID et SGID

Dans de nombreux cas, des utilisateurs "lambda" ont besoin de droits "privilégiés" pour certaines actions, mais les accès au compte administrateur ne peuvent pas être donnés à tous. Pour ces cas particuliers, il existe une permission spéciale, le **Set User ID (SUID)**, qui permet de lancer une commande en étant considéré comme le propriétaire du fichier.

Prenons un exemple. Disons qu'un utilisateur souhaite changer son mot de passe. Pour cela, il utilisera la commande _passwd_, qui elle même modifiera les fichiers système _/etc/passwd_. Cependant, ce fichier est propriété de l'administrateur et n'es modifiable que par lui. Regardons maintenant les permissions de la commande _passwd_ (fichier exécutable situé dans " _/usr/bin/passwd_ "):

	$ ls -l /usr/bin/passwd
	-rwSr-xr-x 1 root root 54256 mars  29  2016 /usr/bin/passwd

Vous remarquerez le nouveau caractère **s** : il s'agit du SUID. Quand cette permission est mise sur un fichier, n'importe quel utilisateur exécutant ce fichier sera considéré comme son propriétaire, dans notre exemple l'administrateur (_root_). Sans ce droit spécial, il serait impossible à un utilisateur de changer son mot de passe sans passer par le compte administrateur.

Ce champ peut être affecté de manière symbolique comme n'importe quel droit, ou en précédent la représentation numérique des permissions d'un _4_ :

	$ ls -l
	-rw-rw-r--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod u+s test.txt
	-rwSrw-r--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod u-s test.txt
	-rw-rw-r--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod 4755 test.txt 
	-rwsr-xr-x  1 julien julien    0 avril 18 16:06 test.txt

De la même manière, il est possible d'autoriser l'exécution d'une commande en étant considéré comme appartenant au groupe propriétaire du fichier, et cela par l'intermédiaire de la permission **Set Group ID (SGID)**. Ce champ peut être affecté de manière symbolique comme n'importe quel droit, ou en précédent la représentation numérique des permissions d'un _2_ :

	$ ls -l
	-rw-rw-r--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod g+s test.txt
	-rw-rwSr--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod g-s test.txt
	-rw-rw-r--  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod 2555 test.txt 
	-r-xr-sr-x  1 julien julien    0 avril 18 16:06 test.txt

<!-- 

7. Process Permissions

Let's segue into process permissions for a bit, remember how I told you that when you run the passwd command with the SUID permission bit enabled you will run the program as root? That is true, however does that mean since you are temporarily root you can modify other user's passwords? Nope fortunately not!

This is because of the many UIDs that Linux implements. There are three UIDS associated with every process:

When you launch a process, it runs with the same permissions as the user or group that ran it, this is known as an effective user ID. This UID is used to grant access rights to a process. So naturally if Bob ran the touch command, the process would run as him and any files he created would be under his ownership.

There is another UID, called the real user ID this is the ID of the user that launched the process. These are used to track down who the user who launched the process is.

One last UID is the saved user ID, this allows a process to switch between the effective UID and real UID, vice versa. This is useful because we don't want our process to run with elevated privileges all the time, it's just good practice to use special privileges at specific times.

Now let's piece these all together by looking at the passwd command once more.

When running the passwd command, your effective UID is your user ID, let's say its 500 for now. Oh but wait, remember the passwd command has the SUID permission enabled. So when you run it, your effective UID is now 0 (0 is the UID of root). Now this program can access files as root.

Let's say you get a little taste of power and you want to modify Sally's password, Sally has a UID of 600. Well you'll be out of luck, fortunately the process also has your real UID in this case 500. It knows that your UID is 500 and therefore you can't modify the password of UID of 600. (This of course is always bypassed if you are a superuser on a machine and can control and change everything).

Since you ran passwd, it will start the process off using your real UID, and it will save the UID of the owner of the file (effective UID), so you can switch between the two. No need to modify all files with root access if it's not required.

Most of the time the real UID and the effective UID are the same, but in such cases as the passwd command they will change.

 -->


## Le "Sticky bit"

Parlons maintenant d'une dernière permissions spéciale: le "sticky bit". Il est possible de rendre un fichier destructible seulement par son propriétaire et l'administrateur, et ce quelles que soient les permissions sur ce fichier. Prenons l'exemple du répertoire " _/tmp_ " (l'option *-d* de _ls_ permet de lister les répertoires, et non leur contenu):

	$ ls -ld /tmp
	drwxrwxrwt 13 root root 135168 avril 19 14:19 /tmp

Vous remarquerez que le dernier caractère des droits est à **t**. C'est le fameur "**sticky bit**". Ce dossier ne peut donc être supprimé que par l'administrateur, tout en étant modifiable par tous.

Comme pour les SUID et SGID, le "sticky bit" peut être affecté de manière symbolique avec **t** ou de manière numérique en précédent les droits d'un **1** :

	$ ls -l
	-rwxr-xr-x  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod 1755 test.txt
	$ ls -l
	-rwxr-xr-t  1 julien julien    0 avril 18 16:06 test.txt

	$ sudo chmod -t test.txt
	$ ls -l
	-rwxr-xr-x  1 julien julien    0 avril 18 16:06 test.txt



# Recherches par utilisateurs et permissions

Je vous ai montré dans [un précédent article]({{< ref "lfcs-search.md" >}}) comment lister et trouver les fichiers dans le système, en proposant quelques filtres possibles. En voici quelques autres permettent de filtrer en fonction des utilisateurs, groupes et permissions :

 - **find -user _u_** : Liste les fichiers appartenant à l'utilisateur _u_.
 - **find -group _g_** : Liste les fichiers appartenant au groupe _g_.
 - **find -perm** : Liste les fichiers selon leurs permissions:
  - **-perm _p_** : ne liste que les fichiers dont les permissions sont exactement _p_.
  - **-perm _-p_** : liste les fichiers dont les permissions sont au moins _p_.
  - **-perm _/p_** : liste les fichiers dont au moins un des droits de _p_ est setté.

Et en pratique, cela nous donne :

	$ ls -l
	-rw-r--r--  1 root    root     0 avril 18 16:11 supertest.txt
	-rw-r--rw-  1 calvin  humains  0 avril 18 16:07 testbis.txt
	-rw-rw-r--  1 hobbes  tigres   0 avril 18 16:14 testTer.txt
	-rw-rw-r--  1 hobbes  tigres   0 avril 18 16:06 test.txt

	$ find . -user hobbes
	./testTer.txt
	./test.txt

	$ find . -group humains
	./testbis.txt

	$ find . -perm 664
	./testTer.txt
	./test.txt


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
