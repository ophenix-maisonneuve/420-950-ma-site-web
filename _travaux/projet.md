---
layout: default
title: "Projet"
nav_order: 3
published: true
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

---

## Tâches à réaliser

### 1. Ajouter la prise en charge multi-utilisateurs
Dans le code fourni, un seul utilisateur est créé au premier démarrage. De plus, la liste des réservations n'affiche que les réservations effectuées par l'utilisateur connecté, ce qui n'est pas très pratique pour tenter de réserver une salle. Vous devrez ajouter :
- Une nouvelle section dans l'interface web qui montre les salles réservées par d'autres utilisateurs
- Le code nécessaire à cette fonctionnalité dans les services backend

{: .highlight}
> Pour les fins du projet, il est acceptable que les utilisateurs soient ajoutés une seule fois au premier démarrage de l'application. Autrement dit, il n'est pas nécessaire d'ajouter la fonctionnalité d'ajout/suppression d'utilisateurs dans l'application.

### 2. Démonstration de code vulnérable
Chaque équipe choisira **une seule** (1) vulnérabilité parmi les suivantes :

- L'injection SQL
- Le *cross-site scripting* réflété
- Le *cross-site scripting* stocké
- La manipulation de jeton JWT
- La traversée de répertoires (*path traversal*)
- L'injection de commandes / scripts
- L'accès non-sécurisé direct aux ressources (*IDOR*)

Pour la vulnérabilité choisie, introduisez volontairement cette vulnérabilité à l'application. En d'autres termes, ajoutez du code à une nouvelle fonctionnalité ou à une fonctionnalité existante qui présente la vulnérabilité choisie.

### 3. Analyse SAST
À l'aide de l'outil **Semgrep**, effectuez une analyse statique de l'application. Normalement, cette analyse devrait détecter la vulnérabilité introduite volontairement à l'étape précédente ainsi que, possiblement, d'autres vulnérabilités qui étaient déjà présentes.

***Notez les vulnérabilités identifiées***

{: .highlight}
> Comme nous le remarquerons pendant les cours, les outils *SAST*, en particulier leurs versions gratuites, n'attrapent pas tous les problèmes. Si votre vulnérabilité n'a pas été rapportée par l'outil, cela ne veut pas nécessairement dire que vous l'avez mal implémentée.

### 4. Analyse SCA
À l'aide de l'outil **Dependency-Check**, effectuez une analyse des composantes logicielles (*SCA*) de l'application. 

***Notez les vulnérabilités identifiées***

### 5. Correction des vulnérabilités identifiées
Pour chaque vulnérabilités identifiées aux étapes 3 et 4 (incluant celle que vous avez introduite volontairement), effectuez l'une des actions suivantes, selon le cas :
- S'il s'agit d'un faux positif ou d'une vulnérabilité non-exploitable, indiquez-le et justifiez.
- S'il s'agit d'une vulnérabilité exploitable : 
    - Corrigez la vulnérabilité (correctif de code, mise à jour d'une librairie, etc)
    - S'il n'existe pas de correctif direct, par exemple dans le cas d'une librairie pour laquelle il n'existe pas encore de mise à jour, expliquez et proposez des méthodes de contournement alternatives.


{: .warning}
> Ne supprimez pas le code vulnérable que vous aviez introduit volontairement, car il sera également évalué. Si votre application respecte bien les principes SOLID ( ;) ), il devrait être assez simple de fournir une implémentation alternative au code vulnérable et de remplacer l'implémentation vulnérable par l'implémentation corrigée. Si ce n'est pas possible, vous pouvez aussi mettre en commentaires l'implémentation fautive et la remplacer par la correction.

### 6. SBOM
Finalement, à l'aide de l'outil **CycloneDX**, produisez une nomenclature logicielle (*SBOM*) de l'application.

---

## Évaluation

Critère | Points
------- | ------
Section 1 : Prise en charge multi-utilisateurs | 10
Section 2 : Ajout du code vulnérable | 25
Section 3 : Analyse SAST | 15
Section 4 : Analyse SCA | 15
Section 5 : Correction des vulnérabilités | 25
Section 6 : Nomenclature logicielle (SBOM) | 10
**Total** | **100 points (25% de la note finale)**

---

## À remettre
- Un fichier **.zip** contenant :
    - Le code final contenant :
        - L'implémentation démontrant volontairement la vulnérabilité (peut être en commentaires au besoin)
        - La version corrigée du portail de réservation contenant tous les correctifs que vous aurez implémentés
    - Le rapport SAST
    - Le rapport SCA
    - Les justifications / explications pour les vulnérabilités qui n'ont pas été corrigées dans le code.
    - La nomenclature logicielle (SBOM)
- Le travail est à remettre sur **Léa (Omnivox)** dans la section **Travaux - Énoncés et remises**
- Le travail se fait en **équipe de 3 personnes** (une remise par équipe)
- Le travail est à remettre au plus tard le **21 mai 2026** en fin de journée.

---

**Oups Technologies compte sur vous!** 
