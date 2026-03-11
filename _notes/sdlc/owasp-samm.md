---
layout: default
title: "OWASP SAMM"
parent: "Cycle de développement sécurisé (SDLC)"
nav_order: 1
published: false
---

# OWASP SAMM

## Qui est OWASP ?
L’OWASP (*Open Worldwide Application Security Project*) est une fondation à but non lucratif dédiée à l’amélioration de la sécurité des logiciels. Elle fonctionne comme une communauté mondiale ouverte, soutenue par des milliers de bénévoles, plus de 250 chapitres locaux et de nombreux projets libres. Ses actions incluent :

- des projets open source (outils, standards, documentation)
- des conférences et formations internationales
- des guides de référence comme l’OWASP Top 10, l’ASVS et SAMM

L’organisation oeuvre pour que chaque développeur et chaque organisation puissent concevoir et maintenir des applications de confiance. Sa mission est de “faire disparaître les logiciels non sécurisés” grâce à l’éducation, aux outils et à la collaboration.

---

## OWASP SAMM

- SAMM (*Software Assurance Maturity Model*) est un modèle de maturité d’OWASP servant à évaluer, planifier et améliorer la sécurité applicative, de façon agnostique aux technologies et méthodes. Il couvre l’ensemble du **Secure Development Lifecycle**. Il définit :
- **5 fonctions métier (*business functions*)** : catégories d'activités devant être réalisées par toute entreprise oeuvrant dans le domaine du logiciel
- **3 pratiques de sécurité** par fonction métier : groupes d'activités de sécurité reliées à la fonction métier à laquelle elles sont associées.
- **3 niveaux de maturité** : pour chaque pratique, de sécurité, une entreprise peut viser le niveau de maturité 1 (moins mature), 2 (modérément mature), 3 (très mature)
- **des activités de sécurité** : pour une pratique de sécurité donnée à un niveau de maturité donné, un certain nombre d'activités de sécurité sont définies.

{: .highlight}
> Pour chaque pratique, les activités de sécurité sont groupées dans le *stream* A ou les *stream* B. Les *streams* servent à définir des sous-objectifs distincts pour un groupe d'activités à l'intérieur d'une même pratique.

![OWASP SAMM](assets/images/SAMM_v2_diagram.svg)

---

## 1) Gouvernance

### 1.1 Stratégie & Métriques (G‑SM)
Établir la vision, les objectifs mesurables et les indicateurs pour le programme de sécurité logicielle, piloter l’amélioration continue.

- **Stream A — Stratégie du programme**  
  **N1** : Définir la vision, le périmètre applicatif et les objectifs sécurité du programme.  
  **N2** : Aligner feuille de route sécurité et risques métier ; assigner rôles et budgets.  
  **N3** : Piloter le programme (revues trimestrielles), arbitrer en fonction de la valeur/risque.
- **Stream B — Mesure & amélioration**  
  **N1** : Sélectionner quelques indicateurs de base (ex. vulnérabilités ouvertes, couverture tests).  
  **N2** : Déployer un tableau de bord, seuils/SLAs et cadence de rapport.  
  **N3** : Mesures intégrées dans la gouvernance (OKR/KPI), amélioration continue outillée.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *1. Establish security standards, metrics, and governance*

### 1.2 Politiques & Conformité (G‑PC)
Définir, publier et faire respecter les politiques de développement sécurisé et s’aligner sur les exigences externes (standards, réglementations, clients).
- **Stream A — Politiques internes**  
  **N1** : Rédiger politiques de dev sécurisé (revues de code, secrets, branches, etc.).  
  **N2** : Diffuser, former, auditer l’application des politiques.  
  **N3** : Réviser périodiquement, intégrer retours incidents/audits. 
- **Stream B — Conformité externe**  
  **N1** : Identifier obligations (réglementaires, contractuelles).  
  **N2** : Mettre en place contrôles/évidences ; préparer audits.  
  **N3** : Harmoniser contrôles avec le SDLC et automatiser la preuve. 

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *1. Establish security standards, metrics, and governance*

### 1.3 Éducation & Guidance (G‑EG)
Former les équipes (dev, ops, produit) et fournir des guides, *cheat sheets* et modèles décisionnels pour intégrer la sécurité au quotidien.
- **Stream A — Formation**  
  **N1** : Formation de base pour dev/ops/produit.  
  **N2** : Parcours par rôle (ex. secure coding pour dev back/front).  
  **N3** : Programme mesuré (quiz, labs), couverture > x % et recyclage annuel.  
- **Stream B — Guides & accompagnement**  
  **N1** : Guides pratiques (cheat sheets, checklists).  
  **N2** : Modèles de décisions (crypto, secrets, journalisation).  
  **N3** : Coaching/office hours, référents sécurité dans les squads.  

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *10. Provide security training*

---

## 2) Design

