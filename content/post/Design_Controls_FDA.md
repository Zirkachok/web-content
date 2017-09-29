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

Vous souhaitez concevoir un dispositif médical pour le marché Américain? Comme pour tout dispositif médical, vous devrez veiller au respect de normes et procédures tout au long de la conception (et en apporter la preuve!). Mais là où l'Union Européenne (avec son fameux marquage CE) demande _simplement_ que la conception soit réalisée dans "les règles de l'art", les Etats-Unis imposent un **cycle de conception et développement particulier**, régi par un processus de contrôle de la conception ou **Design Control**. Et ce n'est pas la seule différence.


# Design controls?

Aux Etats-Unis, l'organisme de réglementation pour les dispositifs médicaux (_DM_) est la **Food and Drugs Administration** ou **FDA**. Comme pour le marquage CE, l'agréation FDA (_FDA clearance_) repose sur le respect de normes internationnales, notamment :

- l'[**ISO 13485**](https://fr.wikipedia.org/wiki/ISO_13485) qui précise les exigences des **systèmes de management de la qualité (SMQ)**
- l'[**IEC 62304**](http://www.verifysoft.com/fr_IEC_62304.html) qui définit les exigences du **cycle de vie des logiciels**
- l'[**IEC 62366**](https://www.tuv.com/media/france/essentiel/normes/Ergonomy_EN_62366.pdf) qui définit les procédures relatives à l’**ingénierie de l’aptitude à l’utilisation**
- l'[**IEC 60601**](https://www.tuv-sud.com/industry/healthcare-medical-device/testing-assessment/testing-of-active-medical-devices/iec-60601) qui définit les exigences générales pour la sécurité et les performances des appareils électromédicaux
- etc.

