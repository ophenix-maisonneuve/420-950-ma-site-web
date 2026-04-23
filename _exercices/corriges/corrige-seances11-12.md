---
layout: default
parent: "Mission : GHOST BEACON - Phase 2"
title: "Corrigé - Mission : GHOST BEACON - Phase 2"
nav_order: 1
has_toc: false
published: true
---

# Exercice : Mission GHOST BEACON - Phase 2

## 1. Configuration du pare-feu

### Objectif

Réduire la surface d’attaque exposée par le serveur en choisissant exclusivement les ports et/ou les services autorisés à communiquer sur le réseau.

### 1.1 Déterminer les ports à autoriser
Sachant que vous désirez permettre les services suivants...
   - **HTTP** : pour permettre l'accès en HTTP pendant les phases de tests (sera désactivé ensuite) 
   - **HTTPS** : pour permettre l'accès sécurisé à GhostBeacon
   - **SSH** : pour permettre l'administration du serveur

... déterminez la liste des ports que vous devrez autoriser sur le pare-feu.

***Il faudra ouvrir les ports 80, 443 et 22. Ceci étant dit, UFW prend aussi en charge les services par leur nom (http, https, ssh).***

### 1.2 Configuration de UFW

1. Installez UFW (s'il n'est pas déjà installé)
   ```bash
   sudo apt install ufw
   ```