### 2.1 Exigences de sécurité (D‑SR)
- **Stream A — Exigences internes**
Capturer des exigences de sécurité non fonctionnelles et spécifiques au risque (authentification/authorisation, journalisation, résilience, confidentialité, etc.).
  **N1** : Lister exigences sécurité de base (authN/Z, logs, erreurs).  
  **N2** : Traçabilité exigences ↔ user stories ↔ tests.  
  **N3** : Bibliothèque vivante d’exigences et critères d’acceptation.
- **Stream B — Exigences fournisseurs/tiers**  
  **N1** : Attentes minimales (licences, vulnérabilités, SBOM).  
  **N2** : Processus d’évaluation et clauses contractuelles.  
  **N3** : Suivi continu risques tiers, revues régulières.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison *3. Perform security design review and threat modeling* (couvre la dérivation d’exigences), *2. Require use of proven security features, languages, and frameworks* et *4. Define and use cryptography standards*

### 2.2 Modélisation des menaces (D‑TA)
Identifier systématiquement les menaces, estimer le risque et choisir les mitigations.
- **Stream A — Méthode & ateliers**  
  **N1** : Introduire STRIDE/DFD et ateliers minimum viables.  
  **N2** : Menaces intégrées au design (critères, validations).  
  **N3** : Ateliers systématiques, mesures et ré‑évaluation périodique.  
- **Stream B — Risques & mitigations**  
  **N1** : Journal des menaces et contre‑mesures basiques.  
  **N2** : Modèles d’attaque/abus cases ; priorisation par risque.  
  **N3** : Cartographie des risques produit/portfolio, arbitrage budgétaire.  

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à *3. Perform security design review and threat modeling*

### 2.3 Architecture sécurisée (D‑SA)
Concevoir une architecture applicative et de données avec des contrôles défensifs, des frontières de confiance et des dépendances maîtrisées.
- **Stream A — Conception d’architecture**  
  **N1** : Schémas, *trust boundaries*, contrôles fondamentaux.  
  **N2** : Patterns de référence (auth, secrets, crypto, journaux).  
  **N3** : Architecture réutilisable (référentiel), revues indépendantes.  
- **Stream B — Technologie & dépendances**  
  **N1** : Liste approuvée de composants/versions.  
  **N2** : Politique SBOM, évaluations SCA.  
  **N3** : Gouvernance des dépendances, remplacement proactif.

{: .highlight}
> Dans Microsoft SDL, correspond aproximativement à une combinaison *3. Perform security design review and threat modeling*, *2. Require use of proven security features, languages, and frameworks* et *5. Secure the software supply chain* (choix et validation des composants).


---

## 3) Implémentation

### 3.1 Secure Build (I‑SB)
- **Stream A — Chaîne de build**  
  **N1** : Builds reproductibles, durcissement de base.  
  **N2** : Signatures, artefacts attestés, séparation des rôles.  
  **N3** : Chaîne CI/CD attestée bout‑en‑bout (politiques, contrôles).  citeturn3search20
- **Stream B — Supply chain & SBOM**  
  **N1** : SBOM minimal, scan de dépendances.  
  **N2** : Règles de provenance, validation d’intégrité.  
  **N3** : Surveillance continue, réponses coordonnées (revocation/rollback).  citeturn3search20

### 3.2 Defect Management (I‑DM)
- **Stream A — Découverte & triage**  
  **N1** : Registre centralisé des vulnérabilités.  
  **N2** : Triage par gravité (CVSS / risque), SLA de correction.  
  **N3** : Mesure de vélocité de remédiation et de ré‑ouverture.  citeturn2search9
- **Stream B — Remédiation & validation**  
  **N1** : Boucle de correction standard (PR, revue).  
  **N2** : Re‑tests automatiques post‑fix (SAST/DAST).  
  **N3** : Vérifications indépendantes et capitalisation (RCA).  citeturn2search9

### 3.3 Secure Deployment (I‑SD)
- **Stream A — Pipelines & contrôles**  
  **N1** : Gates sécurité de base dans CI/CD.  
  **N2** : Politiques de promotion, environnement immuable.  
  **N3** : Contrôles *policy‑as‑code* et attestations automatiques.  citeturn3search20
- **Stream B — Configuration & durcissement**  
  **N1** : Baselines de déploiement, secrets gérés.  
  **N2** : Durcissement systématique, *config drift* surveillé.  
  **N3** : Conformité continue, remédiation automatique.  citeturn3search20

---

## 4) Vérification

### 4.1 Security Testing (V‑ST)
- **Stream A — Baseline scalable (auto)**  
  **N1** : SAST/SCA de base intégrés au pipeline.  
  **N2** : Couverture étendue, gestion des faux positifs, seuils.  
  **N3** : Orchestration multi‑outils, corrélation des résultats.  citeturn4search35
