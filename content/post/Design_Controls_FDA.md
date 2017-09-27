+++
title      = "Dispositifs médicaux aux Etats-Unis : FDA Design Controls"
date       = "2017-09-27T08:52:36+02:00"
author     = "jul"
tags       = ["Qualité", "Normes"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

Vous souhaitez concevoir un dispositif médical pour le marché Américain? Comme pour tout dispositif médical, vous devrez veiller au respect de normes et procédures tout au long de la conception (**et en apporter la preuve!**). Mais là où l'Union Européenne (avec son fameux marquage CE) demande _simplement_ que la conception soit réalisée dans "les règles de l'art", les Etats-Unis imposent un cycle de développement particulier, régi par un processus de contrôle de la conception ou **Design Control**. Et ce n'est pas la seule dissérence.


# Design controls?

Aux Etats-Unis, l'organisme de réglementation pour les dispositifs médicaux (_DM_) est la Food and Drugs Administration ou **FDA**. Comme pour le marquage CE, l'agréation FDA (_FDA clearance_) repose sur le respect de normes internationnales, notamment :

	- l'[**ISO 13485**](https://fr.wikipedia.org/wiki/ISO_13485) qui précise les exigences des **systèmes de management de la qualité (SMQ)**
	- l'[**IEC 62304**](http://www.verifysoft.com/fr_IEC_62304.html) qui définit les exigences du **cycle de vie des logiciels**
	- l'[**IEC 62366**](https://www.tuv.com/media/france/essentiel/normes/Ergonomy_EN_62366.pdf) qui définit les procédures relatives à l’**ingénierie de l’aptitude à l’utilisation**
	- l'[**IEC 60601**](https://www.tuv-sud.com/industry/healthcare-medical-device/testing-assessment/testing-of-active-medical-devices/iec-60601) qui définit les exigences générales pour la sécurité et les performances des appareils électromédicaux
	- etc.

En plus de cela, la FDA impose que la conception des dispositifs médicaux suive une certaine méthodologie, qu'elle appelle [**Design Control**](https://en.wikipedia.org/wiki/Design_controls), depuis le lancement du projet jusqu'à son industrialisation / commercialisation.
Les réglementations de la FDA sont définies dans le Code des Réglementations Fédérales ou **Code of Federal Regulations (CFR)**, et les _Design Controls_ le sont au titre 21 (celui réservé à la FDA), partie 820 (régissant les réglementations sur le système qualité) sous-partie 30 (ou pour faire court, 21 CPR Part 820.30). Cette méthodologie est aussi décrite sommairement dans le [guide aux Design Control](https://www.fda.gov/downloads/MedicalDevices/.../ucm070642.pdf) édité par la FDA, et dans la suite de cet article je vais suivre le même plan que ce document, pour en faciliter la lecture.

Bien que les _Design Controls_ puissent s'appliquer à n'importe quel processus de développement, la FDA mets l'accent (et donc, tacitement, recommande) sur le modèle de processus de conception en cascade suivant :

fda_conception_flowdown.gif

Basiquement, les exigences liées au produit, aux utilisateurs et aux normes sont définies et rafinées dans les deux premières étapes, **User Needs** et **Design Input**. Le coeur du développement du produit est alors réalisé (développement logiciel et électronique, conception mécanique, etc.) est alors réalisé dans la phase **Design Process**, et la phase **Design Output** comprends l'ensemble des documents utilisés pour décrire et détailler ce développement. S'ensuit la phase **Design Verification** qui vise à démontrer que le produit développé (donc le _Design Output_) répond aux exigences définies préalablement (donc le _Design Input_). Si tel est le cas, le produit est alors considéré comme un Dispositif Médical en bonne et due forme, et la phase **Design Validation** permet de s'assurer que ce dispositif médical réspecte les besoins initiaux (donc le _User Needs_) et l'utilisation prévue qui en sera faite.

Les _Design Controls_ permettent donc d'encadrer strictement la conception et le développement de tout futur dispositif médical. Mais n'oubliez pas, l'important dans toutes les procédures et en particulier les _Design Controls_ est d'être capable de **prouver** que les choses ont été fait scrupuleusement, dans le bon ordre, et avec les besoins des patients à l'esprit. Lors d'une inspection de la FDA, les documents de référence seront donc demandés, et devront le montrer clairement.

Passons maintenant en revue chaque étape pour en détailler le rôle et les exigences qu'elles impliquent. 


# User Needs : Exprimer les besoins du produit et des utilisateurs

La phase _User Needs_ a principalement pour objectif de définir le futur dispositif médical selon [deux aspects](https://blog.greenlight.guru/intended-use-and-indications-of-use) :

	- Son _utilisation prévue_ ou **Intended Use**. Il s'agit du but principal du produit, le problème auquel il cherche à répondre et ses fonctionnalités essentielles, telles que prévus par le fabricant. En somme, on peut parler des "Revendications fabricant".
	- Ses _indications thérapeutiques_ ou **Indications for Use(s)**. Il s'agit des pathologies que le dispositif devra diagnostiquer/prévenir/atténuer, ainsi qu'une description des patients et des conditions d'utilisation typiques. Cette description incluera donc notamment des information comme les problèmes éventuels des patients (tremblements, troubles de la mémoire, etc.), leur profil type (âge, sexe, hospitalisé ou non, suivi par du personnel médical ou non, etc.), comment l'appareil sera utilisé (usage unique ou multiple, ambulatoire ou non, porté par le patient, quand et dans quel cadre, etc.) et dans quelles conditions (e.g. lieu, température, humidité).

Bien que cette phase ne soit pas définie clairement dans les réglementations fédérales, elle sert de base à l'expression des exigences (voir _Design Input_), et de référence pour s'assurer de la conformité aux besoins des patients (voir _Design Validation_). Elle est donc particulèrement importante pour la bonne conception du produit.


## Documenter les besoins utilisateurs

Comme nous venons de le voir, la phase _User Needs_ a pour but de décrire votre futur dispositif médical et comment il sera utilisé, et de cette description découleront des besoins. N'hésitez pas à poser et documenter les questions essentielles (voir plus haut _utilisation prévue_ et _indications thérapeutiques_), et à y répondre. En somme, cette description constituera votre **Cahier des Charges** ou **Design Brief**.

A ce stade, les besoins doivent être exprimés dans un langage compréhensible de tous les acteurs (et doit donc éviter les termes trop techniques ou le jargon médical par exemple), et peuvent être raisonnablement imprécis (par exemple "Le dispositif doit être suffisemment petit pour être porté à la ceinture du patient" ou "Le dispositif doit être simple d'utilisation, même pour les personnes âgées").

Documentez aussi le profil type de vos utilisateurs potentiels (les patients ET le personnel médical) et la manière dont ils utiliseront le produit. Ces informations serviront de base pour votre **Dossier d'aptitude à l'utilisation** ou **Usability brief**, qui évoluera tout au long de la conception et du développement de votre produit (et donc des phases du _Design Control_). Enfin, c'est encore à cette phase qu'il faut commencer à répertorier et estimer la gravité des risques, dans le document d'**Analyse de risques** ou **Risk assessment. Ces deux document (_Analyse de risques_ et _Aptitude à l'utilisation_) seront mis à jour tout au long du développement du produit (donc jusque la phase _Design Output_). Par exemple, en phase _Design Process_, il peut revenir à l'équipe de développement d'ajouter des risques techniques spécifiques.


## Planning de conception et de développement

A cette phase, un planning de conception et de développement est aussi demandé. Mais ne vous y méprenez pas, il ne s'agit pas d'un diagramme de Gantt, avec les différentes ressources impliquées, au jour le jour. Le planning sert plutôt ici à :

	- Délimiter les différentes phases et préciser leurs objectifs
	- Identifier les personnes/services et ressources impliqués et les responsabilités
	- Planifier les tâches à réaliser, les livrables de chaque tâche et les principales contraintes temporelles (e.g. chemin critique)
	- Identifier les principales revues et seuils décisionnels
	- Sélectionner les équipes de relecture, et les procédures à suivre par les relecteurs
	- Contrôler la documentation de la conception
	- etc.

Un planning précis et réaliste n'est donc pas nécessaire, mais une description précise des phases et jalon l'est. 


# Design Input : Détailler les exigences liées au produit 

_La phase **Design Input** définit les exigences physiques et de performences pour un dispositif, qui servent de base pour la conception dudit dispositif._ 
21 CFR Part 820.30(f)

Comme nous venons de le voir, à ce stade les besoins du produit et des utilisateurs ont été définis et documentés. Cependant, ces besoins sont encore imprécis et peu techniques. Le rôle de la phase _Design Input_ est donc de servir d'intermédiaire entre les documents de concept de la phase _User Needs_ (et notamment le _Profil Produit / Cahier des Charges_) et la partie technique, en transformant ces besoins en une liste d'exigences précises et sans abiguïté, formant ainsi les **Spécifications fonctionnelles**. Prenons les exemples suivants :


| Besoin exprimé (phase _User Needs_)                                   | Exigence(s) correspondante(s)                                                                     |
|-----------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------:|
| Le produit doit être utilisable sous la douche                        | Le dispositif doit être certifié IP 55                                                            |
| Le dispositif doit être portable                                      | Le dispositif doit être d'une dimention maximale de 10cm x 5cm x 3cm                              |
|                                                                       | Le poids maximal du dispositif doit être de 200g                                                  |
|                                                                       | Le dispositif doit être résistant aux chocs et vibrations                                         |
| Le dispositif doit être simple d'utilisation par des personnes âgées  | La charte graphique doit prévoir des éléments (icônes, boutons, etc.) larges d'au moins 2cm x 2cm |
| Le cathéter doit pouvoir injecter jusque 10mL de médicament par heure | Le diamètre du cathéter doit être de 1mm +- 0.005mm                                               |


Les exigences essentielles listées doivent être nécessaires et suffisantes, c'est à dire qu'elles doivent suffire à décrire précisément le produit, sans pour autant rentrer dans des aspects techniques "superflus". Cette distinction est complexe car la frontière entre un niveau de précision "suffisant" et "trop élevé" est difficile à trouver. En général, à cette phase doivent être définis :

	- Les **exigences fonctionnelles** : ce que le dispositif fait, ses capacités de fonctionnement et contraintes opérationnelles, ses données d'entrée et de sortie, etc.
	- Les **exigences de performance** : le niveau de performance du dispositif (durée de vie, autonomie, précision de mesure, etc.). Chaque valeur doit être précise et éventuellement comporter une marge de précision. Cela comprends également une définition quantitative de l'environnement d'utilisation (température, humidité, chocs, vibrations, compatibilité électromagnétique, etc.)
	- Les **exigences d'interface** : caractéristiques de compatibilité du dispositif avec des systèmes externes (et non contrôlés). Il peut s'agir du patient, de l'utilisateur, de logiciels/appareils communiquant avec le dispositif, etc.

De manière générale, ces exigences sont héritées de l'_Analyse de risques_, de l'_Aptitude à l'utilisation_ et du _Cahier des chages_, et étendues par besoin de complétude. Les standards industriels doivent aussi y être cités et examinés. Enfin, des tests complets doivent être détaillés pour couvrir l'ensemble des exigences, avec des critères de réussite/échec et une méthodologie de test, et les limites de performance du dispositif doivent y être explicitées.


# Design Output : La "recette" du futur produit

_La phase **Design Output** (ou Résultat de conception) définit le résultat d'un travail de conception [...]. Le résultat de conception se compose du produit à proprement parler, son emballage et étiquettage et du "Device Master Record"_
21 CFR Part 820.30(g)


# Design Review

_La Revue de Conception (Design review) désigne un examen documenté, complet, et systématique de la conception pour évaluer son adéquation avec les exigences/besoins, sa capacité à y répondre, ainsi qu'identifier les problèmes potentiels_
21 CFR Part 820.30(h)


# Design Verification

Conformité du produit aux exigences 

# Design Validation

# Design Transfer

# Design Changes

Maintaining a Design History File
Why capturing User Needs sets the stage
The purpose behind Design & Development Planning
Understanding the role and importance of Design Reviews
Becoming a Design Input artist
Defining how to make your medical device through Design Outputs
Proving you designed correctly via Design Verification
Proving you have the correct device via Design Validation
Completing Design Transfer

# Bonnes pratiques générales

DHF & RTM


# Pour aller plus loin

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un _podcast_ très complet sur le SMQ, centré sur les exigences de la FDA](https://soundcloud.com/medical-device-podcast)

