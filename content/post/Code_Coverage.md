+++
title      = "L'assurance qualité dans le logiciel"
date       = "2017-07-11T12:05:25+02:00"
author     = "jul"
tags       = ["Qualité", "Normes"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

Cela fait un moment que je voulais parler de qualité, sécurité, test et validation du logiciel, mais il s'agit d'un sujet tellement vaste et complexe que je me suis jusque là résigné à ne pas l'aborder. Jusque là, car les prochains articles seront sur ce thème, avec aujourd'hui une rapide introduction sur ce qu'est l'assurance qualité, en particulier dans les [**systèmes critiques**](https://fr.wikipedia.org/wiki/Syst%C3%A8me_critique), qui sera suivie par une suite articles plus détaillés (et probablement moins "théoriques").


# Qualité et sécurité logicielle

Commençons par une citation :

_“During the development of any non-trivial program, software structure is almost always created that cannot be determined from top-level software specifications”_

Qui dit logiciel, dit bug. Que ce soit une dérive par rapport aux spécifications, ou plus clairement un défaut du système, les bugs vont de paire avec le développement. Et plus un projet gagne en taille et en complexité, plus leur probabilité et leur criticité augmente. C'est justement pour limiter ces problèmes (ou plutôt les détecter, car pour enchaîner sur une autre citation de Dijkstra, _"Testing never proves the absence of faults, it only shows their presence"_) qu'ont été mises en place des systèmes de test et d'assurance qualité au niveau logiciel.

L'assurance qualité dans le développement logiciel peut prendre plusieurs formes, allant des simples règles de codage, à l'analyse statique complète du code. Passons en revue les principales pratiques à connaître.


## Architecture modulaire et criticité

La manière dont l'**architecture logicielle** a été conçue impacte énormément la qualité du logiciel final, et dans ce domaine, la **modularité** est le maître mot[^1]. Idéalement, un logiciel complexe doit être décomposé en sous-parties cohérentes et relativement indépendantes, appelées **Modules** ou **_Components_**. Chaque module a ses propres **points d'entrée et de sortie** (les fonctions ou commandes accessibles de l'"extérieur"), dans l'idéal, limités au stric nécessaire.

Cette pratique permet de rendre le logiciel plus cempréhensible et facile à décrire (et donc à documenter), mais surtout elle permet de cloisonner les défauts, et lorsqu'il surviennent de limiter les corrections au module concerné ou ses appels. D'autres bonnes pratique (tels que les **Design patterns** par exemple) permettent de renforcer l'architecture, mais nous les aborderons dans un prochain article.

Une fois que l'architecture est décomposée en modules, il est possible d'affecter à chaque module un [**niveau de criticité**](#Assignation-des-niveaux-de-criticité) en fonction de son rôle proprement dit (voir plus bas pour les cas particuliers du [médical](#Médical--EN-62304) et du [ferroviaire](#Ferroviaire--EN-50128)). Il s'agit d'une action particulèrement importante dans les secteurs normés, car cette évaluation y est obligatoire et impliquera des tests et validations plus ou moins approfondis et rigoureux.

[^1]: Il existe de nombreuses études intéressantes sur le sujet, notamment sur le fait que l'organisation logicielle soit souvent le reflet de l'organisation des équipes de développement : c.f. [Loi de Conway](https://fr.wikipedia.org/wiki/Loi_de_Conway).

## Règles de codage et analyse statique

Une fois l'architecture logicielle bien en place, reste le code lui-même. C'est là qu'entrent en jeu deux aspects : les _règles de codage_ et l'_analyse statique_.

Un projet complexe est en règle générale développé par plusieurs ingénieurs en parallèle. Dans le monde de l'informatique, chaque développeur a une manière de coder qui lui est propre, et un logiciel rédigé à plusieurs mains peut vite devenir illisible si aucun cadre n'est défini. Ce cadre, ce sont les **règles de codage**. Elles définissent les pratiques imposées, comme par exemple le nommage des variables/classes/fonctions/méthodes, le [style d'indentation](https://fr.wikipedia.org/wiki/Style_d%27indentation), ou encore le découpage des fichiers sources, et permettent d'uniformiser le "style d'écriture" du code source, et ainsi le rendre plus lisible et maintenable.

Il existe également une norme de programmation reconnue, devenue standard _de facto_ dans les systèmes critiques en C/C++ : [MISRA](https://fr.wikipedia.org/wiki/MISRA_C). Cette norme garantit un socle commun et extérieur, et donc des règles cohérentes et adaptées. MISRA définit des règles et directives (c'est à dire des règles plus ouvertes, sujettes à interprétation), classées comme "obligatoires" (mandatory), "requises" (required) et "conseillées" (advisory).

Pour en citer quelques unes :

- Règle 8.3 (requise): Pour chaque paramètre de fonction, le type donné dans la déclaration et la définition doivent être identiques, et les types des valeurs de retour doivent aussi être identiques
- Règle 16.5 (requise): Les fonctions sans paramètre doivent être définies et déclarées avec _void_
- Règle 16.10 (requise): Si une fonction renvoie une valeur de retour d'erreur, cette valeur doit être testée.

Le respect de ces règles de codage passe souvent par une vérification et relecture du code (idéalement, par une personne indépendante au projet), et est complétée par une documentation rigoureuse et exhaustive des fonctions d'entrée/sortie des modules (au moins). Certains formalismes de commentaire permettent même à des outils comme [Doxygen](https://fr.wikipedia.org/wiki/Doxygen) de générer une documentation en ligne ou papier.

Ces phases sont cependant loin d'être suffisantes, et une analyse automatique du code source est souvent indispensable : on parle alors d'**analyse statique**. L'analyse statique est effectuée par un logiciel[^2], qui déterminera notamment si les règles de codage et/ou MISRA sont respectés, si il existe du code mort (du code ne pouvant être atteint, quelles que soient les chemins d'exécution), ou encore si certaines parties sont susceptibles de générer des bugs dans certaines conditions (typiquement, détecter les dépassements de tampon ou les accès mémoire).

[^2]: Pour n'en citer que quelques uns, [Parasoft](https://www.parasoft.com/), [Polarion](https://polarion.plm.automation.siemens.com/) ou encore [Polyspace](https://www.mathworks.com/products/polyspace.html)


# Couverture de code

L'analyse statique permet d'évaluer et vérifier l'appel des fonctions et instructions, le passage des paramètres, les points d'évaluation, etc. Tout cela repose sur des principes fondamentaux, regroupés dans ce que l'on appelle la **couverture de code**.

Il existe plusieurs principes, dont :

- **Couverture des fonctions** (_Function coverage_) : "Chaque fonction dans le programme a-t-elle été appelée?"
- **Couverture des instructions** (_Statement coverage_) : "Chaque ligne de code dans le programme a-t-elle été exécutée et vérifiée?"
- **Couverture des points de test** (_Condition coverage_) : "Chaque point d'évaluation (e.g. test d'une variable) a-t-il été exécuté et vérifié?"
- **Couverture de branche** (_Branch coverage_ ou _DD-Path_) : "Chaque branche décisionnelle de chaque point d'évaluation a-t-il été exécuté et vérifié?"
- **Couverture des chemins d'exécution** (_Path coverage_) : "Chaque parcours possible (i.e. chemin d'exécution) a-t-il été exécuté et vérifié?"

Bien sûr, le test de couverture des chemins d'exécution est souvent impossible à réaliser, car pour chaque fonction, l'ensemble des variables doivent être testées. Pour un point d'évaluation booléen, il y aura donc 2^N chemins possibles, avec N le nombre de conditions à remplir. 

Prenons un exemple simpliste :

	int testFunc(int8_t paramA, int8_t paramB)
	{
		if(paramA > 0 || paramB < 88)
		{
			return (paramA / paramB);
		}

		return 0;
	}

Dans ce cas, le code génèrera une erreur pour toute valeur de _paramB_ inférieure à 0. Voyons comment les test peuvent aider à détecter ce cas de figure.

La _couverture des fonctions_ nécessite un simple appel à la fonction, quel que soient les paramètres _paramA_ et _paramB_. Bien sûr, ce test est très limité et souvent jugé inutile, et ne détectera pas le défaut de notre exemple.
Une _Couverture des instructions_ obligerait de réaliser un appel à _testFunc_ de façon à remplir la condition (e.g. avec _paramA_ valant -1), et à ne pas la remplir (e.g. avec _paramA_ valant 3). Là encore, notre défaut reste indétecté.
La _Couverture des points de test_ impose de tester chaque condition (e.g. avec _paramA_ à 0 et paramB à 100, _paramA_ à 0 et paramB à 3, _paramA_ à 1 et paramB à 100, _paramA_ à 1 et paramB à 3). S moins de bien choisir les valeurs tester (et donc réaliser le test manuellement), le défaut ne sera toujours pas détecté.
Enfin la _Couverture des chemins d'exécution_ impose de tester toutes les valeurs de _paramA_ et de _paramB_. Maintenant, notre défaut sera détecté, mais au prix du test de 255^255 cas.

Heureusement, il existe d'autres principes permettant de limiter le nombre de tests à réaliser sans trop réduire la portée des tests. Il est par exemple possible de tester tous les cas, en choisissant des valeurs aux limites (-127, 0, 1, 128 pour _paramA_ et _paramB_), ce qui dans notre cas détecterait le défaut sans avoir à réaliser des tests fastidieux. Je pourrais citer également l'[**Analyse des flux de données**](https://en.wikipedia.org/wiki/Data-flow_analysis) ou _Data-flow analysis_, ou encore de la [**Condition composée**](https://en.wikipedia.org/wiki/Modified_condition/decision_coverage) ou _MC/DC_, mais nous verrons tout cela dans un autre article.

# Quelques exemples de normes logiciel


## Médical : EN 62304

La norme [**EN 62304**](http://www.verifysoft.com/fr_IEC_62304.html) définit les exigences du cycle de vie des logiciels. Comme le logiciel lui-même n'est pas évalué ni contraint directement (seulement par le biais du cycle de développement et de la documentation allant avec), il est simplement demandé que  le logiciel soit validé dans "les règles de l'art". 

La norme définit cependant 3 **"classes de sécurité"** :

- A : aucune blessure ou effet néfaste sur la santé n´est possible
- B : une blessure mineure est possible
- C : la mort ou une blessure grave est possible

Idéalement, chaque module devrait être décomposé en _unités logicielles_ (typiquement les fonctions d'entrée/sortie du module), avec chacun une classe de sécurité donnée. Cependant, les différences d'exigeances entre ces classes est principalement documentaire (liés au process plutôt qu'au logiciel proprement dit). Nous aborderons plus en détail la norme _EN 62304_ dans un prochain article.

| Documentation logiciel requise                        | Classe A | Classe B | Classe C |
|-------------------------------------------------------|:--------:|:--------:|:--------:|
| Planning de développement                             |     X    |     X    |     X    |
| Analyse des besoins logiciels                         |     X    |     X    |     X    |
| Définition de l'architecure logicielle                |     X    |     X    |     X    |
| Design détaillé du logiciel                           |     -    |     X    |     X    |
| Implémentation et vérification des unités logicielles |     X    |     X    |     X    |
| Tests d'intégration logicielle                        |     -    |     X    |     X    |
| Test du système logiciel                              |     -    |     X    |     X    |
| Libération du logiciel                                |     X    |     X    |     X    |


## Ferroviaire : EN 50128

La norme [**EN 50128**](http://www.verifysoft.com/fr_EN_50128_Software_for_Railway_Control_and_Protection_Systems.html) définit 5 niveaux d'intégrité de sécurité (ou [**Safety Integrity Level**](https://en.wikipedia.org/wiki/Safety_integrity_level) ou **SIL**, allant de SIL0 - le moins critique, à SIL4 - le plus critique).

La norme recommande également un niveau de couverture de code, en fonction du niveau _SIL_ :


|                                    | SIL 0 | SIL 1 | SIL 2 | SIL 3 | SIL 4 |
|------------------------------------|:-----:|:-----:|:-----:|:-----:|:-----:|
| Couverture des instructions        |   R   |   HR  |   HR  |   HR  |   HR  |
| Couverture de branche              |   -   |   R   |   R   |   HR  |   HR  |
| Condition composée                 |   -   |   R   |   R   |   HR  |   HR  |
| Analyse de flux de données         |   -   |   R   |   R   |   HR  |   HR  |
| Couverture des chemins d'exécution |   -   |   R   |   R   |   HR  |   HR  |

R = Recommandé ; HR = Hautement recommandé


## Assignation des niveaux de criticité

De manière générale, les normes assignent aux modules et unités logiciels des niveaux de criticité (A, B et C dans le médical, SIL0 à SIL4 dans le ferroviaire, etc.). De plus, les risques doivent généraux doivent être listés et analysés, ce qui permettra d'assigner cette criticité à partir de données quantifiables.
Pour cela, on se base souvent sur une [analyse des modes de défaillance](https://fr.wikipedia.org/wiki/Analyse_des_modes_de_d%C3%A9faillance,_de_leurs_effets_et_de_leur_criticit%C3%A9) (ou AMDEC) ou analyse des risques.

Chaque risque se voit attribuer une **probabilité d'apparition** (avec 1 invraissemblable, 5 fréquent, et 10 permanent), une **gravité** (avec 1 non grave, 5 conséquences matérielles, et 10 danger de mort) et une **probabilité de non détection** (avec 1 infaillible, 5 système de détection en place mais faillible, et 10 aucune détection possible). La **criticité** du système équivaut alors au produit de ces trois valeurs. En générale, une criticité maximale et un rapport entre la criticité de l'AMDEC et la classe de sécurité ou SIL sont établis.



# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Une leçon intéressante sur la couverture de code](https://retis.sssup.it/sites/default/files/lesson15_coverage_and_MCDCtesting.pdf)
- [Une explication de la norme médicale EN 62304](http://www.verifysoft.com/fr_IEC_62304.html) et la [page wikipedia correspondante](https://en.wikipedia.org/wiki/IEC_62304)
- [Une explication de la norme ferroviaire EN 50128](http://www.verifysoft.com/fr_EN_50128_Software_for_Railway_Control_and_Protection_Systems.html)
- [La norme MISRA 2004](http://caxapa.ru/thumbs/468328/misra-c-2004.pdf) et la [page wikipedia correspondante](https://fr.wikipedia.org/wiki/MISRA_C)
- [Un article de l'embarqué sur MISRA](http://www.lembarque.com/fichiers/cms/file/PDF%20MAgazine%20N%C2%B01/033_035_APPLICATION_LDRA.pdf)
