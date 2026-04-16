---
layout: default
title: Common Vulnerability Scoring System (CVSS)
nav_order: 11
has_children: true
has_toc: true
published: true
---

# CVSS - Common Vulnerability Scoring System

Le **CVSS (*Common Vulnerability Scoring System*)** est un standard ouvert utilisé pour évaluer la gravité d’une vulnérabilité de sécurité de manière **standardisée, cohérente et comparable**.  
Il permet d’attribuer un **score numérique**, généralement compris entre **0.0 et 10.0**, représentant le niveau de risque associé à une vulnérabilité donnée.

Le CVSS ne mesure pas si une vulnérabilité est *présente* dans une application; il mesure **la sévérité intrinsèque** de la vulnérabilité elle‑même, indépendamment du contexte exact dans lequel elle est exploitée.

Ce système est largement utilisé par :
- la *National Vulnerability Database* (*NVD*)
- les éditeurs de logiciels
- les outils d'analyse de composition logicielle (*SCA*)
- les équipes de sécurité
- les analystes de risque


L’objectif du CVSS est de fournir un **langage commun** permettant de répondre à une question essentielle : **À quel point cette vulnérabilité est‑elle grave ?**

{: .highlight}
> Même si la version 4.0 du CVSS existe, le présent document présente la version 3.x, puisqu'il s'agit encore aujourd'hui de la version la plus utilisée dans l'industrie. En effet, l'adoption de CVSS 4.0 est lente et plusieurs vulnérabilités n'ont pas encore été évaluées selon les critères de CVSS 4.0; par conséquent, elles n'ont pas encore de score CVSS 4.0.

---

## Objectif
Le CVSS est utilisé principalement pour :

### Prioriser les vulnérabilités
Lorsque des dizaines (voire des centaines) de vulnérabilités sont détectées, le CVSS aide à identifier **quelles failles traiter en priorité**.


### Orienter la gestion du risque
Les scores CVSS servent de base pour :
- établir des seuils d’intervention
- définir des SLA de correction
- justifier des décisions de sécurité

Exemple :
- CVSS ≥ 9.0 : correction immédiate  
- CVSS entre 7.0 et 8.9 : prioritaire  
- CVSS < 4.0 : à documenter


### Uniformiser l’analyse entre outils et équipes
Le CVSS permet de comparer :
- des vulnérabilités issues de différents projets,
- des composants *open source* variés,
- des rapports provenant de différents scanners.

---

## Échelle des scores CVSS
Le score CVSS est généralement interprété ainsi :

| Score | Sévérité |
|------:|----------|
| 0.0 | Aucune |
| 0.1 – 3.9 | Faible |
| 4.0 – 6.9 | Moyenne |
| 7.0 – 8.9 | Élevée |
| 9.0 – 10.0 | Critique |

---

## Structure du CVSS
Le score CVSS est composé de **plusieurs groupes de métriques**.

### 1. Score de base (*Base Score*)
C’est le **score principal** et le plus utilisé. Il mesure la gravité intrinsèque de la vulnérabilité.

Il inclut notamment :
- le vecteur d’attaque (réseau, local, physique),
- la complexité de l’attaque,
- les privilèges requis,
- l’interaction utilisateur,
- l’impact sur la confidentialité, l’intégrité et la disponibilité.

{: .highlight}
> C’est ce score qui est affiché par défaut dans la majorité des outils SCA.

---

### 2. Score temporel (*Temporal Score*)
Ce score tient compte de **l’évolution dans le temps**, par exemple :
- disponibilité d’un exploit public
- maturité du correctif
- niveau de confirmation de la vulnérabilité

Ce score est peu utilisé en pratique, mais utile pour ajuster le risque.

---

### 3. Score environnemental (*Environmental Score*)
Ce score adapte la vulnérabilité au **contexte spécifique** de l’organisation :
- importance réelle de la ressource affectée
- exposition du composant
- mesures de sécurité déjà en place

C’est le score le **plus représentatif du risque réel**, mais aussi le moins automatisé.

---

## Calcul du CVSS
Le calcul du score CVSS peut sembler complexe au premier abord, mais il repose sur une logique structurée et cohérente. L’idée n’est pas que chaque développeur mémorise la formule mathématique exacte, mais plutôt qu’il **comprenne les critères qui influencent le score** et leur impact sur la gravité finale.

