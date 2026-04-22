---
layout: default
title: "Pare-feu Linux"
parent: "Durcissement d'un serveur"
nav_order: 1
published: true
---

# Pare-feux sous Linux

Un **pare-feu** (*firewall* en anglais) est un mécanisme de sécurité chargé de **contrôler, filtrer et éventuellement bloquer** les communications réseau entrant et sortant d’un système. Ses objectifs principaux sont :

- limiter la surface d’attaque
- empêcher les accès non autorisés
- contrôler quels services sont exposés

{: .astuce}
> La règle de base en sécurité est : **tout bloquer par défaut, puis autoriser seulement ce qui est nécessaire.**

---

## Netfilter : le moteur du pare-feu Linux

**Netfilter** est un module du noyau Linux permettant :
- le filtrage de paquets
- le suivi des connexions (stateful firewall)
- la modification de paquets (NAT)

Tous les outils de pare-feu Linux reposent sur Netfilter.

---

### Points d’ancrage (*hooks*)

Lorsqu’un paquet traverse la pile réseau Linux, il passe par des points appelés ***hooks*** :

- **PREROUTING** : avant décision de routage
- **INPUT** : paquet destiné à la machine
- **FORWARD** : paquet traversant la machine
- **OUTPUT** : paquet généré localement
- **POSTROUTING** : après routage

Ces *hooks* permettent à Netfilter d’intercepter et traiter les paquets.

---

### Pare-feu *stateful*

Un pare-feu *stateful* suit l’état des connexions :

- **NEW** : nouvelle connexion
- **ESTABLISHED** : connexion établie
- **RELATED** : connexion liée (ex. FTP, ICMP)

Cela permet d’autoriser automatiquement les réponses sans ouvrir tous les ports.

---

## Outils basés sur Netfilter

- **iptables** : outil bas niveau
- **UFW** : interface simplifiée basée sur iptables
- **nftables** : alternative à iptables, plus moderne mais adoption relativement lente

---

## Bonnes pratiques générales

- bloquer par défaut
- limiter les services exposés
- tester avant activation
- documenter les règles
- journaliser les événements critiques

---

## En résumé

- Netfilter est le coeur du pare-feu Linux
- iptables est l'outils le plus populaire pour interagir avec Netfilter
- UFW est une couche supplémentaire sur iptables pour en faciliter l'utilisation
