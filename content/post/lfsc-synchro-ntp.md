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

Dans un système Linux, de nombreux services (en particulier réseaux) reposent sur une synchronisation temporelle précise pour opérer correctement. Pour que cette synchronisation se fasse correctement et reste à jour[^1], il est nécessaire de se référer à une source externe via le réseau, en l'occurrence via le **protocole NTP** (pour **Network Time Protocol**).

[^1]: L'heure du système peut plus ou moins dévier suivant les composants électroniques (quartz notamment). C'est ce qu'on appelle la _dérive d'horloge_.

# Fonctionnement global de NTP

Généralement, un système Linux repose sur deux horloge : l'une matérielle appelée horloge temps-réel ou **Real-Time Clock (RTC)**, et l'autre système appelée **System clock**. La première fonctionne en permanence, même ordinateur éteint et tension coupée (elle est le plus souvent maintenue par une pile dédiée). La seconde est gérée (comme son nom l'indique) par le système. À l'amorçage de Linux (_boot_), la RTC est lue et sert de référence à l'horloge système.

<!-- 
Your Linux system will generally have two clocks, a hardware clock/real time clock (RTC) and a system clock.

The hardware clock is physically present and continues to run from battery power even if the system is not plugged into a power source, this is how the time stays in place when there is no power available. 

The system clock runs in the kernel and after getting its initial time from the hardware clock it will then synchronize with an NTP server to become up to date.

We can manually synchronize the hardware clock to the system clock if required, this would generally only be required if there was no NTP server available.
-->

<!--
Here we are going to cover how to configure chronyd or ntpd in Linux to connect to an NTP server and keep time in sync. We only want to use one of these at a time however, having both running at once is not a good idea and may cause conflicts.

Note: These examples are based on the CentOS 7 operating system so steps may vary slightly for other Linux distributions. In this version chronyd is installed by default, however we will still cover the older ntpd for completeness as this is still widely used. Here we are concerned with configuring NTP clients rather than an NTP server.
-->

# Pour aller plus loin...

Voici mes inspirations ayant mené à cet article. Vous y trouverez des ressources vous permettant d'aller plus loin sur le sujet.

- [Un très bon article sur le sujet issu de la (très bonne elle aussi) formation Red Hat](https://www.rootusers.com/how-to-synchronize-time-in-linux-with-ntp-peers/)
