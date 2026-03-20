---
layout: default
title: Modélisation de la menace
nav_order: 4
has_children: true
has_toc: true
published: false
---

# Modélisation de la menace

La cybersécurité moderne ne se limite plus à installer un antivirus ou à appliquer des mises à jour. Les systèmes que nous développons ou administrons sont de plus en plus complexes, exposés à Internet, interconnectés, intégrés à des services cloud... et donc plus vulnérables.

Modéliser la menace, c’est prendre une longueur d’avance. Il s’agit de répondre à des questions fondamentales :

- Quels acteurs pourraient s’en prendre à mon système ?
- Quelles parties du système sont les plus fragiles ?
- Quelles attaques sont plausibles, probables ou critiques ?
- Quelles protections sont réellement prioritaires ?

{: .astuce}
> *La modélisation de la menace permet de comprendre comment un système peut être attaqué avant qu’il ne le soit.*

## Qu’est-ce qu’une menace ?
Une menace est :
- Un acteur (personne, groupe, organisation)...
- ... qui a une intention malveillante ou opportuniste...
- ... et un moyen d’exploiter une vulnérabilité...
- ... pour causer un impact négatif sur un système.

Une menace n’est pas encore une attaque : c’est un risque potentiel.

## Menaces, vulnérabilités et risques
| Concept | Définition | Métaphore |
|--------|------------|-----------|
| **Menace** | Quelque chose ou quelqu’un qui pourrait faire du mal | Le cambrioleur |
| **Vulnérabilité** | Faiblesse exploitable dans un système | La fenêtre laissée ouverte |
| **Risque** | Menace × vulnérabilité × impact | Probabilité que le cambrioleur passe par la fenêtre |

## Avantages

### Prioriser les protections
En comprenant mieux les menaces et les risques associés, on peut prioriser l'ajout de certaines protections.On ne protège pas tout au même niveau.

### Mieux comprendre les chemins d’attaque possibles
Un système vu « de l’intérieur » semble simple.

### Réduire les coûts
Corriger une faiblesse avant le développement coûte beaucoup moins cher.

### Normaliser la compréhension
Tout le monde parle le même langage.

## Méthodologies
Il existe plusieurs méthodologies utilisées dans l'industrie pour modéliser la menace, chacune ayant ses forces et ses inconvénients : 

| Méthode | Approche | Points forts |
|---------|----------|--------------|
| **STRIDE** | Classification des menaces | Simple, très populaire |
| **PASTA** | Approche orientée risque | Très complet |
| **LINDDUN** | Vie privée | Excellent pour RGPD |
| **MITRE ATT&CK** | Catalogue des techniques | Réaliste, orienté défense |

Pour le cours de cybersécurité, nous utiliserons la méthode STRIDE.
