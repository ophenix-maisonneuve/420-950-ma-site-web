---
layout: default
title: OWASP Top 10 (2025)
nav_order: 8
has_children: true
has_toc: true
published: true

---
# OWASP Top 10 (2025)
Le OWASP Top 10 est un document de sensibilisation standard pour les développeurs et les équipes de sécurité applicative : il présente un consensus large sur les dix risques les plus critiques pour les applications web, afin d’offrir un langage commun et des priorités pour l’industrie.  L’édition 2025 constitue la version la plus récente du projet et reflète l’état actuel des menaces ainsi que la manière dont elles sont observées dans les applications modernes.  Il ne s'agit pas d'une liste exhaustive ni d'une norme de conformité ; c’est plutôt un point de départ pour l’apprentissage, la communication et la priorisation des efforts de sécurité au sein des entreprises et organisations.

---

## Historique et positionnement
Le projet remonte à 2003, date de la première liste, et a connu des mises à jour régulières (notamment 2004, 2007, 2010, 2013, 2017, 2021), jusqu’à l’édition 2025, la huitième itération du Top 10.  Le processus repose sur une collecte de données à grande échelle (résultats de tests SAST/DAST, audits, *bug bounty*) complétée par une enquête dans la communauté pour inclure des risques émergents parfois sous‑testés. Ce modèle permet de s'assurer que les catégories reflètent la réalisé des organisations tout en anticipant les tendances.  L’édition 2025 illustre cette évolution : elle met davantage l’accent sur les causes racines (ex.: consolidation de ***SSRF*** dans ***Broken Access Control***) et introduit deux nouvelles catégories, ***Software Supply Chain Failures*** et ***Mishandling of Exceptional Conditions***, pour mieux refléter les architectures et chaînes de livraison qui sont maintenant présentes partout dans l'industrie.


Dans la pratique, le Top 10 sert de base pour bâtir une culture de sécurité : il oriente la formation des équipes, structure la communication entre développeurs, sécurité et parties prenantes, et fournit une base commune pour la priorisation des corrections et des contrôles.  Il est largement utilisé comme référence dans les formations et comme point de départ dans les programmes internes, dans les échanges avec les fournisseurs et même dans certaines démarches de conformité, tout en restant volontairement concis pour être facilement adoptable par des équipes de tailles et maturités variées.  Enfin, l’édition 2025 aide les organisations à réallouer l’effort vers les risques les plus fréquents aujourd'hui (ex.: mauvaise configuration (en forte progression), chaîne d’approvisionnement logiciel, et résilience en conditions exceptionnelles) afin d’inclure la sécurité dans la conception, les pipelines CI/CD et l’exploitation au quotidien.

---

## La liste 2025

- **A01 : Broken Access Control** : Problèmes liés à un mauvais contrôles des accès et autorisations menant généralement à une élévation de privilèges
- **A02 : Security Misconfiguration** : Problèmes causés par des configurations plutôt que par du code
- **A03 : Software Supply Chain Failures** : Problèmes introduits par des logiciels tierces inclus dans une application
- **A04 : Cryptographic Failures** : Mauvaise utilisation des algorithmes ou du matériel cryptigraphique
- **A05 : Injection** : Vulnérabilités permettant à un attaquant d'ajouter du code malicieux à une application légitime
- **A06 : Insecure Design** : Problèmes émanant de la conception du logiciel, et non de son implémentation
- **A07 : Authentication Failures** : Défaillances avec les mécanismes d'authentification, qui mènent souvent à des problèmes avec A01
- **A08 : Software or Data Integrity Failures** : Problèmes liés à une mauvaise vérification de l'intégrité des données de l'application ou de l'application elle-même (code source ou binaires)
- **A09 : Logging & Alerting Failures** : Logs falsifiés ou trompeurs, ou dévoilant trop d'information, ou simplement logs ou alertes insuffisants
- **A10 : Mishandling of Exceptional Conditions** : Mauvaise gestion des exceptions dans le code menant à une instabilité ou à des comportement indésirables (*DoS* ou *élévation de privilèges*, notamment)

**Faits saillants**
- Montée de ***Security Misconfiguration*** (#2)
- Introduction de  ***Mishandling of Exceptional Conditions*** et ***Software Supply Chain Failures***
- ***Server-side Request Forgery (SSRF)*** absorbé par ***Broken Access Controls***.

---

### Références officielles
- Projet : [https://owasp.org/Top10/2025/](https://owasp.org/Top10/2025/)
- Introduction : [https://owasp.org/Top10/2025/0x00_2025-Introduction/](https://owasp.org/Top10/2025/0x00_2025-Introduction/)
- Page d’accueil : [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/)

