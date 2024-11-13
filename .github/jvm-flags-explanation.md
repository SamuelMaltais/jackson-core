# Explication des Paramètres JVM pour les Tests

Ce document décrit les différents paramètres JVM utilisés dans les tests. Chaque configuration simule un environnement unique pour tester divers aspects de la gestion de la mémoire et des performances de la JVM.

TODO: ON teste des OS differents ???
---

## 1. Avec des mémoires variées

**Description** : Cette configuration attribue 512 Mo de mémoire minimale et 1024 Mo de mémoire maximale à la JVM. Ce paramètre est utile pour tester le comportement de l'application dans un environnement à mémoire limitée.

**Objectif du Test** : Tester la performance de l'application lorsque la mémoire allouée est restreinte. Cela permet d'identifier si des exceptions `OutOfMemoryError` ou des ralentissements significatifs surviennent lorsque la mémoire est insuffisante.

---

## 2. Les différents garbage collectors

**Description** : Le garbage collector G1GC (Garbage-First Garbage Collector) est conçu pour les applications nécessitant des latences faibles tout en ayant une utilisation modérée de la mémoire. Avec cette configuration, on alloue entre 1024 Mo et 2048 Mo de mémoire.

**Objectif du Test** : Évaluer les performances et la réactivité de l'application avec un garbage collector optimisé pour réduire les pauses longues. Ce test est particulièrement utile pour des applications en production qui nécessitent une réponse rapide sans interruption prolongée.

**Description du 2ème Garbage collector** : ZGC (Z Garbage Collector) est un garbage collector basse latence, spécialement conçu pour minimiser les temps de pause, même sur des applications à mémoire élevée. Cette configuration alloue un minimum de 512 Mo et un maximum de 2048 Mo.

**Objectif du Test** : Mesurer les avantages du ZGC en termes de latence pour les applications nécessitant des traitements rapides, notamment lorsque la charge mémoire est élevée. ZGC est particulièrement utile dans les systèmes à haute disponibilité où chaque milliseconde compte.

**Description du 3ème Garbage collector** : Ce troixème garbage collector, pour lequel j'ai eu de la difficulté à trouver de la documentation, multithread la copie de la collection, et arrête les différentes threads pendant la collections. Sa différence avec le collecteur précédant, est que celui la à un algorithme optimisé pour plus des heaps de plus de 10Gb. Le lien juste en dessous explique plus en détail leur différence.

**Objectif du Test** : Mesurer les avantages du ZGC en termes de latence pour les applications nécessitant des traitements rapides, notamment lorsque la charge mémoire est élevée. ZGC est particulièrement utile dans les systèmes à haute disponibilité où chaque milliseconde compte.

**Description du 4ème Garbage collector** : Ce troixème garbage collector, pour lequel j'ai eu de la difficulté à trouver de la documentation, multithread la copie de la collection, et arrête les différentes threads pendant la collections. Sa différence avec le collecteur précédant, est que celui la à un algorithme optimisé pour plus des heaps de plus de 10Gb. Le lien juste en dessous explique plus en détail leur différence.

https://stackoverflow.com/questions/2101518/difference-between-xxuseparallelgc-and-xxuseparnewgc

**Objectif du Test** : Mesurer les avantages du ZGC en termes de latence pour les applications nécessitant des traitements rapides, notamment lorsque la charge mémoire est élevée. ZGC est particulièrement utile dans les systèmes à haute disponibilité où chaque milliseconde compte.


---

## 3. 


---