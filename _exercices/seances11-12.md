---
layout: default
title: "Mission : GHOST BEACON - Phase 2 
nav_order: 5
has_toc: false
published: true
---

# Exercice : Mission GHOST BEACON - Phase 2

## Durcissement d'un serveur (*Hardening*)

## Mise en situation

L’application **GhostBeacon** a maintenant été vérifiée en profondeur à l’aide d’outils SAST et SCA. Plusieurs vulnérabilités ont été identifiées et corrigées, d’autres ont été documentées et acceptées temporairement.

Cependant, dans un contexte opérationnel réel, **la sécurité ne repose jamais uniquement sur le code**. Les mécanismes de journalisation, la configuration du système, les contrôles réseau et les contre‑mesures automatiques jouent un rôle tout aussi critique.

CyberMax vous confie donc une nouvelle mission : **durcir GhostBeacon et son environnement d’exécution**, afin de réduire l’impact d’attaques potentielles et d’améliorer la capacité de détection et de réaction face aux incidents.

---

## 1. Configuration du pare-feu

### Objectif

Réduire la surface d’attaque exposée par le serveur en choisissant exclusivement les ports et/ou les services autorisés à communiquer sur le réseau.

### 1.1 Déterminer les ports à autoriser
Sachant que vous désirez permettre les services suivants...
   - **HTTP** : pour permettre l'accès en HTTP pendant les phases de tests (sera désactivé ensuite) 
   - **HTTPS** : pour permettre l'accès sécurisé à GhostBeacon
   - **SSH** : pour permettre l'administration du serveur

... déterminez la liste des ports que vous devrez autoriser sur le pare-feu.

### 1.2 Configuration de UFW
1. Supprimez les règles iptables de l'une des façons suivantes :
   - Redémarrer la machine
   - En exécutant la commande `sudo iptables --flush`

1. Installez UFW (s'il n'est pas déjà installé)
   ```bash
   sudo apt install ufw
   ```
1. Modifiez la politique par défaut afin de :
   - Permettre le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

### Questions de réflexion

- Quelle méthode (iptables ou UFW) était la plus intuitive ?
- Pourquoi le filtrage réseau reste‑t‑il essentiel même pour une application sécurisée ?


### 1.4 BONUS : Utilisation de iptables
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

1. Qu'est-ce qu'un `Appender` ?
1. Quelle est l'intention derrière les trois `Appender` déclarés dans le fichier ?
1. En ce moment, existe-t-il une différence entre les `Appender` *AuditFile* et *AppFile* ?
   - Si oui, quelle est-elle ?
   - Sinon, que manque-t-il ?

### 2.2 Modification de la configuration de ***Log4j***

1. Activez la rotation des fichiers de logs 
   - limiter la taille maximale des fichiers
   - conserver un nombre fini d’archives
1. Vérifiez que la rotation fonctionne en générant des événements applicatifs

### Questions de réflexion

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
   - normaliser les messages (structure, vocabulaire, niveau)
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

1. Testez le fonctionnement du blocage en effectuant 3 connexions SSH infructueuses à votre VM

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

### 4.4 Création d'un fichier de blocage 
Créez maintenant un fichier de blocage (*jail*) utilisant votre filtre. Ce fichier permettra de paramétrer le blocage effectué lorsqu'une attaque est détectée.

1. Créez un nouveau fichier de blocage sous `/etc/fail2ban/jail.d/ghostbeacon.local`
1. Complétez la structure suivante afin de configurer correctement votre blocage
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

### Questions

- Quels événements applicatifs se prêtent bien à une protection par fail2ban ?
- Pourquoi ce type de mécanisme ne remplace‑t‑il pas une authentification robuste ?

---

## Réflexion finale

Expliquez brièvement :

- En quoi le hardening complète les activités SAST/DAST
- Quels mécanismes sont les plus critiques en environnement réel
- Quels compromis doivent parfois être faits entre sécurité et opération

