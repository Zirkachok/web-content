+++
title      = "LFCS : Tâches et gestion des ressources"
date       = "2017-06-16T14:59:06+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->

Aujourd'hui, nous allons aborder la question des _tâches_ dans un environnement Linux, leur rôle et fonctionnement, ainsi que la manière dont il est possible de les gérer. De manière générale, plusieurs termes sont utilisés pour désigner une _tâche_ réalisée par le système d'exploitation. On parler parfois de _tâche_, de _job_, de _processus_, ou encore de _thread_, mais la différence entre ces désignations n'est pas toujours évidente. Essayons d'y voir plus clair, en les passant en revue une à une.


# Quel est le rôle du noyau Linux ?

Commencons par expliquer comment fonctionne un système d'exploitation. Un **système d'exploitation** (ou **OS** pour _Operating System_), le **noyau Linux** dans notre cas, est un programme chargé de gérer l'utilisation des ressources (processeur/CPU, mémoire, RAM, etc.) d'un matériel informatique[^1]. Il fournit également des services aux autres programmes pour s'interfacer avec le matériel[^2] ou communiquer entre eux.

[^1]: En général il s'agit d'un ordinateur, mais en _informatique embarquée_ il peut s'agir d'une carte électronique seule, par exemple.
[^2]: Typiquement, ces services permettent de s'attribuer des ressources. On y trouve par exemple des fonctions de création de _threads_ et _processus_, d'allocation mémoire, d'entrée/sortie (I/O pour Input/Output) etc.

L'OS partage les ressources disponibles entre les différentes tâches à exécuter, afin que différents codes puissent être exécutés en "simultané"[^3], c'est ce que l'on appelle l'**ordonnancement de tâches** (ou _task scheduling_). Ces tâches peuvent être de deux formes : les **processus** (_process_ en anglais) et les **threads**.

[^3]: En fait, les programmes sont exécutés chacun à leur tour, mais le passage d'une tâche à l'autre est tellement rapide et fréquent que cela donne l'impression qu'ils le sont en simultané. Lorsque des tâches sont exécutées sur deux matériels différents (et ne partagent donc pas les ressources processeur), on parle de **parallélisme**.


## Les processus

Un **processus** est un exécutable qui a accès à un partie de la mémoire qui lui est réservée. Cette mémoire contient les instructions à exécuter (le code) et les données utilisées par le programme pour fonctionner. Ces données sont elles-même divisées en deux sous-ensembles : la **pile** (ou _stack_) et le **tas** (ou _heap_). La **pile** est la zone mémoire où sont stockées les variables locales et les adresses de retour des fonctions du code exécuté, alors que le **tas** conserve en vrac tout le reste de la mémoire allouée par le programme.

Le noyau conserve les adresses des différentes zones mémoire de chaque processus en cours d'exécution, ainsi que leurs fichiers ouverts, son identifiant unique, etc. Ces informations sont listées dans le répertoire **/proc**, qui contient un sous-répertoire pour chaque processus (le processus _1_ étant celui lancé par le noyau Linux au démarrage). Le plan d'adressage (ou _mapping_) est lui contenu dans le fichier **maps** de chaque processus.

Un processus a aussi une **priorité** qui lui est propre. Plus le processus est prioritaire, plus l'OS lui fournira de ressource processeur (de cycle processeur) lors de l'ordonnancement des tâches.


## Les threads

Les processus sont relativement peu flexibles, car ils ont besoin qu'une partie de la mémoire leur soit dédiée en permanence et doivent passer par le système d'exploitation pour communiquer entre eux. C'est là que les **threads** entrent en jeu. Un _thread_ est en quelque sorte un sous-ensemble de processus, il partage la même mémoire et les mêmes fichiers que le processus correspondant, mais exécute un code différent, là encore en simultané du processus et des autres _threads_[^4].