1. Modifiez la politique par défaut afin de :
   - Permettre le trafic sortant
   - Bloquer le trafic entrant

   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   ```

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.

    ```bash
    sudo ufw allow [in] http
    sudo ufw allow [in] https
    sudo ufw allow [in] ssh
    ```

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

   ***Normalement, vous verrez que vous avez encore accès. Cependant, si vous enlevez l'une des règles que vous avez ajoutées, votre accès sera perdu pour le service enlevé.***

### 1.3 Questions de réflexion

- Quelle méthode (iptables ou UFW) était la plus intuitive ?
    ***UFW est normalement beaucoup plus simple et intuitif à utiliser que iptables***
- Pourquoi le filtrage réseau reste‑t‑il essentiel même pour une application sécurisée ?
    ***Parce que le pare-feu permet de sécuriser le réseau avant même que les paquets n'atteignent l'application. Ainsi, il agit comme une couche de défense complémentaire qui ne remplace pas (ni n'est remplacé par) une sécurisation de l'application.***

    ***Autrement dit, le pare-feu devra nécessairement permettre la communication avec votre application. À partir de ce moment, ce sera à l'application de bien gérer la sécurité.***



### 1.4 BONUS : Utilisation de iptables
1. Désactivez d'abord UFW
   ```bash
   sudo ufw disable
   ```

1. Modifiez la politique par défaut afin de :
   - Permettre le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

1. Pour plus de sécurité, modifiez maintenant la politique par défaut afin de :
   - Bloquer le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.
   - ***Indice**: il faudra utiliser les options `-m state --state ESTABLISHED,RELATED`

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

---

## 2. Rotation et gestion des logs

### Objectif

Éviter la saturation du disque et conserver un historique utilisable.

### 2.1 Analyse du fichier de configuration ***Log4j***

Inspectez le fichier de configuration `src/resources/log4j2-spring.xml`.

1. En vous basant sur les notes de cours, à quel endroit pourriez-vous ajouter des paramètres pour activer la rotation des *logs* ?
1. Quelle serait une période raisonnable pour effectuer une rotation ?
1. Quelle serait une taille raisonnable pour effectuer une rotation ?
1. Combien de fichiers archivés pourrait-on garder ?

### 2.2 Modification de la configuration de ***Log4j***

1. Activez la rotation des fichiers de logs 
   - limiter la taille maximale des fichiers, ou l'âge des fichiers, ou les deux en fonction de vos réponses à la question précédente
   - conserver un nombre fini d’archives en fonction de
1. Vérifiez que la rotation fonctionne en générant des événements applicatifs

### 2.3 Questions de réflexion

- Pourquoi la rotation des logs est‑elle un mécanisme de sécurité ?
- Quels risques apparaissent lorsque les logs ne sont pas maîtrisés ?

---

## 3. Amélioration de la journalisation

### Objectif

Améliorer la qualité et l’utilité des logs afin de permettre :
- l’audit de sécurité
- la détection de tentatives d'intrusion
- l’intégration éventuelle avec des systèmes de détection et/ou de surveillance

### 3.1 Analyse du fichier de configuration ***Log4j***

Inspectez le fichier de configuration `src/resources/log4j2-spring.xml`.

1. Qu'est-ce qu'un `Appender` ?
1. Quelle est l'intention derrière les trois `Appender` déclarés dans le fichier ?


### 3.2 Séparation des logs applicatifs et des logs d'audit
Modifiez le fichier de configuration `src/resources/log4j2-spring.xml` de façon à permettre d'écrire les logs d'audit et les logs de sécurité dans deux fichiers différents.

1. En ce moment, existe-t-il une différence entre les `Appender` *AuditFile* et *AppFile* ?
   - Si oui, quelle est-elle ?
   - Sinon, que manque-t-il ?
1. Effectuez les modifications requises dans les `Appender` afin que les logs d'audit utilisent *AuditFile* et que les logs applicatifs utilisent *AppFile*.

{: .astuce}
> Consultez les notes de cours, en particulier l'utilisation de `<Filter>` jumelée avec les `Marker` dans le code...

### 3.3 Analyse des logs

1. Analysez les logs actuellement produits par GhostBeacon
1. Catégorisez les logs selon les deux catégories suivantes : 
   - logs applicatifs / fonctionnels / (ex.: traitement, événements applicatifs, etc)
   - logs d'audit / sécurité (ex.: accès réseau, accès privilégiés, erreurs de sécurité, etc)

### 3.4 Amélioration des logs
1. Modifiez le code de GhostBeacon pour :
   - normaliser les messages (structure, vocabulaire, niveau) au besoin
   - envoyer les logs dans le bon fichier selon la catégorie (audit vs application)


### 3.5 Questions de réflexion
1. Pourquoi est‑il risqué de mélanger logs applicatifs et logs d’audit ?
1. Quels événements devraient systématiquement apparaître dans les logs d’audit ?

---

## 4. Protection automatique avec fail2ban

### Objectif

Bloquer automatiquement des comportements suspects avant qu’ils ne dégénèrent.

### 4.1 Installation de fail2ban
Installez l'outil `fail2ban` sur l'environnement, ou vérifiez qu'il est déjà installé
1. Quelle commande avez-vous utilisée ?

### 4.2 Blocage des tentatives de connexion SSH échouées
1. Configurez `fail2ban` de manière à bloquer automatiquement...
   - les tentatives de connexion SSH
   - ayant eu un échec de connexion
   - au moins 3 fois
   - dans un intervalle de 5 minutes
   - pour une durée de 1 minute

1. Testez le fonctionnement du bannissement en effectuant 3 connexions SSH infructueuses à votre VM

### 4.3 Questions de réflexion

- Quels types d’attaques `fail2ban` permet‑il de contrer efficacement ?
- Quelles sont les limites de ce type de protection ?

---

## 5. Blocage spécifique pour GhostBeacon

### Objectif

Mettre en place une défense spécifique à la logique de l'application.

### 4.1 Identification du code vulnérable au *Path Traversal*
Dans le code de GhostBeacon, identifiez l'endroit où l'application contient une vulnérabilité de type *Path Traversal*
1. Où se situe cette vulnérabilité ?
1. Comment peut-elle être exploitée ?

### 4.2 Identification des logs applicables
Identifiez le ou les logs qui seront produits si un attaquant tente d'exploiter la vulnérabilité de *Path Traversal*

1. Les logs identifiés contiennent-ils une information permettant de détecter l'attaque ?
1. Quelle expression régulière pourrait permettre de générer un échec et d'extraire l'IP ou le nom d'hôte de provenance de l'attaque ?

### 4.3 Création d'un filtre
À partir de vos réponses aux questions précédentes, écrivez un filtre *fai2ban* permettant de détecter les tentatives d'exploitation de la vulnérabilité.

1. Créez un nouveau fichier de filtre sous `/etc/fail2ban/filter.d/ghostbeacon.conf`
1. Complétez la structure suivante afin de configurer correctement votre filtre
   ```
   [Definition]
   failregex = <regex identifiée à la question précédente>
   ```
1. Testez votre filtre
   ```bash
   fail2ban-regex <fichier de log de ghostbeacon contenant l'attaque> /etc/fail2ban/filter.d/ghostbeacon.conf
   ```

### 4.4 Création d'une cellule personnalisée 
Créez maintenant un fichier de cellule (*jail*) utilisant votre filtre. Ce fichier permettra de paramétrer le bannissement effectué lorsqu'une attaque est détectée.

1. Créez un nouveau fichier de cellule sous `/etc/fail2ban/jail.d/ghostbeacon.local`
1. Complétez la structure suivante afin de configurer correctement votre bannissement
   ```
   [ghostbeacon]
   enabled = true
   filter = ghostbeacon
   logpath = <fichier de log d'audit de ghostbeacon>
   maxretry = <nombre maximal d'essais permis avant de bloquer>
   findtime = <intervalle>
   bantime = <durée du blocage>
   ```
1. Rechargez la configuration de fail2ban
   ```bash
   sudo systemctl restart fail2ban
   ```

### 4.5 Questions de réflexion

- Quels événements applicatifs se prêtent bien à une protection par fail2ban ?
- Pourquoi ce type de mécanisme ne remplace‑t‑il pas une authentification robuste ?

---

## 5. Réflexion finale

Expliquez brièvement :

- En quoi le hardening complète les activités SAST/DAST
- Quels mécanismes sont les plus critiques en environnement réel
- Quels compromis doivent parfois être faits entre sécurité et opération

