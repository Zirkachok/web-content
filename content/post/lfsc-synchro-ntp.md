+++
title      = "LFCS : Synchronisation temporelle"
date       = "2017-06-30T11:40:46+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- ê î ô û -->

Dans un système Linux, de nombreux services (en particulier liés au réseau) reposent sur une synchronisation temporelle précise pour opérer correctement. Pour que cette synchronisation se fasse correctement et reste à jour[^1], il est nécessaire de se référer à une source externe via le réseau, en l'occurrence via le **protocole NTP** (pour **Network Time Protocol**).

[^1]: L'heure du système peut plus ou moins dévier suivant les composants électroniques (quartz notamment). C'est ce qu'on appelle la _dérive d'horloge_.

# Fonctionnement global de NTP

Généralement, un système Linux repose sur deux horloge : l'une _matérielle_ appelée horloge temps-réel ou **Real-Time Clock (RTC)**, et l'autre _système_ appelée **System clock**. La première fonctionne en permanence, même ordinateur éteint et tension coupée (elle est le plus souvent maintenue par une pile dédiée). La seconde est gérée (comme son nom l'indique) par le système.

À l'amorçage de Linux (_boot_), la RTC est lue par le noyau et sert de référence à l'horloge système. Après cette première étape, le noyau synchronisera alors l'horloge système et la RTC avec celle fournie par un [**serveur NTP**](https://fr.wikipedia.org/wiki/Network_Time_Protocol). Sur certaines distributions, cette synchronisation est aussi effectuée à intervalles réguliers lors du fonctionnement du système.

Il est aussi possible de paramétrer manuellement la RTC, généralement quand aucun serveur NTP n'est disponible. Pour cela, on utilise la commande **hwclock**. Seule, cette commande permet de lire la valeur de la RTC

## Synchroniser RTC et horloge système

La commande [**hwclock**](https://linux.die.net/man/8/hwclock) peut être utilisée pour obtenir (sans option) ou affecter l'horodatage de la RTC. Avec l'option **--hctosys**, l'horloge système sera synchronisée avec la RTC, alors que l'option **--systohc** effectuera l'opération inverse. De plus, les options **--set --date=VAL** permettent d'affecter manuellement l'heure et la date VAL à la RTC. 

La RTC peut aussi plus ou moins dévier suivant les composants électroniques (quartz notamment) et la manière dont ils ont été routés sur la carte. C'est ce qu'on appelle la [**dérive d'horloge**](https://en.wikipedia.org/wiki/Clock_drift), ou _clock drift_. Dans ce cas, il peut être utile de compenser régulièrement cette dérive, en particulier dans les [systèmes embarqués](https://fr.wikipedia.org/wiki/Syst%C3%A8me_embarqu%C3%A9), en ajoutant ou retirant quelques secondes à la RTC. Cette opération se fait avec la commande **hwclock --adjust**.

Inversement, la commande [**date**](https://linux.die.net/man/1/date) permet d'afficher (sans option) ou affecter, via l'option **--set=VAL** l'horodatage de l'horloge système.


## Couches NTP

Le protocole NTP fonctionne via des serveurs, servant de référence de temps. Ces serveurs sont organisés hiérarchiquement par un système de couches (_layers_). Les serveurs à la couche 0 fournissent une valeur temps aussi exacte que possible, et sont typiquement des [horloges atomiques](https://fr.wikipedia.org/wiki/Horloge_atomique). Ensuite, les serveurs de couche 1 sont synchronisés à ceux de la couche 0, et les serveurs de la couche _N+1_ se synchronisent à ceux de la couche _N_[^1], avec un maximum de 15 couches (16 étant par convention la couche d'un système non-synchronisé).

[^1]: Bien évidemment, plus une source est "bas" dans la hiérarchie, plus elle est précise, et donc plus elle est jugée fiable. Dans la pratique, il existe de nombreux serveurs de couche 1 disponibles pour se synchroniser.

<!--

 -->




<!--
Here we are going to cover how to configure chronyd or ntpd in Linux to connect to an NTP server and keep time in sync. We only want to use one of these at a time however, having both running at once is not a good idea and may cause conflicts.

Note: These examples are based on the CentOS 7 operating system so steps may vary slightly for other Linux distributions. In this version chronyd is installed by default, however we will still cover the older ntpd for completeness as this is still widely used. Here we are concerned with configuring NTP clients rather than an NTP server.
-->

# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un très bon article sur le sujet issu de la (très bonne elle aussi) formation Red Hat](https://www.rootusers.com/how-to-synchronize-time-in-linux-with-ntp-peers/)
