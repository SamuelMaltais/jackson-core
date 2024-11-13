# Explication des Paramètres JVM pour les Tests

Ce document décrit les différents paramètres JVM utilisés dans les tests. Chaque configuration simule un environnement unique pour tester divers aspects de la gestion de la mémoire et des performances de la JVM.

TODO: ON teste des OS differents ???
---

## 1. **-Xms512m -Xmx1024m**

**Description** : Cette configuration attribue 512 Mo de mémoire minimale et 1024 Mo de mémoire maximale à la JVM. Ce paramètre est utile pour tester le comportement de l'application dans un environnement à mémoire limitée.

**Objectif du Test** : Tester la performance de l'application lorsque la mémoire allouée est restreinte. Cela permet d'identifier si des exceptions `OutOfMemoryError` ou des ralentissements significatifs surviennent lorsque la mémoire est insuffisante.
