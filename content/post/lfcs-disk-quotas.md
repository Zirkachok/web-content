+++
title      = "LFCS : Gérer les quotas disque"
date       = "2017-06-06T11:06:04+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Dans un système où cohabitent plusieurs utilisateurs, un seul d'entre eux peut s'accaparer un grande partie des ressources. Cela se fait bien sûr au détriment des autres utilisateurs, mais peut aussi rendre le système indisponible. Pour éviter ces situations, il est possible d'instaurer des quotas et limites pour chaque utilisaterur sur le nombre de fichiers qu'il peut posséder, ou sur l'espace disque dont il dispose. Ces quotas sont gérés par le systèmes, et peuvent s'appliquer à des utilisateurs autant qu'à des groupes.

# Préparer le terrain

La gestion des quotas est une option du noyau, qui est activée par défaut pour la plupart des distributions Linux[^1]. En plus de cette option, il sera nécessaire d'installer des outils pour gérer leur utilisation, qui peuvent être obtenus via le **paquet quotas**. Ce paquet comporte des utilitaires, des fichiers de configurations, des scripts de démarrage, etc.[^2]

[^1]: Si tel n'est pas le cas, il vous sera nécessaire de recompiler votre noyau en activant cette option (option _CONFIG\_QUOTA_ dans la catégorie _File systems_).

[^2]: Il existe aussi un [tutoriel en ligne](http://en.tldp.org/HOWTO/Quota.html)

Il est ensuite nécessaire d'activer les quotas indépendamment pour chaque système de fichiers. Pour ce faire, nous allons devoir modifier les options des systèmes de fichiers dans le fichier _/etc/fstab/_, et ajouter pour chacun d'entre eux l'option usrquota (pour activer les quotas utilisateurs) et/ou grpquota (pour activer les quotas sur les groupes).

	/dev/sdc5	/home	ext3	usrquota,grpquota	1	1

Cette ligne active dans cet exemple pour la partition _/dev/sdc5_, qui est dans ce cas le sysème de fichers monté dans _/home_, la gestion des quotas utilisateurs et groupes[^3]. Après avoir installé _quota_ et configuré les sysèmes de fichiers, il ne nous reste plus qu'à appliquer les changements, soit en redémarrant le système, soit en remontant les systèmes de fichiers concernés avec la commande **mount -o remount /mount-point** (avec /mount-point le point de montage concerné**.

[^3]: Il est aussi possible qu'il faille activer le service _quota_ au démarrage du système. Référez-vous à l'article associé pour découvrir comment.

# Mettre en place les quotas

À présent, _quota_ devrait être pleinement fonctionnel sur votre système, mais les quotas eux-même ne sont pas paramétrés. La commande **edquota** permet alors de le faire. Elle permet d'éditer le fichier de configuration temporaire **/etc/quotatab** qui contrôle les quotas pour l'utilisateur spécifié en paramètre (l'option **-g** permet de faire de même pour les groupes), et les systèmes de fichiers ayant l'option activée. Cela nous donne par exemple :

	$ edquota zirka
	Filesystem	blocks	soft 		hard 		inodes	soft 	hard
	/dev/sdc5	97104	1048576		1048576 	1242 	0		0

On retrouve ici le nombre de blocs mémoire[^4] composant le système de fichiers, et le nombre fichiers (i.e. inodes) présents, suivis des limites strictes (**Hard limits**) et non-strictes (**Soft limits**) dans chacun des cas. Les limites strictes ne peuvent pas être dépassées par l'utilisateur, le noyau Linux empêchant explicitement. Les limites non-strictes peuvent par contre être dépassées, mais leur dépassement génèrera un avertissement. Une valeur de _0_ pour une limite désactive la gestion des quotas dans ce cas.

Il est aussi possible de mettre en place une **période de grâce**, via l'option **-t**, valable pour un système de fichier donné. Si un utilisateur dépasse son quota "non-strict" pendant une période plus longue, cette limite sera alors considérée comme si elle était "stricte".

Enfin, il est possible de vérifier et mettre à jour les informations sur les quotas via la commande **quotacheck**, et de résumer ces informations pour un système de fichiers donné (ou pour tous avec l'option **-a**) via la commande repquota.

[^4]: La taille des blocs mémoire variant selon le matériel.


<!-- https://wiki.archlinux.org/index.php/disk_quota -->
