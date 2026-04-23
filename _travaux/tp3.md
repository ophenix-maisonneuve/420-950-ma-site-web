---
layout: default
title: "Travail pratique 3"
nav_order: 3
published: true
---

# Travail pratique 3 : Chez Oups Technologies, nos serveurs sont durcis!

## Mise en situation

Vous poursuivez votre stage chez **Oups Technologies**. Après avoir sécurisé le transport, analysé les menaces possibles, puis renforcé le portail de réservation grâce à des analyses SAST/SCA/DAST (travail qui est toujours en cours d'ailleurs), les dirigeants réalisent qu’un aspect fondamental n'a toujours pas été validé : **la journalisation et le durcissement du serveur en production**. 

Décidément, plus les dirigeants en apprennent sur la sécurité, plus votre charge de travail tend à augmenter... 

Un incident récent a mis en évidence plusieurs lacunes :

- Aucune trace claire des actions critiques effectuées par les utilisateurs
- Impossible de distinguer les événements de sécurité des simples logs applicatifs
- Aucun mécanisme pour limiter ou bloquer les tentatives d’authentification malveillantes
- Un pare‑feu trop permissif pour un serveur exposé à Internet

Votre mandat : améliorer la **journalisation**, renforcer la **sécurité réseau** et déployer ces améliorations sur un serveur simulant la production.

---

## Objectifs pédagogiques

- Implémenter une **journalisation structurée** dans une application Flask
- Distinguer les **logs applicatifs** des **logs d’audit / de sécurité**
- Configurer une **rotation des logs** côté serveur
- Déployer une application Flask sur une VM de type production
- Appliquer le principe du **moindre privilège réseau** via un pare‑feu
- Mettre en place une défense active avec **fail2ban**

---

## Environnement fourni

Une machine virtuelle Debian minimaliste simulant un serveur de production vous est fournie. Cette VM inclut **déjà** :

- ***Nginx*** comme proxy inverse
- Le serveur applicatif ***Gunicorn*** configuré comme service `systemd`
- Une structure d’application Flask standardisée dans `/var/www/portail`

{: .warning}
> **Aucune modification de la configuration Nginx ou du service systemd n’est requise ni attendue**, sauf indication contraire.

---

## Préparation

1. Importez la machine virtuelle fournie (.ova) dans **VirtualBox ou VMware**
1. Démarrez la VM et connectez‑vous avec l’utilisateur `student`
1. Récupérez le code de départ du portail de réservation ici : [Code portail Oups Technologies - TP 3](https://github.com/ophenix-420-950-ma-24636/tp3)
1. Copiez le code de l'application sous `/var/www/portail`
1. Lancer les commandes suivantes 
    ```bash
    source venv/bin/activate
    pip install -r requirements.txt
    sudo systemctl restart portail
    ```
1. Vérifiez que l’application fonctionne via HTTP ou HTTPS avant toute modification

{: .astuce}
> Pour démarrer/arrêter/redémarrer le serveur, vous pouvez utiliser les commandes suivantes:
> - `sudo systemctl start portail`
> - `sudo systemctl stop portail`
> - `sudo systemctl restart portail`

---

## Tâches à réaliser

### 1. Ajout d’un système de journalisation

Modifiez le code du portail de réservation afin d’y intégrer un système de **logging** basé sur le module standard `logging` de Python.

Vous devez :

- Ajouter un **logger applicatif**
- Ajouter un **logger d’audit / de sécurité** distinct
- Vous assurer que les deux types de logs sont clairement séparés

Les événements suivants doivent au minimum être journalisés :

- Démarrage et arrêt de l’application
- Tentatives d’authentification (succès et échecs)
- Création, modification et suppression de réservations

**Questions**

- Quels critères avez‑vous utilisés pour décider qu’un événement relève d’un log applicatif ou d’un log d’audit ?

---

### 2. Configuration de la rotation des logs

Modifier l'application du portail de réservation :

- Configurez une **rotation automatique des fichiers de logs**
- Limitez la taille maximale des fichiers
- Conservez un nombre raisonnable d’archives

**Questions**

- Pourquoi la rotation des logs est‑elle critique, surtout sur un serveur exposé à Internet ?

---

### 3. Déploiement sur la VM de production simulée

Déployez votre nouvelle version du portail sur la VM fournie :

- Copie du code applicatif
- Installation des dépendances
- Redémarrage du service Flask

Vous devez démontrer que :

- Les nouveaux logs sont générés sur le serveur
- L’application fonctionne toujours correctement après déploiement

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

**Questions**

- Pourquoi est‑il déconseillé de laisser tous les ports ouverts sur un serveur applicatif ?

---

### 5. Mise en place de fail2ban

Configurez **fail2ban** sur la VM afin de créer un blocage (*jail*) respectant les critères suivants :

- Protection du portail de réservation
- Bannissement d’une adresse IP après : 
    - **3 tentatives d’authentification échouées**
    - dans une **fenêtre de 5 minutes**

Vous devez :

- Créer un filtre permettant de détecter les échecs dans les logs d'audit du portail de réservation
- Créer un blocage (*jail*) utilisant votre filtre 
- Démontrer que le bannissement fonctionne

**Questions**

- Pourquoi fail2ban est‑il considéré comme une mesure de défense réactive ?

---

### Évaluation

|Critère|Points|
|---|---|
|Section 1 : Journalisation|25|
|Section 2 : Rotation des logs|15|
|Section 3 : Déploiement|10|
|Section 4 : Pare‑feu|25|
|Section 5 : fail2ban|25|
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

Encore une fois, l'avenir de Oups Technologies est entre vos mains! 🔐🔥
