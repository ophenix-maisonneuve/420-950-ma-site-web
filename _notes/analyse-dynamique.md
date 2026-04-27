---
layout: default
title: Analyse dynamique
nav_order: 10
has_children: true
has_toc: true
published: true
---

# Analyse dynamique d'une application

Globalement, les différentes techniques d'analyse dynamique analysent l’application en cours d'exécution, comme le ferait un attaquant, sans voir son code. Cette différence fondamentale avec l'analyse statique implique des forces et des faiblesses distinctes.

L'analyse dynamique se distingue par sa capacité à :

- détecter uniquement ce qui est réellement exploitable,
- prendre en compte la configuration réelle de l’environnement,
- tester les intégrations externes et les dépendances.

Par contre, elle ne peut pas analyser le code mort ou les branches logiques jamais exécutées ou extrêmement difficiles d'accès.