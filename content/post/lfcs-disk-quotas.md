+++
title      = "LFCS : Gérer les quotas disque"
date       = "2017-06-06T11:06:04+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Dans un système où cohabitent plusieurs utilisateurs, un seul d'entre eux peut s'accaparer un grande partie des ressources. Cela se fait bien sûr au détriment des autres utilisateurs, mais peut aussi rendre le système indisponible. Pour éviter ces situations, il est possible d'instaurer des quotas et limites pour chaque utilisaterur sur le nombre de fichiers qu'il peut posséder, ou sur l'espace disque dont il dispose. Ces quotas sont gérés par le systèmes, et peuvent s'appliquer à des utilisateurs autant qu'à des groupes.

# Préparer le terrain

La gestion des quotas est une option du noyau, qui est activée par défaut pour la plupart des distributions Linux[^1]. En plus de cette option, il sera nécessaire d'installer des outils pour gérer leur utilisation, qui peuvent être obtenus via le **paquet quotas**. Ce paquet comporte des utilitaires, des fichiers de configurations, des scripts de démarrage, etc.[^2]

[^1]: Si tel n'est pas le cas, il vous sera nécessaire de recompiler votre noyau en activant cette option (option _CONFIG_QUOTA_ dans la catégorie _File systems_).

[^2]: Il existe aussi un tutoriel en ligne : http://en.tldp.org/HOWTO/Quota.html

Il est ensuite nécessaire d'activer les quotas indépendamment pour chaque système de fichiers. Pour ce faire, nous allons devoir modifier les options des systèmes de fichiers dans le fichier _/etc/fstab/_, et ajouter pour chacun d'entre eux l'option usrquota (pour activer les quotas utilisateurs) et/ou grpquota (pour activer les quotas sur les groupes).

	/dev/sdc5	/home	ext3	usrquota,grpquota	1	1

Cette ligne active dans cet exemple pour la partition _/dev/sdc5_, qui est dans ce cas le sysème de fichers monté dans _/home_, la gestion des quotas utilisateurs et groupes[^3]. Après avoir installé _quota_ et configuré les sysèmes de fichiers, il ne nous reste plus qu'à appliquer les changements, soit en redémarrant le système, soit en remontant les systèmes de fichiers concernés avec la commande **mount -o remount /mount-point** (avec /mount-point le point de montage concerné**.

[^3]: Il est aussi possible qu'il faille activer le service _quota_ au démarrage du système. Référez-vous à l'article associé pour découvrir comment.


<!-- Depending on your distribution, you may need to configure the quota package’s system startup scripts to run when the system boots. Chapter 5 describes startup script management in detail. Typically, you’ll type a command such as chkconfig quota on , but you should check on the SysV scripts installed by your distribution’s quota package. Some distributions require the use of commands other than chkconfig to do this task, as described in Chapter 5. Whatever its details, this startup script runs the quota on command, which activates quota support.

After installing software and making confi guration fi le changes, you must activate the systems. The simplest way to do this is to reboot the computer, and this step is necessary if you had to recompile your kernel to add quota support directly into the kernel. If you didn’t do this, you should be able to get by with less disruptive measures: using modprobe to install the kernel module, if necessary; running the startup script for the quota tools; and remounting the file sstems on which you intend to use quotas by typing mount -o remount /mount-point , where /mount-point is the mount point in question. -->

# Mettre en place les quotas

À présent, _quota_ devrait être pleinement fonctionnel sur votre système, mais les quotas eux-même ne sont pas paramétrés. La commande **edquota** permet alors de le faire. Elle permet d'éditer le fichier de configuration temporaire **/etc/quotatab** qui contrôle les quotas pour l'utilisateur ou le groupe spécifié en paramètre, et les systèmes de fichiers où l'option est activée. Cela nous donne par exemple :

	$ edquota zirka
	Filesystem	blocks	soft 		hard 		inodes	soft 	hard
	/dev/sdc4	97104	1048576		1048576 	1242 	0		0

