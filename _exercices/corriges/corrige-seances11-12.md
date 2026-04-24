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
    ***On pourrait utiliser la balise `<RollingFile>` au lieu de la balise `<File>` dans chacun des `<Appender>`. Cette balise configure la rotation des logs et permet d'appliquer des politiques basées sur le temps ou la taille des fichiers.***
1. Quelle serait une période raisonnable pour effectuer une rotation ?
    ***Il n'existe pas de réponse unique, mais une journée (qui est la valeur par défaut) est une valeur très raisonnable.***
1. Quelle serait une taille raisonnable pour effectuer une rotation ?
    ***Encore une fois, cela dépend grandement de la quantité de logs. Pour un petit volume comme celui de notre cours, une valeur relativement faible comme 1MB ou 5MB ferait bien l'affaire.***
1. Combien de fichiers archivés pourrait-on garder ?
    ***Plus les fichiers sont petits, moins il est coûteux d'en garder beaucoup. Ainsi, on pourrait facilement ici en garder 30, surtout pour les logs d'audit (et même plus)***

### 2.2 Modification de la configuration de ***Log4j***

1. Activez la rotation des fichiers de logs 
   - limiter la taille maximale des fichiers, ou l'âge des fichiers, ou les deux en fonction de vos réponses à la question précédente
   - conserver un nombre fini d’archives en fonction de votre réponse à la question précédente
1. Vérifiez que la rotation fonctionne en générant des événements applicatifs

***Voir le code dans la branche `solution` du dépôt Git***

### 2.3 Questions de réflexion

- Pourquoi la rotation des logs est‑elle un mécanisme de sécurité ?
    ***Parce qu'elle permet de garantir:***
    - ***Que les logs ne causent pas un déni de service (volontaire ou non) en remplissant l'unité de stockage qui les héberge***
    - ***Que l'on garde suffisamment de logs archivés pour être en mesure de mener une enquête post-incident (au besoin)***
- Quels risques apparaissent lorsque les logs ne sont pas maîtrisés ?
    - ***Déni de service par remplissage***
    - ***Loi de Murphy: c'est exactement au moment où l'on aurait eu besoin de nos logs qu'ils ont été supprimés...***

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
    ***Un Appender est une sortie d'un logger; le logger envoie le message à logger dans un ou plusieurs appenders. Chaque appender a une configuration qui lui est propre, permettant de définir le format des logs, des filtres, etc***
1. Quelle est l'intention derrière les trois `Appender` déclarés dans le fichier ?
    - ***Console : écrit tous les logs à la console***
    - ***AppFile : destination pour les logs applicatifs***
    - ***AuditFile : destination pour les logs d'audit***


### 3.2 Séparation des logs applicatifs et des logs d'audit
Modifiez le fichier de configuration `src/resources/log4j2-spring.xml` de façon à permettre d'écrire les logs d'audit et les logs de sécurité dans deux fichiers différents.

1. En ce moment, existe-t-il une différence entre les `Appender` *AuditFile* et *AppFile* ? ***Non***
   - Si oui, quelle est-elle ?
   - Sinon, que manque-t-il ?
        ***Il manque un mécanisme de filtrage qui permettrait de différencier le contenu écrit dans AuditFile de celui qui est écrit dans AppFile.***
1. Effectuez les modifications requises dans les `Appender` afin que les logs d'audit utilisent *AuditFile* et que les logs applicatifs utilisent *AppFile*.

    ***Voir le code dans la branche `solution` du dépôt Git. Les logs ayant le marqueur AUDIT sont ceux qui seront imprimés dans le log d'audit.***

{: .astuce}
> Consultez les notes de cours, en particulier l'utilisation de `<Filter>` jumelée avec les `Marker` dans le code...

### 3.3 Analyse des logs

1. Analysez les logs actuellement produits par GhostBeacon
1. Catégorisez les logs selon les deux catégories suivantes : 
   - logs applicatifs / fonctionnels / (ex.: traitement, événements applicatifs, etc)
   - logs d'audit / sécurité (ex.: accès réseau, accès privilégiés, erreurs de sécurité, etc)

   ***Voir le code dans la branche `solution` du dépôt Git. Les logs ayant le marqueur AUDIT sont ceux qui seront imprimés dans le log d'audit.***

### 3.4 Amélioration des logs
1. Modifiez le code de GhostBeacon pour :
   - normaliser les messages (structure, vocabulaire, niveau) au besoin
   - envoyer les logs dans le bon fichier selon la catégorie (audit vs application)

    ***Voir le code dans la branche `solution` du dépôt Git. Les logs ayant le marqueur AUDIT sont ceux qui seront imprimés dans le log d'audit.***


### 3.5 Questions de réflexion
1. Pourquoi est‑il risqué de mélanger logs applicatifs et logs d’audit ?
    ***Parce qu'on donne généralement accès aux logs applicatifs à beaucoup plus d'utilisateurs qu'aux logs d'audit. Comme les logs d'audit sont très importants d'un point de vue réglementaire / sécurité / conformité, on doit tout faire pour protéger leur intégrité et ainsi éviter qu'ils soient falsifiés. Les séparer des logs applicatifs est donc fondamental pour ensuite pouvoir attribuer des permissions différentes.***
1. Quels événements devraient systématiquement apparaître dans les logs d’audit ?
    ***Tous les événements pouvant avoir un impact sur la sécurité et/ou étant nécessaires lors d'une éventuelle enquête post-incident: tentatives de connexion (succès ou échec), authentification (succès ou échec), accès à des ressources sensibles, élévations de privilèges (autorisées ou non), changements de mot de passe, etc.***

---

## 4. Protection automatique avec fail2ban

