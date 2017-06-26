+++
title      = "LFCS : Manipulation de fichiers"
date       = "2017-04-03T08:37:02+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++

<!-- â ê î ô û -->
<!-- é è ù à -->

Comme nous l'avons abordé précédemment, Linux gère une grande partie de son fonctionnement (configuration, communication, etc.) par l'intermédiaire de fichier. Avant de pouvoir s'atteler à des points particuliers et complexes du système, il est donc essentiel de savoir comment manipuler des fichiers en ligne de commande. Dans cet article, je vais donc vous présenter des quelques commandes bien utiles pour lire le contenu, éditer, comparer, etc. des fichiers.

# Lire du contenu

Je vous ai présenté dans mon précédent article comment créer, lister et détruire des fichiers, l'étape suivante est donc d'apprendre à visualiser leur contenu. Pour cela, il existe de nombreuses commandes, chacune ayant ses spécificités. En voici les principales :

- **file** : retourne le type d'un "fichier" (répertoire, exécutable, données, etc.)
- **cat** (pour "*ConcATenate*") : concatène et affiche le contenu du/des fichier(s) texte donné(s) en argument(s). Idéal pour lire les petits fichiers
- **less** : affiche le contenu d'un fichier texte de manière paginée, en entrant dans une interface dédiée. Cette interface réagit à des commandes clavier spécifiques :





- **q** : permet de quitter l'interface de *less*
- **g** ou **<** : se déplace à la première ligne du fichier
- **G** ou **>** : se déplace à la dernière ligne du fichier
- **/XYZ** : cherche la chaîne de caractères *XYZ* dans le fichier. **n** permet d'afficher la prochaine occurrence
- **h** : affiche l'aide de *less* et résume les commandes les plus importantes


# Éditer un fichier

Ma solution préférée pour éditer (et lire) des fichiers reste **vim**. _vim_ est extrêmement flexible et puissant, et nécessiterait une série d'articles à part entière pour en détailler le fonctionnement. Pour le moment, je ne saurais trop vous recommander XXXXXXXXXXXXXXXXXXXX

Une alternative intéressante et plus abordable à _Vim_ est **nano**.

Il existe aussi bien sûr de nombreux outils graphiques plus ou moins complexes, plus ou moins puissants, tels que **Atom**, **Kate**, **Sublime Text**, **gedit**, **Visual Studio**, etc.


Pour éditer ces fichiers, il nous faudra utiliser une application spécifique, ma préférence allant pour **vim**, mais cela fera l'objet d'un autre article. Nous verrons aussi plus tard quelques méthodes de manipulation avancée sur ces fichiers (trier et filtrer les données, etc.).


# Comparer des fichiers entre eux

Fichiers texte : diff
fichiers binaires : vbindiff


wc

<!-- http://www.ai.univ-paris8.fr/~fb/Cours/Exposes0405-1/diff.pdf -->


<!-- https://www.generation-nt.com/reponses/diffa-rence-entre-deux-fichiers-binaires-entraide-3946901.html -->


<!--
## Historique et auto-complétion

Sous Linux, les dernières commandes entrées sont mémorisées, et peuvent être parcourues à l'aide des flèches *haut* et *bas*. Il est aussi possible de parcourir l'historique, via la commande **history**. Encore plus pratique, appuyer sur **Ctrl-r** nous fait entrer dans un mode spécifique, qui lorsque l'on tape une commande parcours cet historique et propose la dernière commande entrée similaire. Ce mode est particulièrement pratique lorsque l'on répète régulièrement les mêmes commandes.

Une autre fonctionnalité intéressante est l'auto-complétion. Lorsque l'on entre une commande, il est possible d'appuyer sur la touche **Tab** pour que celle-ci soit automatiquement complétée. En appuyant deux fois sur *Tab*, la liste des complétions possibles est retournée.

Pour finir, on pourrait citer la commande **clear**, qui nettoie le terminal en mettant la ligne courante tout en haut. Il est aussi possible d'utiliser le raccourci **ctrl-L** pour cette action. Bien pratique pour y voir plus clair. -->
