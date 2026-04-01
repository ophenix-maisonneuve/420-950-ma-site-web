---
layout: default
title: "Projet"
nav_order: 3
published: false
---

# Projet : Oups Technologies - Un slogan ? Quel slogan ?

## Mise en situation
Vous continuez votre travail chez **Oups Technologies**. Grâce à votre travail incroyable, le slogan...

> *Développer vite. Casser souvent.*

... devient obsolète plus vite que la neige qui fond au printemps!

La modélisation de la menace que vous avez produite a fait réaliser à plusieurs dirigeants l'importance d'introduire des pratiques de sécurité à toutes les étapes du développement d'un logiciel. Vos recommandations finales les ont aussi convaincus qu'avant d'ouvrir leur portail de réservation sur l'Internet, il serait prudent de pousser l'analyse plus loin et de s'assurer qu'elle est exempte de failles majeures. 

Un consultant en sécurité rencontré dans une conférence leur a parlé de failles très présentes dans les logiciels, notamment :

- L'injection SQL
- Le *cross-site scripting* (réflété ou stocké)
- L'usurpation de jeton JWT
- La traversée de répertoires (*path traversal*)
- L'injection de commandes / scripts
- L'accès non-sécurisé direct aux ressources (*IDOR*)

Se fiant à nouveau sur votre expertise, ils vous demandent de leur préparer une démonstration de ces différentes failles ainsi que des contre-mesures à mettre en place pour les mitiger.

---
## Objectifs pédagogiques
- Utiliser des outils de SAST/SCA pour détecter des vulnérabilités présentes dans une appliation existante
- Comprendre les caractéristiques d'une implémentation vulnérables à certains types d'attaques
- Proposer des correctifs et/ou des contre-mesures pour les vulnérabilités identifiées
- Générer une nomenclature logicielle (*SBOM*) pour une application

---
## Préparation
Le code de départ du projet est disponible dans le dépôt suivant (il s'agit pratiquement du même code que pour le TP2, à quelques exceptions près): [Code portail Oups Technologies](https://github.com/ophenix-420-950-ma-24636/projet)

1. Il est d'abord recommandé de créer un environnement virtuel Python afin d'y installer toutes les dépendances sans affecter les paquets installés sur le système:
    ```bash
    python3 -m venv env
    source env/bin/activate
    ```
2. Ensuite, on installe les dépendances
    ```bash
    pip install flask werkzeug
    ```

3. Finalement, on démarre l'application Python/Flask
    ```bash
    cd portail
    python3 -m flask run
    ```

L'application démarre alors un serveur de développement qui expose l'application sur http://127.0.0.1:5000

Un seul utilisateur est créé automatiquement au premier démarrage de l'application:
- Utilisateur: admin
- Mot de passe: admin

L’application inclut :
- une application web (*Frontend*)
- une application (backend) utilisant Python/Flask
- un module d’authentification
- un moteur de réservation
- une base de données

Clonez le projet et familiarisez-vous avec :
- l’arborescence du code
- les routes et endpoints
- les modèles de données
- la logique du moteur de réservation

---
## Tâches à réaliser
Le travail est à réaliser en équipes de 3 personnes. Pour la partie 2, chaque équipe aura à implémenter et à corriger une vulnérabilité différente

### 1. Ajouter la prise en charge multi-utilisateurs
Dans le code fourni, un seul utilisateur est créé au premier démarrage. De plus, la liste des réservations n'affiche que les réservations effectuées par l'utilisateur connecté, ce qui n'est pas très pratique pour tenter de réserver une salle. Vous devrez ajouter :
- Une nouvelle section dans l'interface web qui montre les salles réservées par d'autres utilisateurs
- Le code nécessaire à cette fonctionnalité dans les services backend

{. :highlight}
> Pour les fins du projet, il est acceptable que les utilisateurs soient ajoutés une seule fois au premier démarrage de l'application. Autrement dit, il n'est pas nécessaire d'ajouter la fonctionnalité d'ajout/suppression d'utilisateurs dans l'application.

### 2. Démonstration de code vulnérable
Chaque équipe choisira une vulnérabilité parmi les suivantes (premier arrivé, premier servi) :
- L'injection SQL
- Le *cross-site scripting* réflété
- Le *cross-site scripting* stocké
- L'usurpation de jeton JWT
- La traversée de répertoires (*path traversal*)
- L'injection de commandes / scripts
- L'accès non-sécurisé direct aux ressources (*IDOR*)

Vous devrez d'abord, pour la vulnérabilité choisie, introduire volontairement cette vulnérabilité à l'application. 

### 3. Analyse SAST

### 4. Analyse SCA

### 5. Correction des vulnérabilités identifiées

### 6. SBOM

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
- Le travail est à remettre au plus tard le **16 avril 2026** en fin de journée.

---

Bon courage ! 🔐🚀