Le score CVSS est calculé à partir de plusieurs **métriques normalisées**, regroupées principalement dans le **score de base**. Chaque métrique représente un aspect précis de la vulnérabilité, et l’ensemble est combiné pour produire un score final compris entre 0.0 et 10.0.

### Métriques du score de base (CVSS 3.x)
Le **score de base** est composé de deux grandes catégories :
1. Exploitabilité
2. Impact

---

### Métriques d'exploitabilité
Ces métriques décrivent **à quel point il est facile d’exploiter la vulnérabilité**.

#### **AV - Attack Vector (Vecteur d’attaque)**
Indique comment l’attaque peut être menée :
- **Network (N)** : exploitable à distance via le réseau
- **Adjacent (A)** : réseau local ou voisin
- **Local (L)** : accès local requis
- **Physical (P)** : accès physique requis

Plus l’attaque peut être faite à distance, plus le score augmente.

#### **AC - Attack Complexity (Complexité)**
Mesure la difficulté technique de l’exploitation :
- **Low (L)** : attaque simple, fiable
- **High (H)** : conditions particulières nécessaires

Une attaque simple augmente la sévérité.


#### **PR - Privileges Required (Privilèges requis)**
Décrit le niveau d’accès nécessaire :
- **None (N)** : aucun privilège requis
- **Low (L)** : accès limité
- **High (H)** : accès administrateur requis

Moins il faut de privilèges, plus le score est élevé.


#### **UI - User Interaction (Interaction utilisateur)**
Indique si une action humaine est nécessaire :
- **None (N)** : aucune interaction requise
- **Required (R)** : clic, ouverture de fichier, etc.

Une vulnérabilité exploitable sans interaction est jugée plus grave.

---

### Métriques d’impact
Ces métriques mesurent **les conséquences de l’exploitation réussie**.

#### **C - Confidentiality (Confidentialité)**
Impact sur les données :
- None (N)
- Low (L)
- High (H)


#### **I - Integrigy (Intégrité)**
Impact sur la modification des données :
- aucune modification
- modification partielle
- modification complète


#### **A - Availability (Disponibilité)**
Impact sur le service :
- pas d’impact,
- dégradation,
- indisponibilité complète.

---

### **S - Scope (Portée)**
La portée est une métrique n'appartenant ni aux métriques d'exploitabilité, ni aux métriques d'impact. Il s'agit d'une métrique transversale qui indique si la vulnérabilité affecte uniquement le composant vulnérable ou si elle permet d’impacter **d’autres composants**.

- **Unchanged (U)** : même périmètre de sécurité
- **Changed (C)** : franchissement d’une frontière de sécurité

Une portée modifié augmente fortement le score final.

---

## Combinaison des métriques
Toutes ces métriques sont combinées à l’aide d’une **formule standardisée** définie par le consortium CVSS. Cette formule :
- applique différents poids à chaque métrique
- produit un score intermédiaire
- arrondit le résultat à un chiffre décimal

En pratique, **les outils SCA ne recalculent pas ce score** : ils utilisent celui publié dans la NVD ou par l’éditeur.

---

## CVSS vs SCA
Les outils d'analyse de composition logicielle (*SCA*) utilisent massivement le CVSS pour :
- attribuer un niveau de gravité aux CVE détectées
- filtrer les vulnérabilités
- générer des rapports lisibles
- automatiser des décisions dans les pipelines CI/CD.

{: .highlight}
> Les outils de SCA tels que **Dependency‑Check**, **Black Duck** et **Snyk* s'appuient tous sur les scores CVSS publiés.

---

## En résumé
Le **CVSS** est un standard fondamental pour comprendre et prioriser les vulnérabilités détectées par les outils SCA. Il apporte une mesure commune de sévérité, facilitant :

- la comparaison
- la priorisation
- la prise de décision

Toutefois, le CVSS ne remplace pas l’analyse humaine : il doit être utilisé **avec discernement**, en tenant compte du contexte réel de l’application.

---

## Liens utiles
- Documentation officielle : [https://www.first.org/cvss/](https://www.first.org/cvss/)
- CVSS sur Wikipedia : [https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System)
