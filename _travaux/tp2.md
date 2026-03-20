---
layout: default
title: "Travail pratique 2"
nav_order: 2
published: false
---

# Travail pratique 2: Chez Oups Technologies, on essaie de casser moins souvent

## Mise en situation
Vous poursuivez votre stage chez **Oups Technologies**, la célèbre entreprise au slogan :

> *Développer vite. Casser souvent.*

Suite à votre bon travail de sécurisation du portail, les dirigeants de l'entreprise vous demandent d'analyser l'application de réservation de locaux complète. On s'intéresse aux vulnérabilités potentielles pouvant affecter le système, ainsi qu'à des contre-mesures qui pourraient être mises en place dès maintenant pour les mitiger.

Votre mandat : **analyser l’application fournie (code inclus)**, produire une **modélisation de la menace STRIDE** et proposer des mesures de mitigation pour tous les risques identifiés qui dépassent le seul *faible*.

---
## Objectifs pédagogiques
- Explorer la structure d’une application Web réelle
- Identifier les composantes pertinentes pour une analyse STRIDE
- Réaliser une **analyse STRIDE complète** appliquée à une application existante
- Proposer des mesures de mitigation **réalistes** et **appropriées**

---
## Préparation
Vous trouverez dans le dépôt suivant l’application à analyser :

> **Dépôt Git du TP** : (fictif — à remplacer par votre dépôt réel)
> https://git.oups.tech.test/tp-stride-reservation

L’application inclut :
- un serveur Web (backend) en Flask / Node / Java (selon version)
- un module d’authentification interne
- un module de réservation
- un module « moteur de disponibilité » (processus complexe)
- une base SQLite ou JSON locale

Clonez le projet et familiarisez-vous avec :
- l’arborescence du code
- les routes et endpoints
- les modèles de données
- la logique du moteur de disponibilité

---
## Tâches à réaliser

### 1. Analyse des composantes
À partir du code fourni :
1. Identifiez les **processus** (simples ou complexes)
1. Identifiez les **entités externes**
1. Identifiez les **stockages**

#### Questions
1. Quels sont les entités que vous avez identifiées ?
1. Quel est le rôle exact du moteur de disponibilité ?
1. Quels types de données sont enregistrés dans les différents stockages ?

---

### 2. Construction des DFD
Créez :
- un **DFD niveau 0 complet**

Si un ou plusieurs processus complexe(s), produisez :
- un (1) DFD de niveau 1 pour un processus complexe.

#### Questions
1. Quels flux de données impliquent les utilisateurs ?
1. Quels flux interagissent avec les stockages ?
1. Quels flux sont les plus sensibles ? Pourquoi ?

---

### 3. Ajout des frontières de confiance
Sur vos DFD, ajoutez des frontières de confiance aux endroits appropriés.

1. Quel(s) critère(s) avez-vous utilisé(s) pour déterminer où ajouter les frontières de confiance ?

---

### 4. Modélisation STRIDE
Pour chaque élément du système (entités externes, processus, stockages, flux), identifiez :
- les menaces potentielles
- les menaces réelles
- un exemple ou scénario d'attaque concret pour chaque menace réelle

#### Questions
1. Quelles sont les menaces et les scénarios d'attaque que vous avez identifiés ?

---

### 5. Analyse du risque
Attribuez une cote de risque à chaque menace réelle identifiée à l'étape précédente. Pour chaque menace dont la cote de risque est supérieure à **faible**, proposez au moins une contre-mesure qui devrait être mise en place.

#### Questions
1. Quelles menaces avez-vous identifiées ? Quelles sont les contre-mesures associées ?


### 6. Recommandations de sécurité
À partir de votre analyse STRIDE :
- Proposez **5 recommandations prioritaires**
- Assurez-vous qu’elles soient réalistes, applicables au code fourni et cohérentes

#### Questions
1. Quelles sont vos 5 recommandations finales ? Justifiez.

---
## Évaluation
Critère | Points
------- | ------
Section 1 : Analyse des composantes | 15
Section 2 : Construction des DFD | 25
Section 3 : Modélisation STRIDE | 25
Section 4 : Analyse de risque | 20
Section 5 : Recommandations de sécurité | 15
**Total** | **100 points (20% de la note finale)**

---

## À remettre
- Un fichier **.zip** contenant :
  - Les réponses aux **questions** des **sections 1 à 5** (dans un document PDF, Word, LibreOffice ou Markdown)
  - Tous les diagrammes DFD (Mermaid ou PNG)
- Le travail est à remettre sur **Léa (Omnivox)** dans la section **Travaux - Énoncés et remises**
- Le travail est **individuel** (une remise par personne)
- Le travail est à remettre au plus tard le **13 avril 2026** en fin de journée.

---

Bon courage ! 🔐🚀
