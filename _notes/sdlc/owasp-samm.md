---
layout: default
title: "OWASP SAMM"
parent: "Cycle de développement sécurisé (SDLC)"
nav_order: 1
published: true
---

# OWASP SAMM

## OWASP
L’OWASP (*Open Worldwide Application Security Project*) est une fondation à but non lucratif dédiée à l’amélioration de la sécurité des logiciels. Elle fonctionne comme une communauté mondiale ouverte, soutenue par des milliers de bénévoles, plus de 250 chapitres locaux et de nombreux projets libres. Ses actions incluent :

- des projets open source (outils, standards, documentation)
- des conférences et formations internationales
- des guides de référence comme l’**OWASP Top 10**, l’**ASVS** et **SAMM**

L’organisation oeuvre pour que chaque développeur et chaque organisation puissent concevoir et maintenir des applications de confiance. Sa mission est de “faire disparaître les logiciels non sécurisés” grâce à l’éducation, aux outils et à la collaboration.

---

## SAMM

SAMM (*Software Assurance Maturity Model*) est un modèle de maturité d’OWASP servant à évaluer, planifier et améliorer la sécurité applicative, de façon agnostique aux technologies et méthodes. Il couvre l’ensemble du **Secure Development Lifecycle**. Il définit :
- **5 fonctions métier (*business functions*)** : catégories d'activités devant être réalisées par toute entreprise oeuvrant dans le domaine du logiciel
- **3 pratiques de sécurité** par fonction métier : groupes d'activités de sécurité reliées à la fonction métier à laquelle elles sont associées.
- **3 niveaux de maturité** : pour chaque pratique, de sécurité, une entreprise peut viser le niveau de maturité 1 (moins mature), 2 (modérément mature), 3 (très mature)
- **des activités de sécurité** : pour une pratique de sécurité donnée à un niveau de maturité donné, un certain nombre d'activités de sécurité sont définies.

![OWASP SAMM](../assets/images/samm-v2-diagram.png)

{: .highlight}
> Pour chaque pratique, les activités de sécurité sont groupées dans le *stream* A ou les *stream* B. Les *streams* servent à définir des sous-objectifs distincts pour un groupe d'activités à l'intérieur d'une même pratique.

---

## 1. Gouvernance
La gouvernance se concentre sur les processus et les activités liés à la façon dont une organisation gère l’ensemble des activités de développement de logiciels. Plus précisément, cela comprend les sujets d’attention ayant un impact sur les groupes interfonctionnels impliqués dans le développement, ainsi que sur les processus opérationnels établis au niveau de l’organisation.

### 1.1 Stratégie & Métriques (G‑SM)
Le but de la pratique Stratégie et Métriques (SM) est de construire un plan efficace et concret pour réaliser vos objectifs de sécurité logicielle au sein de votre organisation.

- **Stream A — Créer et promouvoir**  
  **N1** : Identifier les motivations de l'organisation en ce qui concerne la tolérance au risque.  
  **N2** : Publier une stratégie unifiée pour la sécurité des applications.
  **N3** : Aligner le programme de sécurité des applications pour soutenir la croissance de l’entreprise.
- **Stream B — Mesurer et améliorer**  
  **N1** : Définir des métriques donnant des informations sur l'efficacité et la mise en œuvre du Programme de Sécurité Applicative.
  **N2** : Fixer des objectifs et des indicateurs clés de performance pour mesurer l’efficacité du programme.
  **N3** : Influencer la stratégie en fonction des métriques et des besoins organisationnels.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *1. Establish security standards, metrics, and governance*

### 1.2 Politiques & Conformité (G‑PC)
La pratique de la Politique et Conformité (PC) vise à comprendre et à satisfaire aux exigences juridiques et réglementaires externes tout en respectant les normes de sécurité internes dans le but de garantir que la conformité est en phase avec les objectifs commerciaux de l’organisation.

- **Stream A — Politiques et normes**  
  **N1** : Déterminer une ligne de base de sécurité représentant les politiques et les normes de l’organisation.
  **N2** : Développer des exigences de sécurité applicables à toutes les applications.
  **N3** : Mesurer et rendre compte du degré d'adhésion d'une application donnée aux politiques et normes.