### Objectif

Bloquer automatiquement des comportements suspects avant qu’ils ne dégénèrent.

### 4.1 Installation de fail2ban
Installez l'outil `fail2ban` sur l'environnement, ou vérifiez qu'il est déjà installé
1. Quelle commande avez-vous utilisée ?
   ```bash
   sudo apt install fail2ban
   ```

### 4.2 Blocage des tentatives de connexion SSH échouées
1. Configurez `fail2ban` de manière à bloquer automatiquement...
   - les tentatives de connexion SSH
   - ayant eu un échec de connexion
   - au moins 3 fois
   - dans un intervalle de 5 minutes
   - pour une durée de 1 minute

   ```ini
   [sshd]
   enabled=true
   port=22
   logpath=/var/log/auth.log
   maxretry=3
   findtime=300
   bantime=60
   ```



1. Testez le fonctionnement du bannissement en effectuant 3 connexions SSH infructueuses à votre VM

   ***Après 3 tentatives de connexion, vous devriez être incapables de vous connecter en SSH. Vous devriez aussi voir une sortie similaire à :***
   ```bash
   Status for the jail: sshd
   |- Filter
   |  |- Currently failed:	0
   |  |- Total failed:	6
   |  `- Journal matches:	_SYSTEMD_UNIT=ssh.service + _COMM=sshd + _COMM=sshd-session
   `- Actions
      |- Currently banned:	1
      |- Total banned:	2
      `- Banned IP list:	172.16.116.135
   ```

### 4.3 Questions de réflexion

- Quels types d’attaques `fail2ban` permet‑il de contrer efficacement ?
   ***Fail2ban se spécialise dans la détection d'attaques de type "force brute". Cependant, il peut être efficace pour n'importe quelle autre attaque étant clairement visible à partir des logs.***
- Quelles sont les limites de ce type de protection ?
   ***L'attaquant doit essayer d'exploiter la vulnérabilité un certain nombre de fois avant que la protection ne soit déclenchée.***

---

## 5. Blocage spécifique pour GhostBeacon

### Objectif

Mettre en place une défense spécifique à la logique de l'application.

### 4.1 Identification du code vulnérable au *Path Traversal*
Dans le code de GhostBeacon, identifiez l'endroit où l'application contient une vulnérabilité de type *Path Traversal*
1. Où se situe cette vulnérabilité ?
   ***Elle se situe dans la classe `StatusService`, car on ne valide pas que le chemin passé ne tente pas de sortir du répertoire racine de l'application.***
1. Comment peut-elle être exploitée ?
   ***Un attaquant peut tenter de passer un chemin de type `../` pour reculer dans l'arborescence de fichiers et afficher le contenu de fichiers non-reliés à ghostbeacon.***

### 4.2 Identification des logs applicables
Identifiez le ou les logs qui seront produits si un attaquant tente d'exploiter la vulnérabilité de *Path Traversal*

1. Les logs identifiés contiennent-ils une information permettant de détecter l'attaque ?
   ***Le log d'audit permettra de voir que le `StatusController` a reçu une requête contenant `../` (ou l'une de ses variantes) ***
1. Quelle expression régulière pourrait permettre de générer un échec et d'extraire l'IP ou le nom d'hôte de provenance de l'attaque ?
   ***On pourrait utiliser une regex de ce type :***
   ```
   failregex=^.*\| ip=<HOST> \| code=(?:\.\./)+.*$
   ```

### 4.3 Création d'un filtre
À partir de vos réponses aux questions précédentes, écrivez un filtre *fai2ban* permettant de détecter les tentatives d'exploitation de la vulnérabilité.

1. Créez un nouveau fichier de filtre sous `/etc/fail2ban/filter.d/ghostbeacon.conf`
1. Complétez la structure suivante afin de configurer correctement votre filtre
   ```
   [Definition]
   failregex = failregex=^.*\| ip=<HOST> \| code=(?:\.\./)+.*$
   ```
1. Testez votre filtre
   ```bash
   fail2ban-regex <fichier de log de ghostbeacon contenant l'attaque> /etc/fail2ban/filter.d/ghostbeacon.conf
   ```
   ```bash
      Running tests
   =============

   Use      filter file : ghostbeacon, basedir: /etc/fail2ban
   Use         log file : test.log
   Use         encoding : UTF-8


   Results
   =======

   Failregex: 1 total
   |-  #) [# of hits] regular expression
   |   1) [1] ^.*\| ip=<HOST> \| code=(?:\.\./)+.*$
   `-

   Ignoreregex: 0 total

   Date template hits:
   |- [# of hits] date format
   |  [1] {^LN-BEG}ExYear(?P<_sep>[-/.])Month(?P=_sep)Day(?:T|  ?)24hour:Minute:Second(?:[.,]Microseconds)?(?:\s*Zone offset)?
   `-

   Lines: 1 lines, 0 ignored, 1 matched, 0 missed
   [processed in 0.00 sec]
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
   ***De manière générale, les événements de connexion, d'accès aux ressources et d'authentification sont d'excellents candidats pour fail2ban.***
- Pourquoi ce type de mécanisme ne remplace‑t‑il pas une authentification robuste ?
   ***Parce que si l'attaquant réussit à déjouer le mécanisme d'authentification, l'échec ne sera pas visible dans les logs (autrement dit, il n'y aura pas d'échec). Ainsi, du point de vue de fail2ban, l'attaque passera complètement inaperçue.***

---

## 5. Réflexion finale

Expliquez brièvement :

- En quoi le hardening complète les activités SAST/DAST
- Quels mécanismes sont les plus critiques en environnement réel
- Quels compromis doivent parfois être faits entre sécurité et opération

