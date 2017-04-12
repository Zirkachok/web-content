+++
title      = "LFCS : Dompter la ligne de commande"
date       = "2017-02-12T16:14:27+01:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô -->

Pour bien débuter ma série d'articles concernant Linux, son fonctionnement et son administration, je vous propose de nous plonger dans la fameuse ligne de commande pour en découvrir l'intérêt et le fonctionnement. Et en bonus, je vous livrerai quelques conseils pour y être plus rapide et efficace!

<!-- je vais passer en revue les commandes et rde bases nécessaires pour s'orienter et naviguer au sein du système de fichiers. Je parle bien de "commandes", car ici comme dans toute la suite, nous passerons par la ligne de commande. -->

<!-- Avant toute chose, je recommande d'utiliser une Sandbox (un espace de test dédié et isolé) pour réaliser les exercices et manipulations de cette série. Comme nous allons être amenés à réaliser des manipulations plus ou moins critiques et à altérer le fonctionnement du système, le faire depuis son environnement de tous les jours peut s'avérer risqué si vous ne savez pas exactement ce que vous faites. -->

<!-- Ceci étant dit, plongeons nous dans cette fameuse ligne de commande, et voyons commant nous repérer et naviguer dans un système Linux.  -->

<div class="warning">Il s'agit ici d'une introduction destinée aux débutants. Pour ceux d'entre vous déjà familiers avec la ligne de commande, vous y apprendrez peut-être quelque chose, ou peut-être pas, mais dans tous les cas vous pouvez passer sans scrupules à un article plus avancé.</div>

# La ligne de commande, c'est quoi?

L'interpréteur de commandes (aussi appelé "ligne de commande") permet d'accéder aux fonctions essentielles du système d'exploitation, par le biais de **commandes** données en entrée (dans notre cas, tapées au clavier). Parmi ces fonctions, on retrouve par exemple la manipulation de fichiers, le déplacement dans l'arborescence, la lecture du manuel et de l'aide, et bien d'autres fonctionnalités.

## À quoi bon?

À première vue, l'intérêt de la ligne de commande n'est pas forcément évident, et les applications graphiques peuvent sembler tout aussi efficaces bien plus abordables[^1]. Mais avec l'expérience, il est souvent bien plus rapide et efficace de gérer son système Linux de cette manière que graphiquement. La ligne de commande permet de réaliser dans les moindres détails **toutes** les opérations permises par le système, alors que les outils graphiques ne peuvent réaliser que les opérations usuelles. Pour enfoncer le clou, dans le cas des serveurs entre autres il s'agit même d'un passage obligé (ils n'ont généralement pas de mode graphique). Alors pour un administrateur, un professionnel, ou un utilisateur averti et curieux, impossible d'y échapper, alors autant s'y mettre dès maintenant.

[^1]: Et dans une certaine mesure le sont, quand on ne cherche à réaliser que des opérations courantes)

<!-- ## sh, bash, zsh, ... -->

Il existe de nombreux interpréteurs, les plus courant étant le Bourne Shell (*sh*), le Bourne-Again Shell (*bash*), le Z shell (*zsh*), etc. Chacun a ses particularités[^2], mais inutiler pour le moment (sauf par curiosité) de les essayer tous. Utilisez le plus répendu actuellement (et par défaut sur de nombreuses distributions de Linux) : **bash**.

[^2]: Les différences majeures étant la conformité au standard [POSIX](https://fr.wikipedia.org/wiki/POSIX) et quelques fonctionnalités supplémentaires (e.g. le changement automatique de répertoire avec zsh). Je reviendrai sur ces différences dans un prochain article.

# Gagner en efficacité

Aussi pratique soit-elle, la ligne de commande demande de l'entraînement pour être "domptée". Impossible de faire l'impasse sur la dactylographie et la mémorisation de toutes les commandes et leurs options (pas de panique, ça rentre tout seul avec la pratique), mais il existe aussi de nombreuses commandes et raccourcis pour nous faciliter le travail (et gagner en efficacité et rapidité).

## Se déplacer dans les commandes

Il arrive souvent de devoir se déplacer dans une commande que l'on est en train de taper. Plutôt que d'effacer ou tout recommencer, quelques raccourcis bien pratique permettent de s'y déplacer en un clin d'oeil : 

 - **Ctrl-a** et **Ctrl-e** permettent respectivement de se rendre au début ou à la fin de la ligne courante.
 - **Alt-b** et **Alt-f** permettent de se déplacer vers la gauche ou la droite mot par mot. Il est aussi possible d'utiliser à la place **Ctrl-&larr;** et **Ctrl-&rarr;**.
 - **Ctrl-c** permet d'effacer la ligne courante. **Ctrl-l** ou la commande **clear** permettent d'effacer complètement le terminal courant. Bien pratique pour y voir plus clair!
 - **Ctrl-Shift-c/x/v** remplacent les raccourcis habituels pour copier, couper et coller.
 - **Ctrl-u** efface tout ce qui se trouve à gauche du curseur, et **Ctrl-y** le colle à nouveau à son emplacement courant.

<!-- Ctrl+Alt+F1 entrer en mode ligne de commande seule (non-graphique) -->

Une autre fonctionnalité intéressante est l'auto-complétion. Lorsque l'on entre une commande, il est possible d'appuyer sur la touche **Tab** pour que celle-ci soit automatiquement complétée. En appuyant deux fois sur *Tab*, la liste des complétions possibles est retournée.


## Parcourir l'historique

Sous Linux, les dernières commandes entrées sont mémorisées, et peuvent être parcourues à l'aide des flèches *haut* et *bas*. Il est aussi possible de parcourir l'historique, via la commande **history**. Encore plus pratique, appuyer sur **Ctrl-r** nous fait entrer dans un mode spécifique, qui parcours cet historique lorsque l'on tape une commande, et propose la dernière commande entrée similaire. Ce mode est particulièrement pratique lorsque l'on répète régulièrement les mêmes commandes.

# Trouver de l'aide

Il est juste impossible de mémoriser toutes les commandes, et il n'est pas rare que l'on ait besoin d'une aide pour en connaitre le fonctionnement et les options. Pour cela, il existe le manuel, accessible par la commande **man** suivie de la commande cherchée. On entre alors dans une interface identique à celle de *less*, avec une explication complète de la commande en question. N'hésitez pas à chercher la page de manuel des commandes que nous avons vu jusque là, pour vous familiariser avec cet outil. 

De manière générale, ayez le réflèxe de lire le manuel pour toute nouvelle commande. Vous pourrez constater certaines similitudes (fonctionnement, arguments, etc.), et elles deviendront vite plus facile à retenir.


Essayez d'utiliser ces commandes et raccourcis lors de vos tests, exercices, ou simplement sans raison particulière et vous verrez, vous ne pourrez plus vous en passer. D'ailleurs, cela vaut aussi pour la ligne de commande elle-même, alors [Jetzt geht's los](https://fr.wikipedia.org/wiki/Jetzt_geht's_los)!

<!-- 
REFS :
	https://www.linux.com/learn/intro-to-linux/2017/4/fabulous-bash-navigation-shortcuts
 -->