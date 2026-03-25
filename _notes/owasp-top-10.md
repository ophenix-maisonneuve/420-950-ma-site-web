---
layout: default
title: OWASP Top 10 (2025)
nav_order: 1
has_children: true
has_toc: true
published: false

---
# OWASP Top 10 (2025)
Le OWASP Top 10 est un document de sensibilisation standard pour les développeurs et les équipes de sécurité applicative : il présente un consensus large sur les dix risques les plus critiques pour les applications web, afin d’offrir un langage commun et des priorités à l’échelle de l’industrie.  L’édition 2025 constitue la version la plus récente du projet et reflète l’état actuel des menaces ainsi que la manière dont elles sont observées dans les applications modernes.  Il ne s'agit pas d'un référentiel exhaustif ni d'une norme de conformité ; c’est une base d’orientation pour l’apprentissage, la communication et la priorisation des efforts de sécurité au sein des organisations.

---

## Historique et positionnement
Le projet remonte à 2003, date de la première liste, et a connu des mises à jour régulières (notamment 2004, 2007, 2010, 2013, 2017, 2021), jusqu’à l’édition 2025 qui marque la huitième itération du Top 10.  Le processus d’entretien repose sur une collecte de données à grande échelle (résultats de tests SAST/DAST/IAST, audits, *bug bounty*) complétée par une enquête communautaire pour inclure des risques émergents parfois sous‑testés ; ce modèle permet d’ancrer les catégories dans la réalité opérationnelle tout en anticipant les tendances.  L’édition 2025 illustre cette évolution : elle met davantage l’accent sur les causes racines (ex.: consolidation de ***SSRF*** dans ***Broken Access Control***) et introduit deux nouvelles catégories, ***Software Supply Chain Failures*** et ***Mishandling of Exceptional Conditions***, pour mieux refléter les architectures et chaînes de livraison contemporaines


Dans la pratique, le Top 10 sert de point de départ pragmatique pour bâtir une culture de sécurité : il oriente la formation des équipes, structure la communication entre développeurs, sécurité et parties prenantes, et fournit une base commune pour la priorisation des corrections et des contrôles.  Il est largement utilisé comme référence pédagogique et “baseline” dans les programmes internes, dans les échanges avec les fournisseurs et même dans certaines démarches de conformité, tout en restant volontairement concis pour être facilement adoptable par des équipes de tailles et maturités variées.  Enfin, l’édition 2025 aide les organisations à réallouer l’effort vers des risques systémiques aujourd’hui dominants — mauvaise configuration (en forte progression), chaîne d’approvisionnement logiciel, et résilience en conditions exceptionnelles — afin d’inscrire la sécurité dans la conception, les pipelines CI/CD et l’exploitation au quotidien.

---

## La liste 2025

1. **A01 — Broken Access Control**  
2. **A02 — Security Misconfiguration**  
3. **A03 — Software Supply Chain Failures**  
4. **A04 — Cryptographic Failures**  
5. **A05 — Injection**  
6. **A06 — Insecure Design**  
7. **A07 — Authentication Failures**  
8. **A08 — Software or Data Integrity Failures**  
9. **A09 — Logging & Alerting Failures**  
10. **A10 — Mishandling of Exceptional Conditions**

**Faits saillants :** montée de la **misconfiguration** (#2), introduction de la **chaîne logicielle** et de la **résilience** en conditions exceptionnelles, **SSRF** absorbé par A01.

---

### Références officielles
- Projet : https://owasp.org/Top10/2025/
- Introduction : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- Page d’accueil : https://owasp.org/www-project-top-ten/

