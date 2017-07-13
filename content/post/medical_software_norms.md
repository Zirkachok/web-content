+++
title      = "Normes dans le logiciel médical"
date       = "XXXXXXXXXXXXXX"
author     = "jul"
tags       = ["Qualité", "Normes"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

De nombreux secteurs, typiquement le militaire, les transports, le médical, etc. reposent sur des [systèmes dits critiques](https://fr.wikipedia.org/wiki/Syst%C3%A8me_critique), qui eux-mêmes sont contrôlés de manière "indépendante" et extérieure afin d'assurer que tout a été fait pour qu'ils ne présentent pas de défauts majeurs. Ces systèmes reposent sur et sont contraints par des normes, spécifiques à chaque secteur.

Aujourd'hui, je vais vous présenter l'un de ces secteurs, nommément les [dispositifs médicaux](https://fr.wikipedia.org/wiki/Dispositif_m%C3%A9dical) ou **_DM_**, et passer en revue les étapes nécessaires pour concevoir un dispositif médical "dans les règles de l'art", en particulier en ce qui concerne le sous-système logiciel.

<div class="warning">La présentation des normes médicales et de leurs conséquences n'est qu'une interprétation personnelle, basée sur ma propre expérience. Et en ce qui concerne les normes, une grande partie est sujette à interprétation. Mes remarques et conseils ne sont donc pas uniques et pas non plus forcément les plus adaptés à votre cas de figure.</div>


# Les DM, en substance

Au niveau européen, et dans de nombreux pays s'allignant sur ce modèle, de nombreuses normes et standards internationaux définissent les exigeances sur les dispositifs électro-médicaux (ici simplifiés par DM)[^1].

Parmis ces exigeances, j'en ai retenu 4 principales, qui à mon sens décrivent la procédure générale à appliquer pour la conception des DM, en particulier pour ce qui concerne le sous-système logiciel :

- [**EN 60601**](https://en.wikipedia.org/wiki/IEC_60601), compilant les standards techniques liés à la sécurité et les "perfomances" des DM
- [**ISO 13485**](https://fr.wikipedia.org/wiki/ISO_13485), qui définit comment doit être géré le dispositif, tout au long de son cycle de vie ('est ce que l'on appelle le [**système de management de la qualité**](https://fr.wikipedia.org/wiki/Syst%C3%A8me_de_management_de_la_qualit%C3%A9) ou **SMQ**);
- [IEC 62304](https://en.wikipedia.org/wiki/IEC_62304), qui s'intéresse plus particulièrement au cycle de vie logiciel;
- [IEC 62366](https://en.wikipedia.org/wiki/IEC_62366), qui s'occupe de la partie liée à l'aptitude à l'utilisation.

[^1]: Les dispositifs médicaux vont des compresses stériles aux véhicules d'intervention d'urgence, et nous ne parlerons donc ici que des dispositifs électro-médicaux, dispositifs médicaux contrôlés par de l'électronique et éventuellement de l'informatique.


# Processus de conception


- Phase d'étude du marché et des utilisateurs / User needs




Comme dans tous les systèmes normés, la traçabilité est un aspect primordial. Il est donc important d'associer un marqueur à chaque exigeance, spécification, test afin d'en tracer les répercussions au fil des documents.



# Impact des exigeances sur le logiciel

La norme [**EN 62304**](http://www.verifysoft.com/fr_IEC_62304.html) définit les exigences du cycle de vie des logiciels. Le logiciel lui-même n'est pas évalué ni contraint directement (seulement par le biais du cycle de développement et de la documentation allant avec), il est simplement demandé que le logiciel soit validé dans "les règles de l'art". Bien sûr, cette notion vague peut laisser perplexe, mais elle cache un ensemble de principes communément admis dans le domaine des systèmes critiques.


<!--
https://www.fda.gov/RegulatoryInformation/Guidances/ucm070627.htm
-->


<!--
La norme définit cependant 3 **"classes de sécurité"** :

- A : aucune blessure ou effet néfaste sur la santé n'est possible
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
-->





Besoins ==> Spécifications / risques ==> Conception ==> Vérification ==> Validation