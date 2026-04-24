---
layout: default
title: "Travail pratique 3"
nav_order: 3
published: true
---

# Travail pratique 3 : Chez Oups Technologies, nos serveurs sont durcis!

## Mise en situation

Vous poursuivez votre stage chez **Oups Technologies**. Après avoir sécurisé le transport, analysé les menaces possibles, puis renforcé le portail de réservation grâce à des analyses SAST/SCA/DAST (travail qui est toujours en cours d'ailleurs), les dirigeants réalisent qu’un aspect fondamental n'a toujours pas été validé : **la journalisation et le durcissement du serveur en production**. 

Décidément, plus les dirigeants en apprennent sur la sécurité et sur votre capacité à régler les problèmes, plus votre charge de travail tend à augmenter... 

Un incident récent a mis en évidence plusieurs lacunes :

- Aucune trace claire des actions critiques effectuées par les utilisateurs
- Impossible de distinguer les événements de sécurité des simples logs applicatifs
- Aucun mécanisme pour limiter ou bloquer les tentatives d’authentification malveillantes
- Un pare‑feu trop permissif pour un serveur exposé à Internet

Votre mandat : améliorer la **journalisation**, renforcer la **sécurité réseau** et déployer ces améliorations sur un serveur simulant la production afin d'en faire la démonstration.

---

## Objectifs pédagogiques

- Implémenter une **journalisation structurée** dans une application Python/Flask
- Distinguer les **logs applicatifs** des **logs d’audit / de sécurité**
- Configurer une **rotation des logs** côté serveur
- Déployer une application sur une VM simulant un serveur de production
- Appliquer le principe du **moindre privilège réseau** via un pare‑feu
- Mettre en place une défense active avec **fail2ban**

---

## Environnement fourni

Une machine virtuelle Debian minimaliste simulant un serveur de production vous est fournie. Cette VM inclut déjà :

- ***Nginx*** comme proxy inverse
- Le serveur applicatif ***Gunicorn*** configuré comme service `systemd`
- Une structure vide prête à recevoir l'application du portail dans `/var/www/portail`

Vous pouvez la télécharger ici : [Téléchargement de la machine virtuelle](https://cmaisonneuveqcca-my.sharepoint.com/:u:/r/personal/ophenix_cmaisonneuve_qc_ca/Documents/Cours/420-950-MA%20-%20Cybers%C3%A9curit%C3%A9/Mat%C3%A9riel%20de%20cours/420-950-MA-TP3.ova?csf=1&web=1&e=WOlja9)

{: .warning}
> Aucune modification de la configuration Nginx ou du service systemd n’est requise ni attendue, sauf indication contraire.

---

## Préparation

1. Importez la machine virtuelle fournie (.ova) dans **VirtualBox ou VMware** (téléchargez [ici](https://cmaisonneuveqcca-my.sharepoint.com/:u:/r/personal/ophenix_cmaisonneuve_qc_ca/Documents/Cours/420-950-MA%20-%20Cybers%C3%A9curit%C3%A9/Mat%C3%A9riel%20de%20cours/420-950-MA-TP3.ova?csf=1&web=1&e=WOlja9))
1. Démarrez la VM et connectez‑vous avec les identifiants suivants :
    - Utilisateur : `employe`
    - Mot de passe : `employe` 
1. Récupérez le code de départ du portail de réservation ici : [Code portail Oups Technologies - TP 3](https://github.com/ophenix-420-950-ma-24636/tp3)
1. Copiez le code de l'application sous `/var/www/portail` de façon à obtenir la structure suivante :
    ```
/var/www/portail
├─ venv (déjà présent)
├─ portail (répertoire parent contenant votre code)
    ├─ (vos fichiers de code)
├─ requirements.txt

    ```
1. Lancer les commandes suivantes 
    ```bash
    cd /var/www/portail
    source venv/bin/activate
    pip install -r requirements.txt
    sudo systemctl restart portail
    ```
1. Vérifiez que l’application fonctionne via HTTP avant toute modification
    - Le serveur est disponible à `http://<ip de votre VM>`
    - Par souci de simplicité, comme il s'agit d'un test, le serveur utilise HTTP et non HTTPS. Vous n'avez pas à changer cela (vous l'avez déjà fait dans le TP1, une fois c'est assez 😄)

{: .astuce}
> Pour démarrer/arrêter/redémarrer le serveur, vous pouvez utiliser les commandes suivantes:
> - `sudo systemctl start portail`
> - `sudo systemctl stop portail`
> - `sudo systemctl restart portail`

---

## Tâches à réaliser

### 1. Amélioration du système de journalisation

Modifiez le code du portail de réservation afin d’y améliorer le système de **logging** basé sur le module standard `logging` de Python.

Vous devez :

- Ajouter (ou modifier) un **logger applicatif**
- Ajouter (ou modifier) un **logger d’audit** distinct
- Vous assurer que les deux types de logs sont clairement séparés (par exemple écrits dans des fichiers séparés)

Les événements suivants doivent au minimum être journalisés :

- Démarrage et arrêt de l’application
- Tentatives d’authentification (succès et échecs)
- Création, modification et suppression de réservations

**Question(s)**

- Quels critères avez‑vous utilisés pour décider qu’un événement relève d’un log applicatif ou d’un log d’audit ?

---

### 2. Configuration de la rotation des logs

Modifier l'application du portail de réservation :

- Configurez une **rotation automatique des fichiers de logs**
- Limitez la taille maximale des fichiers ou l'âge des fichiers
    - Vous pouvez donc choisir une rotation basée sur la **taille** des fichiers ou sur la **date/heure**
- Conservez un nombre raisonnable d’archives

**Question(s)**

- Pourquoi la rotation des logs est‑elle critique, surtout sur un serveur exposé à Internet ?
- Pourquoi avez-vous choisi le critère de rotation choisi (taille ou date/heure) ?
- Pourquoi avez-vous choisi le nombre de fichiers archivés choisi ?

---

### 3. Déploiement sur la VM de production simulée

Déployez votre nouvelle version du portail sur la VM fournie :

- Copiez le code modifié aux étapes précédentes sous `/var/www/portail`
- Assurez-vous que toutes les dépendances sont bien installées
    ```bash
    source venv/bin/activate
    pip install -r requirements.txt
    ```

- Redémarrez le portail
    ```bash
    sudo systemctl restart portail
    ```

Démontrez que :

- Les nouveaux logs sont générés sur le serveur
- L’application fonctionne correctement après le déploiement

---

### 4. Configuration du pare‑feu

Renforcez la sécurité réseau du serveur en configurant un pare‑feu.

Vous pouvez utiliser l'un ou l'autre des deux outils suivants :

- `ufw`
- `iptables`

Le pare‑feu doit :

- Autoriser **uniquement** : 
    - SSH
    - HTTP
    - HTTPS
- Bloquer tout autre trafic entrant

**Question(s)**

- Pourquoi est‑il déconseillé de laisser tous les ports ouverts sur un serveur applicatif ?
- En quoi un pare-feu permet-il d'améliorer la sécurité du serveur ?

---

### 5. Mise en place de fail2ban

Configurez **fail2ban** sur la VM afin de créer une cellule (*jail*) respectant les critères suivants :

- Protection du portail de réservation
- Bannissement d’une adresse IP après : 
    - **3 tentatives d’authentification échouées**
    - dans une **fenêtre de 5 minutes**
- Choisissez une durée de bannissement raisonnable

Vous devez :

- Créer un filtre permettant de détecter les échecs dans les logs d'audit du portail de réservation
- Créer une cellule (*jail*) utilisant votre filtre 
- Démontrer que le bannissement fonctionne

**Question(s)**

- Pourquoi fail2ban est‑il considéré comme une mesure de défense réactive ?
- Quel(s) critère(s) avez-vous utilisés pour choisir la durée du bannissement ?

---

### Évaluation

|Critère|Points|
|---|---|
|Section 1 : Journalisation|25|
|Section 2 : Rotation des logs|15|
|Section 3 : Déploiement|10|
|Section 4 : Pare‑feu|20|
|Section 5 : fail2ban|30|
|**Total**|**100 points (15 % de la note finale)**|

---

### À remettre

Un fichier **.zip** contenant :

- Le **code modifié** du portail de réservation
- Un document (PDF, Word, LibreOffice ou Markdown) contenant : 
    - Les réponses aux questions des sections 1 à 5
    - Les commandes utilisées pour la configuration du pare-feu
    - Pour chaque section, des captures d’écran démontrant le bon fonctionnement des mécanismes demandés
- Les fichiers de configuration : 
    - fail2ban (filtre et *jail*)
    - tout autre fichier de configuration utilisé ou modifié (logging, etc)

- Le travail est à remettre sur **Léa (Omnivox)** dans la section **Travaux - Énoncés et remises**
- Le travail peut être fait en équipes de **deux ou trois personnes** (une seule remise pour l’équipe)
- Le travail est à remettre au plus tard le **11 mai 2026** en fin de journée.

---

Encore une fois, l'avenir de Oups Technologies est entre vos mains (vous devriez vraiment demander une augmentation à votre prochaine évaluation de rendement)!