En plus de cela, la FDA impose que la conception des dispositifs médicaux suive une certaine méthodologie, qu'elle appelle [**Design Control**](https://en.wikipedia.org/wiki/Design_controls), depuis le lancement du projet jusqu'à son industrialisation / commercialisation.
Les réglementations de la FDA sont définies dans le Code des Réglementations Fédérales ou **Code of Federal Regulations (CFR)**, et les _Design Controls_ le sont au titre 21 (celui réservé à la FDA), partie 820 (régissant les réglementations sur le système qualité) sous-partie 30 (ou pour faire court, 21 CPR Part 820.30). Cette méthodologie est aussi décrite sommairement dans le [**Guide aux Design Control**](https://www.fda.gov/downloads/MedicalDevices/.../ucm070642.pdf) édité par la FDA, et dans la suite de cet article je vais suivre le même plan que ce document, pour en faciliter la lecture.

Bien que les _Design Controls_ puissent s'appliquer à n'importe quel processus de développement, la FDA mets l'accent (et donc, tacitement, recommande) sur le modèle de processus de conception en cascade suivant :

![alt text][logo]

[logo]: https://www.fda.gov/ucm/groups/fdagov-public/documents/image/ucm070635.gif "Waterfall"


Basiquement, les exigences liées au produit, aux utilisateurs et aux normes sont définies et rafinées dans les deux premières étapes, **User Needs** et **Design Input**. Le coeur du développement du produit est alors réalisé (développement logiciel et électronique, conception mécanique, etc.) est alors réalisé dans la phase **Design Process**, et la phase **Design Output** comprends l'ensemble des documents utilisés pour décrire et détailler ce développement. S'ensuit la phase **Design Verification** qui vise à démontrer que le produit développé (donc le _Design Output_) répond aux exigences définies préalablement (donc le _Design Input_). Si tel est le cas, le produit est alors considéré comme un Dispositif Médical en bonne et due forme, et la phase **Design Validation** permet de s'assurer que ce dispositif médical réspecte les besoins initiaux (donc le _User Needs_) et l'utilisation prévue qui en sera faite.

Les _Design Controls_ permettent donc d'encadrer strictement la conception et le développement de tout futur dispositif médical. Mais n'oubliez pas, l'important dans toutes les procédures et en particulier les _Design Controls_ est d'être capable de **prouver** que les choses ont été fait scrupuleusement, dans le bon ordre, et avec les besoins des patients à l'esprit. Lors d'une inspection de la FDA, les documents de référence seront donc demandés, et devront le montrer clairement.

## L'analogie du bâtiment

Une analogie simple peut être faite pour expliquer le rôle de chaque phase dans les _Design Controls_, celle d'un bâtiment :

- La phase **User Needs** correspond à la expression d'un cahier des charges par la maîtrise d'ouvrage : des appartements/pièces pour combien de personnes, espaces communs ou non, ensoleillement souhaité, etc.
- La phase **Design Input** correspond au travail de l'architecte. Il interprète les besoins, définit des règles concrètes et prépare les premières ébauches. Certains sous-traitants sont aussi impliqués pour une première ébauche technique.
- La phase **Design Process** correspond au travail des sous-traitants pour faire naître le bâtiment. C'est aussi à eux que revient le rôle de rafiner les spécifications techniques. A cette étape, les premiers plans de contrôle sont établis, et les premières vérifications de conformité du travail sont opérées.L'ensemble es documents ayant servi à réliser le bâtiment sont compilés dans la phase **Design Output**.
- La phase **Design Verification** correspond à l'expertise réalisée pour s'assurer que les exigences de l'architecte ont été respectées.
- La phase **Design Validation** correspond à l'expertise réalisée pour s'assurer que le cahier des charges a été réspecté.
- Chaque phase inclut des expertises permettant au chef de chantier de s'assurer que le chantier est "dans les clous", ce sont les **Design Reviews**.

Passons maintenant en revue chaque étape pour en détailler le rôle et les exigences qu'elles impliquent. 


# User Needs : Exprimer les besoins et les usages

La phase _User Needs_ a principalement pour objectif de définir le futur dispositif médical selon [deux aspects](https://blog.greenlight.guru/intended-use-and-indications-of-use) :

- Son **utilisation prévue** ou **Intended Use**. Il s'agit du but principal du produit, le problème auquel il cherche à répondre et ses fonctionnalités essentielles, telles que prévus par le fabricant. En somme, on peut parler des "Revendications fabricant".
- Ses **indications thérapeutiques** ou **Indications for Use(s)**. Il s'agit des pathologies que le dispositif devra diagnostiquer/prévenir/atténuer, ainsi qu'une description des patients et des conditions d'utilisation typiques. Cette description incluera donc notamment des information comme les problèmes éventuels des patients (tremblements, troubles de la mémoire, etc.), leur profil type (âge, sexe, hospitalisé ou non, suivi par du personnel médical ou non, etc.), comment l'appareil sera utilisé (usage unique ou multiple, ambulatoire ou non, porté par le patient, quand et dans quel cadre, etc.) et dans quelles conditions (e.g. lieu, température, humidité).

Bien que cette phase ne soit pas définie clairement dans les réglementations fédérales, elle sert de base à l'expression des exigences (voir _Design Input_), et de référence pour s'assurer de la conformité aux besoins des patients (voir _Design Validation_). Elle est donc particulèrement importante pour la bonne conception du produit.


## Documenter les besoins utilisateurs

Comme nous venons de le voir, la phase _User Needs_ a pour but de décrire votre futur dispositif médical et comment il sera utilisé, et de cette description découleront des besoins. N'hésitez pas à poser et documenter les questions essentielles (voir plus haut _utilisation prévue_ et _indications thérapeutiques_), et à y répondre. En somme, cette description constituera votre **Cahier des Charges** ou **Design Brief**.

A ce stade, les besoins doivent être exprimés dans un langage compréhensible de tous les acteurs (et doit donc éviter les termes trop techniques ou le jargon médical par exemple), et peuvent être raisonnablement imprécis (par exemple _"Le dispositif doit être suffisemment petit pour être porté à la ceinture du patient"_ ou _"Le dispositif doit être simple d'utilisation, même pour les personnes âgées"_).

Documentez aussi le profil type de vos utilisateurs potentiels (les patients ET le personnel médical) et la manière dont ils utiliseront le produit. Ces informations serviront de base pour votre **Dossier d'aptitude à l'utilisation** ou **Usability brief**, qui évoluera tout au long de la conception et du développement de votre produit (et donc des phases du _Design Control_). Enfin, c'est encore à cette phase qu'il faut commencer à répertorier et [estimer la gravité des risques](https://soundcloud.com/medical-device-podcast/significant-risk-vs-nonsignificant-risk-devices-whats-the-difference), dans le document d'**Analyse de risques** ou **Risk assessment**. Ces deux document (_Analyse de risques_ et _Aptitude à l'utilisation_) seront mis à jour tout au long du développement du produit (donc jusque la phase _Design Output_). Par exemple, en phase _Design Process_, il peut revenir à l'équipe de développement d'ajouter des risques techniques spécifiques.

La FDA recommande aussi implicitement qu'une **méthodologie de validation** et des **critères d'acceptation objectifs** (les performances essentielles du produit) soient définis à ce stade (c.f. _Design Validation_).

## Planning de conception et de développement

A cette phase, un planning de conception et de développement est aussi demandé. Mais ne vous y méprenez pas, il ne s'agit pas d'un diagramme de Gantt, avec les différentes ressources impliquées, au jour le jour. Le planning sert plutôt ici à :

- Délimiter les différentes phases et préciser leurs objectifs
- Identifier les personnes/services et ressources impliqués et les responsabilités
- Planifier les tâches à réaliser, les livrables de chaque tâche et les principales contraintes temporelles (e.g. chemin critique)
- Identifier les principales revues et seuils décisionnels
- Sélectionner les équipes de relecture, et les procédures à suivre par les relecteurs
- Contrôler la documentation de la conception
- etc.

Un planning précis et réaliste n'est donc pas nécessaire, mais une **description précise des phases et jalon** l'est.


# Design Input : Détailler les exigences liées au produit 

_"La phase **Design Input** définit les exigences physiques et de performences pour un dispositif, qui servent de base pour la conception dudit dispositif."_, [**21 CFR Part 820.30(f)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Comme nous venons de le voir, à ce stade les besoins du produit et des utilisateurs ont été définis et documentés. Cependant, ces besoins sont encore imprécis et peu techniques. Le rôle de la phase [_Design Input_](https://soundcloud.com/medical-device-podcast/medical-device-design-inputs-design-label) est donc de servir d'intermédiaire entre les documents de concept de la phase _User Needs_ (et notamment le _Profil Produit / Cahier des Charges_) et la partie technique, en transformant ces besoins en une liste d'exigences précises et sans abiguïté, formant ainsi les **Spécifications fonctionnelles**. Prenons les exemples suivants :


| Besoin exprimé (phase _User Needs_)                                   | Exigence(s) correspondante(s) (phase _Design Input)                                               |
|-----------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------:|
| Le produit doit être utilisable par le patient sous la douche         | Le dispositif doit être certifié IP 55                                                            |
| Le dispositif doit être discrètement porté par le patient en continu  | Le dispositif doit être d'une dimention maximale de 10cm x 5cm x 3cm                              |
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

_"La phase **Design Output** (ou Résultat de conception) définit le résultat d'un travail de conception [...]. Le résultat de conception se compose du produit à proprement parler, son emballage et étiquettage et du "Device Master Record""_, [**21 CFR Part 820.30(g)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Le _Design Output_ est en quelque sorte **la recette permettant de créer le produit**. Elle peut se décomposer selon trois axes :

**Contrôle qualité** :

- Les spécifications et procédures de l'assurance qualité (tests spécifiques et complémentaires à réaliser, acteurs indépendants, etc.)
- Les résultats de l'analyse de risques (Analyse AMDEC par exemple)
- Le résultat des activités de vérification (plans de test, compte-rendus d'essai, rapports de vérification, etc.)
- Les résultats de tests de biocompatibilité, de charge microbienne, d'émissions électromagnétiques, etc.
- etc.

**Résultats de développement** :

- Les codes source de l'appareil
- L'ensemble des binaires de l'appareil (y compris des mémoires)
- Les schémas de la carte électonique, plasturgie, etc.
- Les instructions de fonctionnement
- etc.

**Industrialisation** :

- Le plan de montage de l'appareil
- Les spécifications des processus
- Les procédures de production et de fabrication
- Les procédures d'installation et de mise en service
- Les spécifications de l'emballage et étiquettage, ainsi que les méthodes et procédures utilisées
- etc.

Ainsi, le _Design Output_ décrit tous les aspects importants de la conception et du développement, et mets l'accent sur leur vérification et validation. C'est aussi à cette étape que le [**Device Master Record** ou **DMR**](https://en.wikipedia.org/wiki/Device_Master_Record) est constitué. Le DMR est un document [défini et exigé par la FDA](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?fr=820.181) (21 CFR 820.181), qui liste et inclut ou fait référence aux documents de cette phase. C'est donc en quelque sorte le coeur de notre "recette".

# Design Review : Evaluer la faisabilité

_"La Revue de Conception (Design review) désigne un examen documenté, complet, et systématique de la conception pour évaluer son adéquation avec les exigences/besoins, sa capacité à y répondre, ainsi qu'identifier les problèmes potentiels"_, [**21 CFR Part 820.30(e)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Vous l'avez sûrement remarqué, le modèle en cascade suggéré par la FDA indique une revue à chaque étape, la [_Design Review_](https://soundcloud.com/medical-device-podcast/how-to-improve-your-medical-device-design-reviews). C'est en quelque sorte un jalon, qui permet d'évaluer le bon avancement du projet, la justesse des choix effectués, et la faisabilité de la conception et plus généralement du produit. Chaque revue est différente, une revue des _Design Inputs_ sera donc centrée sur la justesse des exigences, une autre en _Design Process_ portera plutôt sur les choix techniques. Mais [certains principent s'appliquent dans tous les cas](https://blog.greenlight.guru/design-reviews-best-practices-for-medical-device), et en particulier **deux formes restent et prédominent : l'une interne, centrée sur la capacité de la conception à être mise en oeuvre et en production, l'autre externe, centrée sur les exigences/besoins utilisateurs**.

A chaque revue, une équipe est mise sur pied. Elle comprends les acteurs du projet impliqués dans les phases à revoir, ainsi des relecteurs "indépendants", chargés d'apporter un regard neuf sur le projet. En général, une revue se déroule en trois étapes :

- **Evaluation de la conception**, centrée sur la recherche de potentiels problèmes, incohérences ou manques dans le processus. Idéalement, dans la pratique, le responsable de la phase et son équipe présentent leurs choix et les justifient, devant un panel constitué des correspondants de l'équipe projet, des relecteurs indépendants (leurs alter-ego d'un autre projet/service, ou encore mieux un consultant externe), et des représentants des utilisateurs. Des documents doivent être fournis à ce moment pour appuyer les choix (études cliniques, tests externes, études de faisabilité, etc.), en plus des documents officiels de la phase à évaluer.
- **Résolution des "inquiétudes"**, où chaque problème soulevé est discuté et où des dispositions à prendre pour les combler sont proposées.
- **Implémentation des actions correctives**, où les éventuels changements au design sont décidés. C'est à cette étape que des changements dans les exigences ou dans le processus de conception peuvent intervenir.

Suivant la complexité du projet, la revue peut être une simple formalité ou un exercice complet, à vous d'estimer quel niveau de précision et de rigueur sera nécessaire à chaque étape de la conception.

**Attention, la FDA insiste fortement sur la présence de personnes extérieures et/ou indépendantes lors de la revue, ainsi que sur la prise en compte des utilisateurs finaux (patients et personnel médical). N'oubliez pas aussi d'inclure les revues essentielles et détailler si possible leur contenu dans le plan de conception.**

La phase de revue est l'une des plus importante, mais aussi l'une des plus complexe à préparer. Il est facile de s'y laisser emporter dans des débats en longueur, ou au contraire à survoler et négliger des aspects importants. Voici quelques conseils pour échapper à ces écarts :

- Veillez à ce que la revue soit **préparée par chacun des membres**. Par exemple, les personnes directement impliquées dans la phase peuvent préparer une présentation des choix majeurs opérés, et les relecteurs peuvent lire les documents de référence en avance. Cela aide à limiter les discussions et à les concentrer sur les points les plus critiques.
- Prévoyez **un ordre du jour précis et une check-list** des points importants. Là encore, cela aide à centrer la discussion et assure que l'essentiel est abordé.
- **Sélectionnez les présentateurs et relecteurs avec attention.** Rien de pire qu'un présentateur mal renseigné ou trop timide, ou un relecteur trop pointilleux ou négligeant.

De manière générale, si cette phase est préparée avec rigueur, elle peut devenir une simple formalité ou apporter un regard différent sur le projet. Elle permet aussi de faire se rencontrer et discuter des personnes qui ne sont pas en relation directe la plupart du temps. Alors ne la négligez pas, et votre projet ne projet ne pourra que s'en trouver amélioré.


# Design Verification : Evaluer la conformité aux exigences
_"Chaque fabricant doit établir et maintenir des procédures pour vérifier la conception du dispositif. La phase Design Verification doit confirmer que le produit (Design Output) est conforme aux exigences (Design Inuput). Les résultats de la vérification, la méthodologie, la date et les personnes ayant effectué la vérification doivent être documentés dans le DHF"_, [**21 CFR Part 820.30(f)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

La vérification sert à prouver que le design produit, en somme la première ébauche du dispositif, est conforme aux exigences établies dans la phase _Design Input_. Comme vous pouvez vous en douter, la vérification peut prendre des formes très diverses suivant la nature, le type et la complexité du projet, c'est donc à vous de déterminer quelle méthodologie est appropriée. La plupart des techniques de vérifications sont mises en oeuvre par défaut par les développeurs, il est donc bon de s'inspirer des bonnes pratiques de votre secteur de l'industrie, ou **Good Manufacturing Practices (GMP)**.

De manière générale, la FDA attend essentiellement :

- Une **analyse des risques** doublée d'un **arbre de défaillance**.
- Une **matrice de tracabilité**.
- Une **analyse comparée de la conception** avec un produit précédent, permettant de capitaliser sur les succès ou erreurs passées.
- Des **tests en condition d'utilisation** en condition réelles, par une manipulation de l'appareil et de chaque fonctionnalités/exigences.
- Des **tests aux bordures**, tests de choc thermiques, tests de pression, etc.
- Des preuves que le développement respecte les standards et **bonnes pratiques du domaine**
- etc.

La FDA propose aussi un ["Guide aux principes généraux à la validation logicielle"](https://www.fda.gov/MedicalDevices/ucm085281.htm), et vous pouvez trouver l'inspiration dans mes articles sur la [couverture de code](https://github.com/Zirkachok/web-content/blob/master/content/post/Code_Coverage.md), l'[analyse de risques et des modes de défaillance](), la [traçabilité des exigences]() et bien d'autres à venir.

Une revue de la vérification doit être effectuée après celle-ci pour estimer et résoudre toute faiblesse ou carence, et éventuellement implémenter des actions correctives.

# Design Validation : Evaluer la conformité aux besoins
_"Chaque fabricant doit établir et maintenir des procédure pour valider la conception du dispositif. La phase Design Validation doit être effectuée sur des unités ou lots de production initiales, selon des conditions définies. La _Design Validation_ doit assurer que les dispositifs sont conformes aux _User Needs_ et à l'utilisation prévue, et doivent comporter des tests d'unités de production dans des conditions réelles ou simulées. La _Design Validation_ doit aussi inclure une validation des logiciels et une analyse des risques, lorsqu'approprié. Les résultats de la validation, comprenant une identification de la conception, des méthodes, les dates et les personnes effectuant la validation, doivent être documentés dans le DHF."_, [**21 CFR Part 820.30(g)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Vous l'aurez compris, autant la phase verification assure la conformité des _Design Outputs_ (la "recette") avec les _Design Inputs_ (les exigences), autant **la validation assure la conformité du produit "fini" aux besoins initiaux et à l'utilisation qui en sera faite par les patients et le personnal médical**. Vous trouverez d'ailleurs une discussion intéressante sur la vérification et la validation [ici](https://soundcloud.com/medical-device-podcast/what-device-makers-need-to-know-about-design-verification-and-validation-with-mike-drues).

Il s'agit du passage du produit créé (un prototype ou pré-série) aux utilisateurs décrits initialement, ainsi que des tests assurant qu'aucun écart par rapport aux besoins n'existe, appuyés par des **preuves objectives**. La phase de validation prends donc essentiellement deux formes (c.f. [21 CFR 820.30(z)](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)) : 

- Une **validation des processus**, prouvant qu'**invariablement**, un processus produit un résultat donné, ou que le produit est conforme aux spécification pré-déterminées.
- Une **validation de la conception**, prouvant que les spécifications du dispositif sont conformes à l'utilisation prévue (c.f. _User Needs_) et aux besoins initialement exprimés.

Vous l'aurez compris, la manière dont ont été initialement définis les besoins, le profil type des utilisateurs, les performances essentielles, peut avoir un impact important à ce stade (alors que le produit est au plus proche de sa fabrication). Il est aussi apprécié par la FDA que la méthodologie de validation et des critères objectifs d'acceptation soient définis en amont, idéalement dès la phase _User Needs_. 

Une revue de la validation doit être effectuée après celle-ci pour estimer et résoudre toute faiblesse ou carence, et éventuellement implémenter des actions correctives.

## Méthode de validation

Tout dispositif nécessite d'être appuyé par une **évaluation clinique** (bien que tous ne nécessitent pas d'essais cliniques) dans le cadre de sa validation. Des **tests en conditions réelles ou simulées** doivent être également effectués dans l'environnement d'utilisation prévu (c.f. "Indications thérapeutiques", _User Needs_), avec des appareils (y compris emballage et étiquettage) fabriqués en utilisant les **mêmes méthodes et procédures de production** que celles prévues pour le produit final. Il est aussi bon de fournir à ce stade toute preuve que le produit ou des produits similaires sont cliniquement sûrs, et de bien distinguer les clients du personnel médical, personnel technique et des patients.

Il est également important à ce stade de réaliser un profil des personnes amenées à réaliser l'industrialisation du produit, et de sélectionner les personnes les plus expérimentées pour évaluer le processus, et faire réaliser les produits testés par d'autres.

# Autres phases post-conception

## Design Transfer

_"Chaque fabricant doit établir et maintenir des procédures pour assurer que la conception du dispositif est correctement traduite en spécifications de production"_, [**21 CFR Part 820.30(h)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Le rôle de la phase _Design Transfer_ est de transférer le dispositif médical du développement à la production. Cette phase exige essentiellement qu'une évaluation qualitative de la complétude et adéquation des spécifications de production, répertoriée dans les procédures de conception et de développement. Aussi, chacune de ces spécifications doit être revue et approuvée, et il doit être démontré que seules les procédures valides sont utilisées pour fabriquer le dispositif.

Cette phase n'a pas de fin à proprement parler, et est conduite au fil de l'eau tout au long de la vie du produit.

## Design Changes

_"Chaque fabricant doit établir et maintenir des procédures pour l'identification, documentation, validation et si nécessaires la vérification, revue et approbation des changements dans la conception avant leur implémentation"_, [**21 CFR Part 820.30(i)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)

Au fur et à mesure que le produit évolue, il est important d'y apporter des changements, qu'il s'agissent de correctifs ou de nouvelles fonctionnalités. Deux éléments doivent alors être considérés :

- Le contrôle documentaire (_Document Control_) : il s'agit d'une énumération des documents de conception, leur status et l'historique de leurs révisions.
- Le contrôle des changements (_Change Control_) : il s'agit d'une énumération des défauts et actions correctives venant d'une phase de vérification et de revue, ainsi que d'un suivi de leur résolution avant la phase _Design Transfer_.

# Activités transverses

Certaines activités doivent être implémentées tout au long de la conception du futur dispositif médical. Il s'agit notamment de l'historisation des documents, de la traçabilité des besoins et exigences, et du fil conducteur liant les documents entre eux.

## Gestion de la traçabilité

_" Chaque fabricant doit établir et maintenir un DHF pour chaque type de dispositif. Le DHF doit contenir ou référencer les documents nécessaires à démontrer que la conception a été effectuée en accord avec le plan de conception approuvé et les exigences"_, [**21 CFR Part 820.30(j)**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?FR=820.30)


Le premier document nécessaire est le [**Design History File** ou **DHF**](https://www.fda.gov/RegulatoryInformation/Guidances/ucm070627.htm#_Toc382720792), qui liste l'ensemble des documents produits pour chaque phase, avec leur date de validation, une référence ou un lien audit document, etc. N'oubliez pas non plus son pendant de la phase _Design Output_, le [**Device Master Record** ou **DMR**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?fr=820.181). Et une fois le dispositif médical lançé sur le marché, celui de l'industrialisation, le [**Device History Record** ou **DHR**](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfcfr/CFRSearch.cfm?fr=820.184), et de la surveillance post-market, le [**Medical Device Reporting** ou **MDR**](https://www.fda.gov/MedicalDevices/Safety/ReportaProblem/default.htm).

En plus de cela, il est fortement recommendé d'établir une matrice de traçabilité des exigences, [**Requirements Traceability Matrix** ou **RTM**](https://en.wikipedia.org/wiki/Traceability_matrix). Les inspecteurs de la FDA, lors de leur revue de la conception, voudront voir le chemin parcouru par un besoin, risque ou contrainte d'utilisation, depuis sa définition, expression comme exigence(s), test, vérification et finalement validation. Pour s'y retrouver facilement et ne manquer aucune étape, il est indispensable de mettre en place un mécanisme de traçabilité complet, testable (pour qu'aucune étape ne manque, et que tout besoin ait été suivi) et facilement manipulable.

## Gestion des risques & Aptitude à l'utilisation

En plus de cela, certains documents doivent évoluer au fil de l'avancement du projet, je parle en particulier de l'**Analyse de risques** et de l'**Aptitude à l'utilisation**. [Ces deux "étapes"](https://soundcloud.com/medical-device-podcast/intersection-of-medical-device-usability-and-risk-management) sont le plus souvent initiées dès le lancement du projet, et complétées à chaque phase. Par exemple, au lancement du projet, les risques les plus évidents sont répertoriés, puis plus tard des risques de plus en plus fins et/ou techniques. De même pour l'aptitude à l'utilisation, où le profil type de l'utilisateur et les conditions d'utilisation sont définis dès le lancement, puis suivis d'études cliniques, d'études de maniabilité d'un prototype, de l'interface graphique, etc. 

De toute évidence, lister les risques, les évaluer et trouver des moyens d'atténuation [n'est pas une tâche évidente](https://soundcloud.com/medical-device-podcast/significant-risk-vs-nonsignificant-risk-devices-whats-the-difference), et demande beaucoup de réflexion. En voici les principales étapes :

- Répertorier le risque, ses causes et ses conséquences sur le patient.
- Estimer la gravité du risque (par une [analyse des modes de défaillance](https://fr.wikipedia.org/wiki/Analyse_des_modes_de_d%C3%A9faillance,_de_leurs_effets_et_de_leur_criticit%C3%A9) par exemple).
- Définir des moyens d'atténuation du risque, qui seront alors liés à des exigences.
- Estimer la gravité du risque après la mise en place de moyens d'atténuation, et s'il est raisonnable ou non
- Evaluer la gravité de la combinaison des risques listés

L'ingéniérie à l'aptitude à l'utilisation est aussi une tâche complexe, [qui mériterait à elle seule une explication longue et complexe](http://www.thaymedical.com/blog/). Je l'aborderai donc dans un prochain article.

# Pour aller plus loin

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Une explication assez proche des recommandations de la FDA](https://blog.greenlight.guru/design-controls)
- [Un _podcast_ très complet sur le SMQ, centré sur les exigences de la FDA](https://soundcloud.com/medical-device-podcast)
- [Un blog intéressant sur l'aptitude à l'utilisation](http://www.thaymedical.com/blog/)
