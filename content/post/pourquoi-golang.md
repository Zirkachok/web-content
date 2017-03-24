+++
title      = "Quelques impressions sur le Go"
date       = "2017-01-18T17:20:17+01:00"
author     = "jul"
tags       = ["dev.", "golang"]
categories = ["Informatique"]
menu       = ""
banner     = "banners/placeholder.png"
draft      = false
+++

Il y a de cela des années, j'avais opté pour python comme language de prédilection pour mes développements débarqués. J'étais à l'époque tombé sous le charme de sa syntaxe minimaliste, de son efficacité (dans le traitement des chaines de caractères notamment) et de l'étendue de ses librairies. Autant d'avantages qui me permettaient de réaliser rapidement et efficacement des outils là où mes langages de script ne suffisaient plus. Cependant, au fil des années, de nouvelles problématiques m'ont amené à me détourner petit à petit de ce langage[^1]. J'ai donc commencé à chercher si ce n'est un successeur au moins un complément.


[Go](https://golang.org/) fait partie de ces langages "à la mode" ces dernières années (avec [Scala](https://www.scala-lang.org/), [Rust](https://www.rust-lang.org), [D](http://dlang.org/), etc.), mais il se démarque par sa croissance[^2] et ses origines[^3]. Je me suis donc laissé tenter, et vous fait part de mes impressions après quelques semaines d'utilisation.

[^1]: Pour n'en citer qu'une, rendre disponible et maintenir un outil pour des cibles n'ayant pas d'interpréteur python et/ou l'ensemble des librairies installées, est un parcours du combattant (pas de cross-compilation possible, nécessité d'utiliser des packets spécifiques, exécutables ainsi générés souvent lents et gourmands, etc.).
[^2]: Le langage Go se situait en 2016 à la 13ème place au [classement Tiobe](http://www.tiobe.com/tiobe-index/) des langages les plus utilisés, contre la 54ème en 2015.
[^3]: Le langage Go est issu des laboratoires de Google et de l'esprit de [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike)


# Une philosophie nouvelle

Comme le jeu du même nom, Le langage Go a été conçu pour être simple à apprendre, mais profond dans ses possibilités. Celà se traduit par de nombreux aspects, assez novateurs dans le monde du développement.

## Un langage rigide...

Au premier regard, avec un typage fort et une syntaxe stricte, le Go semble assez rigide. Pas question ici de faire les choses à sa sauce. Les crochets suivent la déclaration des méthodes/fonctions/conditions/boucles sur la même ligne, il est interdit d'importer une bibliothèque sans l'appeler dans le code, toute variable a un type fixe, etc. Il existe même [un outil](https://blog.golang.org/go-fmt-your-code) fourni par défaut pour rendre conforme la syntaxe d'un code source en suivant les pratiques préconisées ou imposées.

Un exemple de code en Go :

    package main

    import (
        "fmt"
        "flag"
    )

    func main() {
        var svar string
        wordPtr := flag.String("file", "foo", "a string")

        flag.StringVar(&svar, "svar", "bar", "a string var")
        flag.Parse()

        if fileExists(*filePtr) {
            fmt.Println("Valid file provided")
        } else {
            fmt.Println("File " + *filePtr + " not found")
        }
    }

Cela peut sembler désagréable, surtout lorsque l'on est habitué à des langages plus permissifs (la plupart des langages en fait...). Cependant, cette rigidité a pour avantage de forcer une uniformisation du code, et donc de faciliter grandement son inter-compréhension et son maintien. À ce niveau, le Go est particulièrement adapté aux projets de grande taille impliquant des développeurs d'expériences différentes (comme dans la communauté Open-Source, par exemple).

Pour contrebalancer, La syntaxe du Go emprunte beaucoup au C/C++[^4], en y mêlant des éléments issus de langages plus récents. Elle se veut donc rassurante tout en étant moderne et puissante. Reste qu'une telle rigueur est un parti-pris qui ne plaira pas à tout le monde.

[^4]: La syntaxe elle-même, proche du style [K&R](https://fr.wikipedia.org/wiki/Style_d'indentation#Style_K.26R), l'usage des [pointeurs](https://blog.golang.org/gos-declaration-syntax), etc.


## ...Et flexible à la fois

Passée cette première impression, le Go fait preuve de nombreuses avantages. Comme citer ses qualités et inconvénients demanderait un article à lui seul (qui viendra peut-être, qui sait), je ne m'attarderai que sur ceux qui m'ont semblé les plus marquants :

 - **Optimisé et optimisable** : Le langage Go, suivant le cas d'utilisation, n'est pas forcément le plus rapide ni le moins gourmand, comme le montrent les [études](https://days2011.scala-lang.org/sites/days2011/files/ws3-1-Hundt.pdf) sur le sujet. Par contre, outre la la présence d'un ramasse-miettes efficace, son utilisation du paradigme de programmation [concurrente](https://blog.golang.org/concurrency-is-not-parallelism) fait qu'il peut tirer le meilleur parti d'une architecture muti-coeurs sans perdre en qualité et propreté de code.

 - **Intuitif** : Comme je l'ai mentionné plus avant, pour qui est versé dans le C, C++ ou Java, le Go s'apprend de manière intuitive. Sa syntaxe proche de ces langages, la [gestion des erreurs](https://blog.golang.org/error-handling-and-go) simple et élégente, l'import des packages (notamment possible [via leur URL](https://golang.org/doc/code.html#ImportPaths)) largement simplifié, rendent sa prise en main rapide et facile. Comparé à aux langages fonctionnels tels que [Scala](https://www.scala-lang.org/) ou à des syntaxes plus originales comme [Rust](https://www.rust-lang.org), Go permet une transition rapide et efficace. Un atout de poids lorsque l'on travaille en équipe.

 - **Des outils pour tout** : Go propose une large panoplie d'outils intégrés qui apportent un réel plus au langage en tant que tel. On y trouve entre autres [Go doc](https://blog.golang.org/godoc-documenting-go-code) pour la documentation automatique de code, [Gofix](https://blog.golang.org/introducing-gofix) pour la mise à niveau de code source au fil des versions, [Go list](https://golang.org/cmd/go/#hdr-List_packages) pour la génération des graphes de dépendances du package, [Go pprof](https://blog.golang.org/profiling-go-programs) pour le profilage, [GDB](https://blog.golang.org/debugging-go-code-status-report) pour le déboguage, et bien d'autres encore... Enfin, il jouis d'une communauté très active proposant des packages variés et puissants[^5], le tout en plus de ceux proposés officiellement.
 
 - **Orienté Back-End** : La gestion native et optimisée protocoles et standards de l'Internet ([HTTP](https://golang.org/pkg/net/http/), [SSH](https://godoc.org/golang.org/x/crypto/ssh), [JSON](https://golang.org/pkg/encoding/json/), [XML](https://golang.org/pkg/encoding/xml/), [SQL](https://golang.org/pkg/database/sql/), [cryptographie](https://golang.org/pkg/crypto/), etc.) et sa gestion de la concurrence font du Go un langage de choix pour le développement Cloud et Back-end. Ce n'est pas pour rien que des fournisseur de services comme SendGrid, Docker ou encore Dropbox s'y sont lancés.

 - **Multi-plateformes** : Go intègre aussi nativement la [compilation croisée](https://dave.cheney.net/2015/08/22/cross-compilation-with-go-1-5), permettant ainsi de compiler un executable pour une autre architecture que la sienne. Il offre la possibilité de réaliser des [appels à du code C](https://blog.golang.org/c-go-cgo), et ainsi d'intégrer des bibliothèques héritées (ou Legacy code) plus facilement.

<!-- Built with concurrency in mind -->

[^5]: Pour une liste non exhaustive, on peut se référer [ici](https://github.com/golang/go/wiki/Projects), [là](https://github.com/avelino/awesome-go) et [là-bas](http://www.mjhall.org/golang-data-science-libraries/).


## Puissant, mais perfectible

Malgré tous ses apports, il reste au Go bien des défauts. Certains tiennent plus de choix clivants et d'une volonté de rester proche du C/C++, comme son typage fort et le manque d'[inférence de types](https://fr.wikipedia.org/wiki/Inf%C3%A9rence_de_types), l'absence de paradigme orienté objet, ou encore l'usage de pointeurs que nous avons mentionnés plus haut, mais d'autres rentrent moins dans cette logique.

 - **Pas de généricité** : Le défaut le plus évident est le manque de support pour la programmation générique et tout ce qu'elle apporte (dont le polymorphisme). Même si les [interfaces](http://golangtutorials.blogspot.fr/2011/06/polymorphism-in-go.html) peuvent remplir ce rôle et donc contourner le problème, on y perd grandement en transparence et en propreté du code.

 - **Mutable ou immutable?** : Pour des "objets" complexes (structures, etc.), il n'est pas toujours évident de savoir si il sera copié ou modifié. Un objet peut etre passé par adresse et ainsi lever le doute, mais une gestion explicite de l'[immutabilité](https://fr.wikipedia.org/wiki/Objet_immuable) aurait été un plus bienvenu.

 - **Pas le plus performant** : Comme mentionné plus haut, le langage Go n'est pas toujours le plus performant. Son empreinte mémoire est supérieure à celle de langages tels que C++ ou Scala, et il en va de même pour le temps d'exécution ou la taille du binaire généré. Malgré tout, Go reste un langage performant, tout est une question de priorités.


# Au final, ça en vaut la peine?

Le Go peut en rebuter certains par ses choix clivants, mais ses inspirations font de lui un langage simple à apprendre mais profond, et qui apporte de nombreuses récompenses à qui fait l'effort de s'y investir. Il n'a certes pas les performances du C/C++ ou des langages fonctionnels modernes, mais il a pour lui la flexibilité du Python tout en étant plus maintenable et plus strict.

En tout cas, il semble promis à un bel avenir, en témoigne [sa progression](http://www.tiobe.com/tiobe-index/go/), sa communauté grandissante et son adoption par des grandes companies et services de l'Internet, tels que [Docker](http://fr.slideshare.net/jpetazzo/docker-and-go-why-did-we-decide-to-write-docker-in-go), [Dropbox](https://blogs.dropbox.com/tech/2014/07/open-sourcing-our-go-libraries/), [Soundcloud](https://developers.soundcloud.com/blog/go-at-soundcloud) ou encore [SendGrid](https://sendgrid.com/blog/convince-company-go-golang/), [Netflix](https://github.com/Netflix/rend) et bien d'autres.