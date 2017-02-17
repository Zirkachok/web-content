+++
title      = "Mettre en place une sandbox de test Linux"
date       = "2017-02-14T16:14:27+01:00"
tags       = ["Linux", "LFCE-LFCS"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = true
+++

Comme je l'avais annoncé il y a quelques jours, je publierai régulièrement dans ces colonnes des articles concernant Linux, son fonctionnement et son administration, accompagnés de tutoriels, d'exercices et de tests. Nous allons donc être amenés à réaliser des manipulations plus ou moins critiques et à altérer le fonctionnement du système. Pour ne citer que quelques exemples, nous verrons comment modifier des paramètres système et réseau, changer des droits, créer et supprimer des utilisateurs, installer un nouveau noyau, etc. Il n'est donc pas envisageable de faire ces manipulations directement depuis son environnement de tous les jours. Une autre raison pour laquelle il est préférable de réaliser ses tests sur un autre système est d'éviter que le système lui-m^eme (ses configurations courantes, les droits affectés, les installations réalisées) n'altère le résultat de nous tests. Donc, pour faire les choses proprement, je vais avant toutes choses vous proposer quelques solutions pour mettre en place un espace de test dédié et isolé : une Sandbox.

# Virtualisation

# IaaS mon amour

<!-- uname -a -->
<!-- Linux Epinet 3.13.0-93-generic #140-Ubuntu SMP Mon Jul 18 21:21:05 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux -->
<!--   A      B         C                                D                          E      F      G      H      -->
<!-- A = kernel name ; B = network node hostname ; C = kernel release ; D = kernel version ; E = machine hardware name -->
<!-- F = processor type (non-portable) ; G = hardware platform (non-portable) ; H = operating system -->
<!-- shutdown -->
<!-- logout -->