- **Stream B — Gestion de la confirmité**  
  **N1** : Identifier les facteurs et les exigences de conformité tiers et les faire correspondre aux politiques et aux normes existantes.
  **N2** : Publier les exigences propres à la conformité d'une application et les conseils de test.
  **N3** : Mesurer et rendre compte de la conformité d'une application donnée avec les exigences des tierces parties.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *1. Establish security standards, metrics, and governance*

### 1.3 Éducation & Orientation (G‑EG)
La pratique de l’Education et Orientation (EG) vise à fournir au personnel impliqué dans le cycle de vie du logiciel des connaissances et des ressources pour concevoir, développer et déployer des logiciels sécurisés. 

- **Stream A — Formation et sensibilisation**  
  **N1** : Fournir une formation de sensibilisation à la sécurité à tous les employés impliqués dans le développement de logiciels
  **N2** : Offrir une technologie et des conseils spécifiques aux rôles, y compris des nuances de sécurité pour chaque langue et plate-forme
  **N3** : Des conseils internes standardisés sur les normes de développement de sécurité logicielle de l’entreprise.
- **Stream B — Organisation et culture**  
  **N1** : Identifier un "Champion de la Sécurité" au sein de chaque équipe de développement.
  **N2** : Développer un centre d'excellence en matière de sécurité logicielle qui favorise le leadership par la pensée parmi les développeurs et les architectes.
  **N3** : Construire une communauté de sécurité logicielle incluant toutes les personnes de l'organisation impliquées dans la sécurité des logiciels.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *10. Provide security training*

---

## 2. Conception
La conception concerne les processus et les activités liés à la façon dont une organisation définit les objectifs et crée les logiciels dans les projets de développement. En général, cela comprendra la collecte des exigences, la spécification de haut niveau de l’architecture et la conception détaillée.

### 2.1 Évaluation des menaces (D‑TA)
La pratique d’évaluation des menaces (TA) se concentre sur l’identification et la compréhension des risques au niveau du projet en fonction de la fonctionnalité du logiciel en cours de développement et des caractéristiques de l’environnement d’exécution.

- **Stream A — Profil de risque de l'application**  
  **N1** : Une évaluation basique du niveau de risque de l'application est effectuée afin de comprendre la probabilité et l'impact d'une attaque.
  **N2** : Comprendre le risque pour toutes les applications au sein de l'organisation en centralisant l'inventaire des profils de risque pour les intervenants.
  **N3** : Examiner périodiquement les profils de risque des applications à intervalles réguliers afin de s'assurer de l'exactitude et de la pertinence de l'état actuel.
- **Stream B — Modélisation des menaces**  
  **N1** : Effectuez une modélisation des menaces adaptée à vos moyens et avec une approche par les risques en utilisant le remue-méninges et les diagrammes existants via de simples listes de contrôle des menaces.
  **N2** : Normaliser la formation, les processus et les outils de modélisation des menaces à l'échelle de l'entreprise.
  **N3** : Optimisation et automatisation continue de votre méthodologie de modélisation des menaces.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *3. Perform security design review and threat modeling*

### 2.2 Exigences de sécurité (D‑SR)
La pratique des exigences de sécurité (SR) se concentre sur les exigences de sécurité qui sont importantes dans le contexte des logiciels sécurisés.

- **Stream A — Exigences logicielles**
Capturer des exigences de sécurité non fonctionnelles et spécifiques au risque (authentification/autorisation, journalisation, résilience, confidentialité, etc.).
  **N1** : Les objectifs de sécurité applicatifs de haut niveau sont associés aux exigences fonctionnelles.
  **N2** : Des exigences de sécurité structurées sont disponibles et utilisées par les équipes de développeurs.
  **N3** : Construire un ensemble d'exigences pour utilisation par les équipes produits.
- **Stream B — Sécurité du fournisseur**  
  **N1** : Évaluer le fournisseur selon des exigences de sécurité organisationnelles.
  **N2** : Intégrer la sécurité dans les contrats avec les fournisseurs afin de garantir la conformité avec les exigences de l'organisation.
  **N3** : Assurer une couverture de sécurité adéquate pour les fournisseurs externes en fournissant des objectifs clairs.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison *3. Perform security design review and threat modeling* (couvre la dérivation d’exigences), *2. Require use of proven security features, languages, and frameworks* et *4. Define and use cryptography standards*