On retrouve ici le nombre de blocs mémoire[^4] composant le système de fichiers, et le nombre fichiers (i.e. inodes) présents, suivis des limites strictes (**Hard limits**) et non-strictes (**Soft limits**) dans chacun des cas. Les limites strictes ne peuvent pas être dépassées par l'utilisateur, le noyau Linux empêchant explicitement. Les limites non-strictes peuvent par contre être dépassées, mais leur dépassement génèrera un avertissement. Une valeur de _0_ pour une limite désactive la gestion des quotas dans ce cas.

Il est aussi possible de mettre en place une **période de grâce**, via l'option **-t**, valable pour un système de fichier donné. Si un utilisateur dépasse son quota "non-strict" pendant une période plus longue, cette limite sera alors considérée comme si elle était "stricte".

[^4]: La taille des blocs mémoire variant selon le matériel.

<!-- At this point, quota support should be fully active on your computer, but the quotas themselves aren’t set. You can set the quotas by using edquota , which starts the Vi editor (described in Chapter 1, “Exploring Linux Command-Line Tools”) on a temporary configuration file ( /etc/quotatab ) that controls quotas for the user you specify. 
When you exit the utility, edquota uses the temporary configuration file to write the quota information to low-level disk data structures that control the kernel’s quota mechanisms. For instance, you might type edquota sally to edit sally ’s quotas. The contents of the editor show the cur-
rent quota information:

Disk quotas for user sally (uid 21810):
Filesystem	blocks	Soft 		hard 		inodes	soft 	hard
/dev/sdc4	97104	1048576		1048576 	1242 	0		0

The temporary confi guration fi le provides information about both the number of disk blocks in use and the number of inodes in use. (Each fi le or symbolic link consumes a single inode, so the inode limits are effectively limits on the number of files a user may own. Disk
blocks vary in size depending on the fi lesystem and fi lesystem creation options, but they typically range from 512 bytes to 8KiB.) Changing the use information (under the blocks and inodes columns) has no effect; these columns report how many blocks or inodes the user is actually consuming. You can alter the soft and hard limits for both blocks and inodes. The hard limit is the maximum number of blocks or inodes that the user may consume; the kernel won’t permit a user to surpass these limits. Soft limits are somewhat less stringent; users may temporarily exceed soft limit values, but when they do so, the system issues warnings. 

Soft limits also interact with a grace period; if the soft quota limit is exceeded for longer than the grace period, the kernel begins treating it like a hard limit and refuses to allow the user to create more files. You can set the grace period by using edquota
with its -t option, as in edquota -t . Grace periods are set on a per-fi lesystem basis rather than a per-user basis. Setting a limit to 0 (as in the inode limits in the preceding example)
eliminates the use of quotas for that value; users may consume as much disk space or create
as many fi les as they like, up to the available space on the fi lesystem.


When using edquota , you can adjust quotas independently for every fi lesystem for which
quotas are enabled and separately for every user or group. (To edit quotas for a group, use
the -g option, as in edquota -g users to adjust quotas for the users group.)
A few more quota-related commands are useful. The fi rst is quotacheck , which verifi es
and updates quota information on quota-enabled disks. This command is normally run
as part of the quota package’s startup script, but you may want to run it periodically (say,
once a week) as a cron job. (Chapter 7 describes cron jobs.) Although theoretically not
necessary if everything works correctly, quotacheck ensures that quota accounting doesn’t
become inaccurate. The second useful auxiliary quota command is repquota , which sum-
marizes the quota information about the fi lesystem you specify or on all fi lesystems if you
pass it the -a option. This tool can be very helpful in keeping track of disk usage. The
quota command has a similar effect. The quota tool takes a number of options to have
them modify their outputs. For instance, -g displays group quotas, -l omits NFS mounts,
and -q limits output to fi lesystems on which usage is over the limit. Consult quota ’s man
page for still more obscure options. -->

<!-- https://wiki.archlinux.org/index.php/disk_quota -->