Pour le noyau Linux, un _thread_ partage donc les zones mémoire du processus auquel il est affilié, mais il s'agira d'une nouvelle entrée pour l'ordonnanceur de tâches.

[^4]: En somme, les pointeurs d'exécution et de pile sont différents pour chaque thread et leur processus.

Souvent on emploie également les termes de **tâches** et de **job**. Une tâche désigne le plus souvent l'action réalisée par un processus ou un _thread_, alors qu'un **job** désigne en général l'action réalisée par un ou plusieurs processus (un navigateur Internet par exemple, qui utilise plusieurs processus, peut être vu comme un _job_). Cependant, ces deux termes ne sont pas formellement définis (contrairement aux processus et _threads_), et leur signification peut donc varier.


<!-- Le problème d'un processus c'est que c'est un peu lourd, cela implique des recopies de mémoire et surtout cela implique de passer par le système pour communiquer avec d'autre processus. Il existe donc un autre concept nommé thread.

Un thread, c'est un découpage d'un processus. Cela veut dire qu'il utilise exactement la même mémoire qu'un processus, ainsi que les mêmes descripteurs de fichiers. Ce qui diffère d'un thread à l'autre ce sont uniquement les pointeurs d'exécution et de pile. ce qui veut dire que 2 threads peuvent exécuter des bouts de code différents d'un même exécutable et avoir des variables locales différentes de celles des autres threads du processus.

Du point de vue du noyau un thread est donc uniquement caractérisé par l'état du processeur spécifiquement à l'intérieur d'un processus. Pour créer un nouveau thread, un thread existant (oeuf ou poule, un processus est déjà un thread) va appeler la fonction pthread_create() qui duplique la pile du thread existant et qui duplique l'état du processeur. Le scheduler du noyau va donc voir une nouvelle entrée. -->



# Gérer les processus

## Lister les processus

Sous linux, de nombreuses commandes permettent d'obtenir des informations sur les processus en cours d'exécution. Comme nous l'avons vu plus haut, chaque processus a un identifiant unique, et un dossier qui lui est propre dans le répertoire **/proc**. Ce dossier contient en particulier les fichiers **maps** et **map_files**, contenant respectivement le plan d'adressage mémoire et les descripteurs de fichiers ouverts par le processus.

Il est donc possible de lister les processus en cours d'exécution (ou _actifs_) en listant les répertoires dans _/proc_. Mais il existe aussi une commande pour cela : **ps**. Par défaut _ps_ affiche les processus actifs sur le terminal courant, mais des options la rendent bien plus puissante :

- **-x** : retourne tous les processus actifs de l'utilisateur courant
- **-ax** : retourne tous les processus actifs pour tous les utilisateurs
- ** -p PID** : retourne le processus actif dont l'identifiant unique est _PID_

_ps_ donne également pour chaque processus des informations basiques sur le processus. Avec l'option **-u**, _ps_ fournit en plus des informations sur l'utilisateur possédant le processus, le pourcentage processeur/CPU et mémoire dédié, etc.

Dans la pratique, cela nous donne :

	$ ps -aux
	USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
	...
	zirka     3282  0.0  0.5 490924 21504 ?        Sl   08:27   0:01 /usr/lib/x86_64-linux-gnu/xfce4/panel-plugins/xfce4-xkb-plugin  8 6291499 xkb-plugin Dispositions du clavier Gre
	zirka     3292  0.0  0.1 287132  4748 ?        Sl   08:27   0:00 /usr/lib/gvfs/gvfs-udisks2-volume-monitor
	root      3295  0.0  0.2 367848  8516 ?        Ssl  08:27   0:00 /usr/lib/udisks2/udisksd --no-debug
	zirka     3303  0.0  0.0 259036  2724 ?        Sl   08:27   0:00 /usr/lib/gvfs/gvfs-goa-volume-monitor
	zirka     3308  0.0  0.1 405112  4900 ?        Sl   08:27   0:00 /usr/lib/gvfs/gvfs-afc-volume-monitor
	...

	$ sudo ls /proc
	1   10   10220   11   ...   3282   3292   3295   3303   3308   ... 


