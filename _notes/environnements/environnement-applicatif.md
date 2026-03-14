---
layout: default
title: "Environnement applicatif"
parent: "Environnements de travail"
nav_order: 1
published: true
has_toc: false
---

# Environnement applicatif

La machine applicative est l'environnement utilisé pour développement et les opérations de sécurité *défensive*. Au fil de la session, elle servira notamment pour les fonctions de **SAMM** suivantes : 
- **Conception** : gestion et modélisation de la menace, pratiques sécuritaires en développement (exigences).
- **Implémentation** : analyse des composantes (*SCA*) et nomenclature logicielle (*SBOM*).
- **Verification** : analyse statique (*SAST*), tests basés sur les exigences.
- **Opérations** : journalisation, gestion du pare‑feu, détection et réponse aux intrusions, durcissement.

Cet environnement est fourni sous forme d'une machine virtuelle en format **.ova** pouvant être importée directement sous **VMware Workstation** ou **Virtualbox**.*

{. :highlight}
> Un guide d'installation des divers outils sur d'autres plateformes (Windows, Mac) est disponible [ici](../notes/installation-outils). Cependant, cette configuration n'est pas officiellement supportée pour le cours et est à utiliser à ses propres risques.

## Cas d’usage
- **TP1** : Nginx, HTTPS/TLS
- **TP2** : Modélisation de la menace (STRIDE)
- **TP3** : UFW, fail2ban et journalisation (*logs*)
- **Projet** : Développement de l'application vulnérable et correctifs
- **Ateliers** : SAST, SCA, SBOM, OWASP Top 10
- **Test d'intrusion** : Re‑tests et scores CVSS


## Outils utilisés

### Développement & IDE
- **Visual Studio Code** : IDE principal pour les exercices demandant l'écriture ou l'analyse de code
- **Git** : versionnement du code
- **Python / pip / pipx** : l'un des langages de programmation utilisés dans le cours
- **Java (JDK et JRE) / Maven** : l'un des langages de programmation utilisés dans le cours, et nécessaire pour WebGoat

### Cryptographie / Sécurité réseau
- **OpenSSL** : génération de matériel cryptographique (clés, certificats, etc)
- **Nginx** : serveur web
- **Wireshark** : analyseur de paquets IP

### SAST / SCA / SBOM
- **Semgrep** *(SAST)* : analyse statique.
- **OWASP dependency-check** *(SCA)* : analyse des dépendances
- **OWASP CycloneDX** *(SBOM)* : génération de la nomenclature logicielle (*SBOM - software build of material*)

### Durcissement & Opérations
- **UFW** : pare‑feu simple, réduction surface d’attaque.
- **fail2ban** : détection d'intrusions et réponse basée sur les journaux.

### Applications vulnérables
- **Juice Shop** : vulnérabilités modernes (DAST, OWASP Top 10).
- **WebGoat** : exercices guidés OWASP (Java/JAR).

### Outils réseau & utilitaires
- **nmap**, **netcat**, **dnsutils**, **traceroute**, **whois**, **tcpdump**
- **jq**, **tree**, **vim/nano**
- **rsyslog**
