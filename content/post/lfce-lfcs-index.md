+++
title      = "Index des prérequis LFCS-LFCE"
date       = "2017-04-18T11:05:07+02:00"
author     = "jul"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

<!-- â ê î ô û -->


Chose promise, chose due. Voici donc un index des compétences officiellement requises par la Fondation Linux pour les certifications [LFCS](https://training.linuxfoundation.org/certification/lfcs) et [LFCE](https://training.linuxfoundation.org/certification/lfce).

<div class="warning">Cette page sera mise à jour au fur et à mesure de l'écriture des articles de blog. Elle est donc vouée à être constamment en chantier (du moins jusqu'à la fin du tutoriel LFCS-LFCE.</div>

Mais avant cela, voici ce qeu dit la Fondation Linux sur cette liste de compétences (en VF) :

_La fondation Linux a travaillé de concert avec des experts de l'industrie et la communauté du noyau Linux pour identifier les principaux domaines et les compétences, connaissances et aptitudes essentielles pour chaque certification._

_Vous trouverez ci-dessous un sous-ensemble de la liste complète des domaines et compétences couverts par l'examen du LFCS/LFCE. Ce sous-ensemble représente les sujets ayant le plus de chances d'apparaître en considérant les technologies employées, les questions et les contraintes de temps de l'examen._

# Linux Foundation Certified Sysadmin (LFCS)

## Commandes essentielles (25%)

- Se connecter en mode graphique et texte
- <span style="color:red">Effectuer des recherches sur des fichiers</span> : [[ici]]({{< ref "lfcs-search.md" >}}) et [[là]]({{< ref "lfcs-rights-2.md" >}})
- Évaluer et comparer les fonctionnalités et options des systèmes de fichiers de base
- Comparer, créer et éditer des fichiers texte
- Comparer des fichiers binaires
- Utiliser les redirections des flux d'entrée et de sortie (e.g. >, >>, |, 2>)
- Analyser du texte en utilisant les expressions régulières de base
- <span style="color:red">Archiver, sauvegarder, compresser et décompresser des fichiers</span> : [[ici]]({{< ref "lfcs-archive.md" >}})
- <span style="color:red">Créer, supprimer, copier et déplacer des fichiers et répertoires</span> : [[ici]]({{< ref "lfcs-navigate.md" >}})
- <span style="color:red">Créer des liens physiques et symboliques</span> : [[ici]]({{< ref "lfcs-links.md" >}})
- <span style="color:red">Lister, affecter et modifier les permissions standard de fichiers</span> : [[ici]]({{< ref "lfcs-rights.md" >}}) et [[là]]({{< ref "lfcs-rights-2.md" >}})
- <span style="color:red">Lire et utiliser la documentation système</span> : [[ici]]({{< ref "lfcs-man.md" >}})
- Gérer les accès au compte superutilisateur (root)

Avancement : <span style="color:red">12%</span> / 25% (6/13)

<!-- 
Essential Commands - 25%

    Log into graphical and text mode consoles
    Search for files
    Evaluate and compare the basic file system features and options
    Compare, create and edit text files
    Compare binary files
    Use input-output redirection (e.g. >, >>, |, 2>)
    Analyze text using basic regular expressions
    Archive, backup, compress, unpack, and uncompress files
    Create, delete, copy, and move files and directories
    Create hard and soft links
    List, set, and change standard file permissions
    Read, and use system documentation
    Manage access to the root account
-->

## Fonctionnement des systèmes (20%)

- Démarrer, redémarrer et éteindre le système en toute sécurité
- Démarrer les systèmes manuellement sous différents niveaux de fonctionnement (runlevels)
- Installer, configurer et dépanner le bootloader
- Changer la priorité d'un processus
- Identifier l'utilisation des ressources par processus
- Localiser et analyser les fichiers de logs système
- Programmer des tâches pour se lancer à une certaine date et heure
- Vérifier la complétion de tâches planifiées

Avancement : <span style="color:red">0%</span> / 20% (0/17)

<!--
Operation of Running Systems - 20%

    Boot, reboot, and shut down a system safely
    Boot systems into different runlevels manually
    Install, configure and troubleshoot the bootloader
    Change the priority of a process
    Identify resource utilization by process
    Locate and analyze system log files
    Schedule tasks to run at a set date and time
    Verify completion of scheduled jobs
    Update software to provide required functionality and security
    Verify the integrity and availability of resources
    Verify the integrity and availability of key processes
    Change kernel runtime parameters, persistent and non-persistent
    Use scripting to automate system maintenance tasks
    Manage the startup process and services
    List and identify SELinux/AppArmor file and process contexts
    Configure and modify SELinux/AppArmor policies
    Install software from source
-->

## Gestion des utilisateurs et des groupes (15%)

- <span style="color:red">Créer, supprimer et modifier les comptes d'utilateurs locaux</span> : [[ici]]({{< ref "lfcs-rights.md" >}}) et [[là]]({{< ref "lfcs-rights-2.md" >}})
- <span style="color:red">Créer, supprimer et modifier les groupes locaux et leur appartenance</span> : [[ici]]({{< ref "lfcs-rights.md" >}}) et [[là]]({{< ref "lfcs-rights-2.md" >}})
- Manage system-wide environment profiles
- Manage template user environment
- <span style="color:red">Configuer les limites de ressources des utilisateurs</span> : [[ici]]({{< ref "lfcs-user-limits.md" >}})
- Gérer les processus utilisateurs
- Configurer le système d'authentification PAM 

Avancement : <span style="color:red">7%</span> / 15% (3/7)

<!--
User and Group Management - 15%

    Create, delete, and modify local user accounts
    Create, delete, and modify local groups and group memberships
    Manage system-wide environment profiles
    Manage template user environment
    Configure user resource limits
    Manage user processes
    Configure PAM
-->

## Réseaux (15%)

<!-- Networking - 15%

    Configure networking and hostname resolution statically or dynamically
    Configure network services to start automatically at boot
    Implement packet filtering
    Configure firewall settings
    Start, stop, and check the status of network services
    Statically route IP traffic
    Dynamically route IP traffic
    Synchronize time using other network peers -->

## Configuration des services (10%)
- Configurer un serveur DNS basique
- Maintenir une zone DNS
- Configurer un serveur FTP


<!-- Service Configuration - 10%

    Configure a basic DNS server
    Maintain a DNS zone
    Configure an FTP server
    Configure anonymous-only download on FTP servers
    Provide/configure network shares via NFS
    Provide/configure network shares via CIFS
    Configure email aliases
    Configure SSH servers and clients
    Configure SSH-based remote access using public/private key pairs
    Restrict access to the HTTP proxy server
    Configure an IMAP and IMAPS service
    Query and modify the behavior of system services at various run levels
    Configure an HTTP server
    Configure HTTP server log files
    Restrict access to a web page
    Diagnose routine SELinux/AppArmor policy violations
    Configure database server -->

## Virtualisation (5%)

- Configurer un hyperviseur pour héberger des hôtes virtuels
- Accéder à une console de la machine virtuelle (VM)
- Configurer le système pour démarrer des VMs au démarrage
- Évaluer l'utilisation mémoire de VMs
- Redimentionner la RAM et l'espace de stockage de VMs

<!-- Virtualization - 5%

    Configure a hypervisor to host virtual guests
    Access a VM console
    Configure systems to launch virtual machines at boot
    Evaluate memory usage of virtual machines
    Resize RAM or storage of VMs -->

## Gestion des espaces de stockage (10%)

- Lister, créer, supprimer et modifier les partitions de stockage
- Créer, modifier, et supprimer des volumes logiques
- Étendre des volumes logiques et systèmes de fichiers existants
- Créer et configurer des partitions cryptées
- Configurer le système pour monter des systèmes de fichiers au démarrage
- Configurer et gérer le [Swap](https://fr.wikipedia.org/wiki/M%C3%A9moire_virtuelle#Swapping)
- Ajouter de nouvelles partitions et volumes logiques
- Assembler des partitions comme des dispositifs RAID
- Configurer le système pour monter à la demande des systèmes de fichiers standard, encryptés ou réseau
- Créer et gérer les Access Control Lists (ACLs) de systèmes de fichiers
- Diagnostiquer et corriger les problèmes de permissions fichiers
- <span style="color:red">Mettre en place des quotas disque sur les systèmes de fichiers pour les utilisateurs et groupes</span> : [[ici]]({{< ref "lfcs-disk-quotas.md" >}})

Avancement : <span style="color:red">1%</span> / 10% (1/12)

<!-- Storage Management - 10%

    List, create, delete, and modify storage partitions
    Create, modify and delete Logical Volumes
    Extend existing Logical Volumes and filesystems
    Create and configure encrypted partitions
    Configure systems to mount file systems at or during boot
    Configure and manage swap space
    Add new partitions, and logical volumes
    Assemble partitions as RAID devices
    Configure systems to mount standard, encrypted, and network file systems on demand
    Create and manage filesystem Access Control Lists (ACLs)
    Diagnose and correct file permission problems
    Setup user and group disk quotas for filesystems -->


# Linux Foundation Certified Engineer (LFCE)

## Administration réseaux

- Configurer les services réseau pour les démarrer automatiquement au démarrage
- Implémenter le filtrage de paquets
- Surveiller les performances réseau
- Générer et transmettre des rapports sur l'utilisation système, les interruptions et les requètes utilisateur
- Router le trafic IP statiquement et dynamiquement
- Dépanner les problèmes réseau


<!-- 
Network administration
	Configure network services to start automatically at boot
	Implement packet filtering
	Monitor network performance
	Produce and deliver reports on system use, outages and user requests
	Route IP traffic statically and dynamically
	Troubleshoot network issues
 -->

## Systèmes de fichiers

<!-- 
Network filesystems and file services
	Configure systems to mount standard, encrypted and network file systems on demand
	Create, mount and unmount standard Linux file systems
	Provide/configure network shares via NFS
	Transfer files securely via the network
	Update packages from the network, a repository or the local file system
-->

## Sécurité réseau

- Configurer les fichiers de log Apache
- Configurer le pare-feu avec iptables
- Installer et configurer SSL avec Apache
- Configurer un accès distant basé sur SSH en utilisant une paire de clés publique/privée

<!--
Network security
	Configure Apache log files
	Configure the firewall with iptables
	Install and configure SSL with Apache
	Configuring SSH-based remote access using public/private key pairs
-->

## Accès distant

 - Configurer le pare-feu avec iptables

<!-- 
Remote access
	Configure the firewall with iptables
 -->

## Services HTTP

- Configurer un client HTTP pour utiliser automatiquement un serveur proxy
- Installer et configurer un serveur web Apache
- Installer et configurer un serveur procy Squid
- Mettre en place un hébergement virtuel

<!-- 
HTTP services
	Configure an http client to automatically use a proxy server
	Install and configure an Apache web server
	Install and configure the Squid proxy server
	Restrict access to a web page with Apache
	Restrict access to the Squid proxy server
	Setting up name-based virtual web hosts
 -->


## Services Email

- Configurer les alias mail
- Installer et configurer IMAP et le service IMAPS
- Installer et configurer le service SMTP
- Restreindre les accès à un serveur SMTP

<!-- 
Email services
Configure email aliases
Install and configure an IMAP and IMAPS service
Install and configure an smtp service
Restrict access to an smtp server
 -->