## Identifier l’utilisation des ressources par processus

Nous l'avons vu, la commande _ps_ peut indiquer le pourcentage CPU et mémoire dédié à chaque processus. Cependant, il existe des commandes bien plus efficaces et détaillées pour le faire : **top** et sa variante interractive **htop**. Ces commandes retournent l'ensemble des processus actifs avec leur utilisateur, priorité, utilisation CPU et mémoire et la commande associée, entre autres. _top_ et _htop_ affichent aussi les ressources totales disponibles. Chacune de ces commandes reste ouverte et rafraîchit les informations affichées en continu, jusqu'à ce que la touche _q_ soit pressée. De manière plus spécifique, la commande **vmstat** permet aussi de monitorer l'utilisation de la mémoire virtuelle.

## Changer la priorité d'un processus

L'ordonnanceur de tâches est chargé de gérer les processus, en leur allouant plus ou moins de cycles processeur/CPU en fonction de leur priorité (il s'agit du champs _time_ de la commande _top_). Il est facile de faire l'analogie entre l'ordonnanceur et nous-même lorsque nous planifions nos activités. Parfois, il nous est possible de mener de front plusieurs tâches, et parfois une seule d'entre elle nécessite notre attention complète.

Il est possible de donner des consignes au noyau Linux pour ordonnancer les processus le plus efficacement en fonction des besoins. Cela se fait via un paramètre appelé **valeur nice** ou **priorité** (voir le champ _PR_ de _top_)[^5], allant de -20 à +19 et avec 0 la valeur par défaut. Plus un processus est prioritaire, plus sa _valeur nice_ sera faible, et plus il bénéficiera de ressources CPU lorsque nécessaire. En ajustant cette valeur, il est donc possible d'agir sur les ressources allouées à des processus particuliers (limiter la priorité d'un processus gourmand et non-essentiel, par exemple). Pour cela, il existe deux commandes : _nice_ et _renice_.

[^5]: La _valeur nice_ et la _priorité_ d'un processus sont liés mais ne désignent pas exactement la même chose. Cependant ici nous considèrerons que si, par volonté de simplification.

La commande **nice -n PRIO CMD** permet d'exécuter une commande _CMD_ avec une priorité _PRIO_ donnée. De la même manière, la commande **renice PRIO -p PID** permet de changer la priorité du processus _PID_ en lui attribuant la valeur _PRIO_. Enfin, il est possible de changer la priorité par défaut des processus lancés par un utilisateur ou groupe spécifique, en modifiant le fichier **/etc/security/limits.conf**, comme expliqué dans [mon article sur les limitations de ressources]({{< ref "lfcs-user-limits.md" >}})


# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un article très détaillé sur le fonctionnement des processus et threads](http://linux-attitude.fr/post/processus-et-threads) (FR)
- [Une question/réponse relative aux processus, threads, jobs et tâches](https://stackoverflow.com/questions/3073948/job-task-and-process-whats-the-difference) (EN)
- [Des informations détaillées sur la commande ps](http://www.octetmalin.net/linux/tutoriels/ps-connaitre-afficher-processus-actifs-a-un-moment-donne-instant-en-ligne-de-commande.php) (FR)
- [Un article sur le changement de priorité de processus](https://www.nixtutor.com/linux/changing-priority-on-linux-processes/) (EN)
- [Un article complet sur vmstat](https://www.cyberciti.biz/tips/linux-resource-utilization-to-detect-system-bottlenecks.html) (EN)

<!-- 
https://unix.stackexchange.com/questions/4999/how-to-find-which-processes-are-taking-all-the-memory
https://www.cyberciti.biz/tips/linux-resource-utilization-to-detect-system-bottlenecks.html
 -->