### 2.3 Architecture de sécurité (D‑SA)
La pratique Architecture de Sécurité (SA) se concentre sur la sécurité liée aux composants et à la technologie que vous utilisez pendant la conception architecturale de votre logiciel.

- **Stream A — Conception d’architecture**  
  **N1** : Les équipes sont formées sur l'utilisation des principes de base de la sécurité durant la phase de conception
  **N2** : Etablir des modèles de conception et des solutions de sécurité communs.
  **N3** : Les architectures de référence sont utilisées et évaluées continuellement en vue de leur adoption et par rapport à leur pertinence.
- **Stream B — Gestion de la technologie**  
  **N1** : Enumérer les technologies, les cadres et les outils d'intégration de la solution globale pour identifier les risques.
  **N2** : Standardiser les technologies et les frameworks à utiliser pour les différentes applications
  **N3** : Imposer l’utilisation de technologies standards sur tous les développements logiciels.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison *3. Perform security design review and threat modeling*, *2. Require use of proven security features, languages, and frameworks* et *5. Secure the software supply chain* (choix et validation des composants).

---

## 3. Implémentation
L’implémentation se focalise sur les processus et les activités liés à la façon dont une organisation construit et déploie des composants logiciels et les défauts associés. Les activités au sein de la fonction Implémentation ont le plus d’impact sur la vie quotidienne des développeurs. L’objectif commun est de livrer un logiciel fonctionnant de manière fiable avec un minimum de défauts.

### 3.1 Génération sécurisée (I‑SB)
La pratique Génération Sécurisée (GS) souligne l’importance de construire des logiciels d’une façon standardisée et reproductible, et de le faire en utilisant des composants sécurisés, y compris les dépendances de logiciels tiers.

- **Stream A — Processus de génération**  
  **N1** : Créer une définition formelle du processus de génération afin qu'il devienne cohérent et répétable. 
  **N2** : Automatiser la chaîne de génération et sécuriser l'outillage utilisé. Ajouter des vérifications de sécurité dans la chaîne de génération.
  **N3** : Définir des vérifications de sécurité obligatoires dans le processus de génération et s'assurer que la construction des artefacts non conformes échoue. 
