# Explication des Paramètres JVM pour les Tests

Ce document décrit les différents paramètres JVM utilisés dans les tests. Chaque configuration simule un environnement unique pour tester divers aspects de la gestion de la mémoire et des performances de la JVM.

## TODO: ON teste des OS differents ???

## 1. Avec des mémoires variées

**Description** : Cette configuration attribue 512 Mo de mémoire minimale et 1024 Mo de mémoire maximale à la JVM. Ce paramètre est utile pour tester le comportement de l'application dans un environnement à mémoire limitée.

**Objectif du Test** : Tester la performance de l'application lorsque la mémoire allouée est restreinte. Cela permet d'identifier si des exceptions `OutOfMemoryError` ou des ralentissements significatifs surviennent lorsque la mémoire est insuffisante.

---

## 2. Les différents garbage collectors

**Description** : Le garbage collector G1GC (Garbage-First Garbage Collector) est conçu pour les applications nécessitant des latences faibles tout en ayant une utilisation modérée de la mémoire. Avec cette configuration, on alloue entre 1024 Mo et 2048 Mo de mémoire.

**Objectif du Test** : Évaluer les performances et la réactivité de l'application avec un garbage collector optimisé pour réduire les pauses longues. Ce test est particulièrement utile pour des applications en production qui nécessitent une réponse rapide sans interruption prolongée.

**Description du 2ème Garbage collector** : ZGC (Z Garbage Collector) est un garbage collector basse latence, spécialement conçu pour minimiser les temps de pause, même sur des applications à mémoire élevée. Cette configuration alloue un minimum de 512 Mo et un maximum de 2048 Mo.

**Objectif du Test** : Mesurer les avantages du ZGC en termes de latence pour les applications nécessitant des traitements rapides, notamment lorsque la charge mémoire est élevée. ZGC est particulièrement utile dans les systèmes à haute disponibilité où chaque milliseconde compte.

**Description du 3ème Garbage collector** : Ce garbage collector exécute de façon multithreaded les collections de déchets, et opère mieux sur des sytèmes avec beaucoup de mémoire dispoo.
**Objectif du Test** : TODO

**Description du 4ème Garbage collector** : Ce dernier garbage collector, pour lequel j'ai eu de la difficulté à trouver de la documentation, multithread la copie de la collection, et arrête les différentes threads pendant la collections. Sa différence avec le collecteur précédant, est que celui la à un algorithme optimisé pour plus des heaps de plus de 10Gb. Le lien juste en dessous explique plus en détail leur différence. En réalité je n'ai pas executé cette option, car elle n'existait pas pour JDK 17, ce autour de quoi ce repo est construit. Je laisse les détails de mes recherches dans le readme car c'est intéressant.

https://stackoverflow.com/questions/2101518/difference-between-xxuseparallelgc-and-xxuseparnewgc

**Objectif du Test** : Pour des ordinateurs multicores (maintenant standard) on veut que nos programmes executent de façon multithreaded, et cela même si ils n'ont pas beaucoups de RAM. Nous voulons observer dans ce cas la performance du github action en restraignant la mémoire disponible, pour assurer l'éfficacité du programme sous ces conditions.

---

## 3. Compression des object pointers

**Description** : L'option `UseCompressedOops` compresse les références d'objets, ce qui réduit l'empreinte mémoire pour les applications fonctionnant dans un espace mémoire de moins de 32 Go. Cette configuration alloue entre 1024 Mo et 4096 Mo.

**Objectif du Test** : Optimiser la gestion de la mémoire pour des applications utilisant de grandes quantités de données. Ce test évalue l'efficacité de la JVM à compresser les références et peut être utile pour les applications nécessitant une manipulation intensive de données tout en réduisant la consommation de mémoire. Toutefois, ayant beaucoup de mémoire disponible, nous voulons nous assurer que cette option ne causera pas de baisse de performance. Un parser est un programme qui contient des énormes quantité de pointers, et donc si leur quantité est très grande, peut être que ce flag pourrait causer problème.

---

## 4. Use thread priorities, utile pour les race conditions

**Objectif du test** Dans un parser, énormement des operations sont multithreaded. Changer la priorité des opérations est utile pour vérifier si nous pouvons trigger des race conditions. Nous pouvons également évaluer la performance de notre multithreading avec une différente priorité.