- **Stream B — Profondeur (dyn./manuel)**  
  **N1** : DAST simple et scans ciblés.  
  **N2** : Fuzzing, tests manuels ciblés.  
  **N3** : Tests avancés (IAST/RASP/pentest coordonné).  citeturn4search35

### 4.2 Architecture Assessment (V‑AA)
- **Stream A — Revue d’architecture**  
  **N1** : Revue légère des schémas/DFD.  
  **N2** : Checklists par patterns.  
  **N3** : Revue indépendante, traçabilité complète vers exigences/menaces.  citeturn2search9
- **Stream B — Surface d’attaque & assurance**  
  **N1** : Inventaire des interfaces exposées.  
  **N2** : Scénarios d’abus et validations ciblées.  
  **N3** : Assurance continue sur composants critiques.  citeturn2search9

### 4.3 Requirements‑Driven Testing (V‑RT)
- **Stream A — Couverture par exigences**  
  **N1** : Lier cas de test aux exigences (ex. ASVS).  
  **N2** : Mesurer la couverture et les écarts.  
  **N3** : Exiger la conformité préalable à release (gates).  citeturn2search9
- **Stream B — Preuve & acceptation**  
  **N1** : Évidences de test stockées.  
  **N2** : Traçabilité V&V (req→test→résultat).  
  **N3** : Audits réguliers et amélioration du référentiel de tests.  citeturn2search9

---

## 5) Opérations

### 5.1 Operational Management (O‑OM)
- **Stream A — Exploitation sécurisée**  
  **N1** : Procédures *runbooks*, sauvegardes, accès minimaux.  
  **N2** : Revue périodique des privilèges et des configurations.  
  **N3** : Gestion de configuration centralisée et contrôles préventifs.  citeturn2search9
- **Stream B — Continuité & résilience**  
  **N1** : Plan basique de continuité (restauration, DR).  
  **N2** : Exercices réguliers, *chaos days* limités.  
  **N3** : Résilience mesurée (RTO/RPO), tests à l’échelle.  citeturn2search9

### 5.2 Incident Management (O‑IM)
- **Stream A — Détection & triage**  
  **N1** : Journalisation et alertes minimales.  
  **N2** : Playbooks de triage, classification des incidents.  
  **N3** : Détection enrichie (corrélation), métriques MTTA/MTTR.  citeturn2search9
- **Stream B — Réponse & *lessons learned***  
  **N1** : Processus de réponse basique (qui fait quoi, quand).  
  **N2** : Communications, escalades, rétention d’artefacts.  
  **N3** : Post‑mortems systématiques, boucles d’amélioration.  citeturn2search9

### 5.3 Environment Management (O‑EM)
- **Stream A — Environnements & secrets**  
  **N1** : Séparation dev/test/prod, gestion de secrets.  
  **N2** : Baselines durcies, accès machine/service minimaux.  
  **N3** : Politique de rotation/attestation, revues périodiques.  citeturn2search9
- **Stream B — Observabilité & monitoring**  
  **N1** : Journaux essentiels centralisés.  
  **N2** : Détection d’anomalies, tableaux de bord opérationnels.  
  **N3** : Télémétrie avancée, détection comportementale et réponse auto.  citeturn2search9

---

## Références (sélection)
- **OWASP — About** : mission, activités, communauté.  
  https://owasp.org/about/
- **OWASP SAMM — Quick Start Guide (v2)** : structure (5 fonctions, 15 pratiques, 2 streams, maturité).  
  https://owaspsamm.org/guidance/quick-start-guide/
- **OWASP DevGuide — SAMM** : présentation hiérarchique (fonctions, pratiques, streams, maturité).  
  https://devguide.owasp.org/en/08-culture-process/03-samm/
- **SAMM v2 Datamodel (GitHub)** : noms exacts des fonctions et pratiques (G‑SM, G‑PC, G‑EG, D‑SR, D‑TA, D‑SA, I‑SB, I‑DM, I‑SD, V‑ST, V‑AA, V‑RT, O‑OM, O‑IM, O‑EM).  
  https://github.com/OWASP/samm/tree/master/Supporting%20Resources/v2.0/Datamodel/Datafiles
- **Microsoft — SDL Practices (10 pratiques)** : liste des pratiques et positionnement SDL.  
  https://www.microsoft.com/en-us/securityengineering/sdl/practices
- **OWASP SAMM Blog — Comparing Microsoft SDL and SAMM (2025)** : cadrage des rapprochements et étendue de couverture.  
  https://owaspsamm.org/blog/2025/01/20/comparing-microsoft-sdl-and-samm/

---

> *Note pédagogique* : pour un usage en classe, tu peux transformer ces sections en cartes mémoire : « Définition » au recto, « Équivalent SDL » au verso, ou construire une matrice Fonctions × Pratiques × SDL pour des exercices de mise en correspondance.