- **Stream B — Dépendances du logiciel**  
  **N1** : Créer des enregistrements avec la nomenclature de vos applications et analysez-les opportunément. 
  **N2** : Évaluer les dépendances utilisées et s'assurer d'une réaction rapide aux situations présentant un risque pour vos applications.
  **N3** : Analyser les dépendances utilisées quant aux problèmes de sécurité d'une manière comparable à votre propre code.

  {: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison *5. Secure the software supply chain*, *6. Secure the engineering environment* et de quelques éléments de *8. Ensure operational platform security* pour les baselines des environnements de build.

### 3.2 Déploiement sécurisé (I‑SD)
L’une des dernières étapes de la fourniture de logiciels sécurisés est de garantir que la sécurité et l’intégrité des applications développées ne sont pas compromises lors du déploiement. La pratique du Déploiement Sécurisé (DS) se focalise sur ce point

- **Stream A — Processus de déploiement**  
  **N1** : Formaliser le processus de déploiement et sécuriser les outils et les processus utilisés.
  **N2** : Automatiser le processus de déploiement à toutes les étapes et introduire des tests de vérification de la sécurité raisonnables.  
  **N3** : Vérifier automatiquement l'intégrité de tous les logiciels déployés, indépendamment du fait qu'ils ont été développés en interne ou en externe.
- **Stream B — Gestion des secrets**  
  **N1** : Introduire des mesures de protection de base pour limiter l'accès à vos secrets de production.
  **N2** : Injecter dynamiquement les secrets lors du processus de déploiement à partir de stockages durcis et auditer tout accès humain à ceux-ci.
  **N3** : Améliorer le cycle de vie des secrets d'application en les générant régulièrement et en en garantissant une utilisation appropriée.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison de *5. Secure the software supply chain* et *8. Ensure operational platform security*.

### 3.3 Gestion des défauts (I‑DM)
La pratique de la Gestion des Défauts (GD) se concentre sur la collecte, l’enregistrement et l’analyse des défauts de sécurité des logiciels et leur enrichissement en informations afin de pouvoir prendre des décisions basées sur des métriques.

- **Stream A — Suivi des défauts**  
  **N1** : Mettre en place un suivi structuré des défauts de sécurité et prendre des décisions éclairées sur la base de ces informations.
  **N2** : Évaluer de façon cohérente tous les défauts de sécurité sur l'ensemble de l'organisation et définir les SLA pour des classes de gravité particulière. 
  **N3** : Faire respecter les SLA prédéfinis et intégrer le système de gestion des défauts aux autres outils pertinents.
- **Stream B — Métriques et commentaires**  
  **N1** : Revoir périodiquement les défauts de sécurité précédemment enregistrés et en dériver des victoires rapides à partir des métriques de base.
  **N2** : Recueillir des métriques de gestion des défauts normalisées et les utiliser également pour établir les priorités des initiatives centralisées.
  **N3** : Améliorer continuellement les métriques de gestion des défauts de sécurité et corrélation d'autres sources.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison de *5. Secure the software supply chain*, *6. Secure the engineering environment* et de quelques éléments de *8. Ensure operational platform security* pour les baselines des environnements de build.


---

## 4. Vérification
La vérification se concentre sur les processus et les activités liés à la façon dont une organisation vérifie et teste les artefacts produits tout au long du développement de logiciels. Cela inclut généralement les activités d’assurance qualité telles que les tests, mais peut également inclure d’autres activités de revue et d’évaluation.

### 4.1 Évaluation de l'architecture (V‑AA)
La pratique de l’Évaluation de l’Architecture (AA) veille à ce que les architectures de l’application et de l’infrastructure répondent de façon adéquate à toutes les exigences de sécurité et de conformité pertinentes et atténuent suffisamment les menaces de sécurité identifiées. 

- **Stream A — Validation de l'architecture**  
  **N1** : Identifier les composants de l'architecture applicative et d'infrastructure et les passer en revue pour garantir un niveau de sécurité basique de l'approvisionnement  
  **N2** : Valider les mécanismes de sécurité de l'architecture 
  **N3** : Passage en revue de l'efficacité des composants d'architecture 
- **Stream B — Réduction de risques architecturaux**  
  **N1** : Revue ad hoc de l'architecture pour les menaces de sécurité non atténuées.
  **N2** : Analyser l'architecture par rapport aux menaces connues.
  **N3** : Mettre en place une boucle de rétroaction pour les résultats de revue d'architecture vers l'architecture d'entreprise, les principes & les modèles de conception d'organisation, les solutions de sécurité et les architectures de référence.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *3. Perform security design review and threat modeling*.

### 4.2 Tests axés sur les exigences (V‑RT)
L’objectif de la pratique de Tests axés sur les Exigences (RT) est de s’assurer que les contrôles de sécurité implémentés fonctionnent comme prévu et répondent aux exigences de sécurité élicités dans le projet. Un ensemble de cas de test de sécurité et de régression est progressivement construit et régulièrement exécuté.
- **Stream A — Vérification des contrôles**  
  **N1** : Tester les contrôles de sécurité logiciels
  **N2** : Dériver les cas de tests à partir des exigences de sécurité connues
  **N3** : Effectuer des tests de régression (avec des tests unitaires de sécurité)
- **Stream B — Tests de mauvaise utilisation ou d'abus**  
  **N1** : Effectuer des tests de fuzzing de sécurité
  **N2** : Créer et tester les cas d'abus et les défauts de la logique métier.  
  **N3** : Tests des dénis de service et de la sécurité aux limites

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison de *7. Perform security testing* et *4. Define and use cryptography standards* (si crypto)

### 4.3 Tests de sécurité (V‑ST)
La pratique des Tests de Sécurité (TS) tire parti du fait que, bien que les tests de sécurité automatisés soient rapides et adaptés à de nombreuses applications, des tests approfondis basés sur une bonne connaissance d’une application et de sa logique métier n’est souvent possible que par des tests de sécurité plus lents et manuels réalisés par un expert.

- **Stream A — Base de référence à l'échelle**  
  **N1** : Utiliser des outils de test de sécurité automatisés
  **N2** : Employer l'automatisation des tests de sécurité spécifiques à l'application
  **N3** : Intégrer les tests de sécurité automatisés dans le processus de génération et de déploiement
- **Stream B — Compréhension approfondie**  
  **N1** : Effectuer des tests de sécurité manuels sur les composants à haut risque 
  **N2** : Effectuer un test de pénétration manuel
  **N3** : Intégrer les tests de sécurité dans le processus de développement

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *7. Perform security testing*.

---

## 5. Opérations

### 5.1 Gestion des incidents (O‑IM)
Détecter, répondre et apprendre des incidents (détection, triage, communication, post‑mortem, amélioration continue).
- **Stream A — Détection & triage**  
  **N1** : Journalisation et alertes minimales.  
  **N2** : Playbooks de triage, classification des incidents.  
  **N3** : Détection enrichie (corrélation), métriques MTTA/MTTR.
- **Stream B — Réponse & *lessons learned***  
  **N1** : Processus de réponse basique (qui fait quoi, quand).  
  **N2** : Communications, escalades, rétention d’artefacts.  
  **N3** : Post‑mortems systématiques, boucles d’amélioration.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *9. Implement security monitoring and response*.

### 5.2 Gestion de l'environnement (O‑EM)
Gérer et surveiller les environnements (journalisation, télémétrie, *monitoring*, séparation des environnements, secrets, accès machine/service).
- **Stream A — Environnements & secrets**  
  **N1** : Séparation dev/test/prod, gestion de secrets.  
  **N2** : Baselines durcies, accès machine/service minimaux.  
  **N3** : Politique de rotation/attestation, revues périodiques.
- **Stream B — Observabilité & monitoring**  
  **N1** : Journaux essentiels centralisés.  
  **N2** : Détection d’anomalies, tableaux de bord opérationnels.  
  **N3** : Télémétrie avancée, détection comportementale et réponse auto.  

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison de 
*6. Secure the engineering environment*, *8. Ensure operational platform security* et *9. Implement security monitoring and response*.

### 5.3 Gestion opérationnelle (O‑OM)
Exploiter les applications de façon sécurisée (durcissement, configuration, sauvegardes, gestion des accès, etc).
- **Stream A — Exploitation sécurisée**  
  **N1** : Procédures et guides d'exploitation (*runbooks*), sauvegardes, accès minimaux.  
  **N2** : Revue périodique des privilèges et des configurations.  
  **N3** : Gestion de configuration centralisée et contrôles préventifs.  
- **Stream B — Continuité & résilience**  
  **N1** : Plan basique de continuité (restauration, DR).  
  **N2** : Exercices réguliers, *chaos days* limités.  
  **N3** : Résilience mesurée (RTO/RPO), tests à l’échelle.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *8. Ensure operational platform security*.



---

## Liens utiles

- **OWASP — About** : mission, activités, communauté
  [https://owasp.org/about/](https://owasp.org/about/)
- **OWASP SAMM - Le modèle** : détails du modèle SAMM
  [https://owaspsamm.org/model/](https://owaspsamm.org/model/)
- **OWASP SAMM V2** ([PDF](../assets/files/SAMM-v2-PDF.pdf))
- **OWASP SAMM - Quick Start Guide (v2)** : structure 
  [https://owaspsamm.org/guidance/quick-start-guide/](https://owaspsamm.org/guidance/quick-start-guide/)
- **Microsoft - SDL Practices (10 pratiques)** : liste des pratiques et positionnement SDL
  [https://www.microsoft.com/en-us/securityengineering/sdl/practices](https://www.microsoft.com/en-us/securityengineering/sdl/practices)
- **OWASP SAMM Blog - Comparing Microsoft SDL and SAMM (2025)** : correspondance entre requis SDL et activités SAMM 
  [https://owaspsamm.org/blog/2025/01/20/comparing-microsoft-sdl-and-samm/](https://owaspsamm.org/blog/2025/01/20/comparing-microsoft-sdl-and-samm/)

