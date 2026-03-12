---
layout: default
title: "Machine principale"
parent: "Environnements de travail"
nav_order: 1
published: true
has_toc: false
---

# Machine principale

La machine principale est l'environnement utilisé pour développement et les opérations de sécurité *défensive*. Au fil de la session, elle servira notamment pour les fonctions de **SAMM** suivantes : 
- **Conception** : gestion et modélisation de la menace, pratiques sécuritaires en développement (exigences).
- **Implémentation** : analyse statique (*SAST*), analyse des composantes (*SCA*) et nomenclature logicielle (*SBOM*).
- **Verification** : tests dynamiques (DAST, fuzzing, etc) sur des applications vulnérables locales.
- **Opérations** : journalisation, gestion du pare‑feu, détection et réponse aux intrusions, durcissement.

Pour mettre en place la machine principale, les deux options suivantes peuvent être utilisées :
- Utiliser la machine virtuelle Linux (Linux Mint Debian Edition 7) pré-configurée (recommandé)
   - *Cette machine virtuelle est fournie en format .ova et peut donc être importée directement sous **VMware Workstation** ou **Virtualbox**.*
- Installer tous les outils directement sur la machine Windows (possiblement avec une dépendance à WSL) ou Mac

## Cas d’usage
- **TP1** : Nginx, HTTPS/TLS
- **TP2** : Modélisation STRIDE
- **TP3** : ZAP, UFW, fail2ban et journalisation (*logs*)
- **Projet** : Développement de l'application vulnérable et correctifs
- **Ateliers** : SAST, SCA, SBOM, OWASP Top 10
- **Test d'intrusion** : Re‑tests et scores CVSS


## Outils utilisés

### Développement & IDE
- **Visual Studio Code** : IDE principal pour les exercices demandant l'écriture ou l'analyse de code
- **Python 3 / pip** : l'un des langages de programmation utilisés dans le cours
- **Java 25 (JDK et JRE)** : l'un des langages de programmation utilisés dans le cours, et nécessaire pour WebGoat

### Cryptographie / Sécurité réseau
- **OpenSSL** : génération de matériel cryptographique (clés, certificats, etc)
- **Nginx** : serveur web
- **Wireshark** : analyseur de paquets IP

### SAST / SCA / SBOM
- **Semgrep** *(SAST)* : analyse statique.
- **Trivy** *(SCA)* : analyse des dépendances
- **Syft** *(SBOM)* : génération de la nomenclature logicielle (*SBOM - software build of material*)
- **Dependency‑Check**.

### DAST / Fuzzing
- **OWASP ZAP** : proxy, spider, scans actifs/passifs, fuzzing.

### Durcissement & Opérations
- **UFW** : pare‑feu simple, réduction surface d’attaque.
- **fail2ban** : détection d'intrusions et réponse basée sur les journaux.
- **rsyslog** : logs système.
- **tcpdump** : analyse réseau bas niveau.

### Applications vulnérables
- **Juice Shop** : vulnérabilités modernes (DAST, OWASP Top 10).
- **WebGoat** : exercices guidés OWASP (Java/JAR).

### Outils réseau & utilitaires
- **nmap**, **netcat**, **dnsutils**, **traceroute**, **whois**
- **jq**, **tree**, **vim/nano**