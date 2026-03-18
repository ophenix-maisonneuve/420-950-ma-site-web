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

Cet environnement est fourni sous forme d'une machine virtuelle en format **.ova** pouvant être importée directement sous **VMware Workstation** ou **Virtualbox**.

[Télécharger l'environnement applicatif](https://cmaisonneuveqcca-my.sharepoint.com/:u:/r/personal/ophenix_cmaisonneuve_qc_ca/Documents/Cours/420-950-MA%20-%20Cybers%C3%A9curit%C3%A9/Mat%C3%A9riel%20de%20cours/420-950-MA-applicatif.ova?csf=1&web=1&e=9LSWNF)

{: .highlight}
> Un guide d'installation des divers outils sur d'autres plateformes (Windows, Mac) est disponible [ici](../notes/installation-outils). Cependant, cette configuration n'est pas officiellement supportée pour le cours et est à utiliser à ses propres risques.

## Information générale
* Nom d'utilisateur : **dev**
* Mot de passe : **dev**

Pour modifier le mot de passe par défaut:
```bash
passwd
```
Le système vous demandera votre mot de passe courant ainsi que le nouveau mot de passe désiré.

## Usage
- **Ateliers pendant le cours** : Cryptographie, Certificats X.509, STRIDE, SAST, SCA, SBOM, OWASP Top 10
- **TP1** : Nginx, HTTPS/TLS
- **TP2** : Modélisation de la menace (STRIDE)
- **TP3** : UFW, fail2ban et journalisation (*logs*)
- **Projet** : Développement de l'application vulnérable et correctifs
- **Test d'intrusion** : Re‑tests et scores CVSS

## Outils pré-installés

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

---

## Configuration

La machine virtuelle a été créée avec une configuration minimale:

- 1 CPU
- 2 GB de mémoire
- Mémoire vidéo minimale
- Aucune accélération 3D

Si votre système hôte le permet, il est recommandé d'allouer plus de mémoire RAM (8 ou 16GB) et vidéo (32MB ou plus) à la machine virtuelle, ainsi que d'ajuster certains paramètres selon l'hyperviseur utilisé.

### VirtualBox

1. Ouvrir les paramètres de la machine virtuelle

    Dans VirtualBox :
    - Faire un clic droit sur la machine virtuelle
    - Sélectionner **Settings...**

1. Ajuster le type de système d'exploitation
- Sous **General > Basic**, choisir **Debian** puis **Debian 13 (Trixie)**

    ![RAM](../assets/images/vbox-os.png)

1. Ajuster la mémoire vive (RAM)
- Sous **System > Motherboard**, ajuster la mémoire RAM

    ![RAM](../assets/images/vbox-memoire.png)
    
1. Ajuster le nombre de processeurs
    - Sous **System > Processor**, ajuster le nombre de CPUs virtuels si besoin

    ![CPU](../assets/images/vbox-cpu.png)

1. Ajuster les paramètres vidéo
    - Sous **Display > Screen**, ajuster la mémoire vidéo (32MB ou plus recommandé)

    ![Vidéo](../assets/images/vbox-display.png)

1. Installer les additions invités VirtualBox (*Guest Additions*)**

    1. Mettre à jour le système

        ```bash
        sudo apt update && sudo apt upgrade -y
        ```

    1. Installer les paquets nécessaires

        ```bash
        sudo apt install build-essential dkms linux-headers-$(uname -r) -y
        ```

    1. Insérer l’image ISO des Guest Additions

        Dans le menu de VirtualBox :

        - Aller dans **Périphériques > Insérer l’image CD des Additions Invité...**

        Cela montera un CD dans `/media/<utilisateur>/VBox_GAs_...`

    1. Exécuter le script d’installation

        ```bash
        sudo sh /media/$USER/VBox_GAs_*/VBoxLinuxAdditions.run
        ```

    1. Redémarrer la machine virtuelle

        ```bash
        sudo reboot
        ```

### VMware Workstation

1. Ouvrir les paramètres de la machine virtuelle

    Dans VMware Workstation :
    - Faire un clic droit sur la machine virtuelle
    - Sélectionner **Settings**

1. Ajuster la mémoire vive (RAM)

    - Sous **Hardware > Memory**, ajuster la mémoire RAM

    ![RAM](../assets/images/vmware-memoire.png)
    
1. Ajuster le nombre de processeurs

    - Sous **Hardware > Processors**, ajuster le nombre de CPUs virtuels comme désiré

    ![CPU](../assets/images/vmware-cpu.png)

1. Installer les outils VMware (*VMware Tools*)

    1. Mettre à jour le système

        ```bash
        sudo apt update && sudo apt upgrade -y
        ```

    1. Installer les paquets nécessaires

        ```bash
        sudo apt install open-vm-tools
        ```

    1. Redémarrer la machine virtuelle

        ```bash
        sudo reboot
        ```

## Liens utiles

[Téléchargement de l'environnement applicatif](https://cmaisonneuveqcca-my.sharepoint.com/:u:/r/personal/ophenix_cmaisonneuve_qc_ca/Documents/Cours/420-950-MA%20-%20Cybers%C3%A9curit%C3%A9/Mat%C3%A9riel%20de%20cours/420-950-MA-applicatif.ova?csf=1&web=1&e=9LSWNF)