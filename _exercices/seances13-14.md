---
layout: default
title: "Mission : GHOST BEACON – Phase 3"
nav_order: 6
has_toc: true
published: true
---

## Exercice : Mission GHOST BEACON – Phase 3  
### Analyse dynamique de sécurité (DAST)

---

## Mise en situation

Malgré les correctifs appliqués lors de la **phase 1** (SAST / SCA) et le durcissement du serveur effectué dans la **phase 2** (configuration réseau, journalisation, protections automatiques), la direction de CyberMax demeure prudente.

L’expérience a démontré qu’un code qui *passe le build* peut néanmoins présenter des failles exploitables une fois l’application déployée et accessible sur le réseau. Avant toute utilisation opérationnelle, **GhostBeacon** doit maintenant être analysée du point de vue d’un attaquant externe, sans accès au code source ni connaissance interne de son implémentation.

Cette approche, appelée **analyse dynamique de sécurité** (*DAST*), vise à évaluer le comportement réel de l’application en fonctionnement.

Votre mandat consiste à mener cette analyse à l’aide de **OWASP ZAP**, outil de référence pour les tests de sécurité dynamiques sur les applications web.

---

## Objectifs

- Comprendre le rôle, la portée et les **limites du DAST**
- Utiliser **OWASP ZAP** comme proxy d’analyse dynamique
- Réaliser une analyse **passive** et **active**
- Utiliser le **Spider** pour cartographier la surface d’attaque
- Effectuer du **fuzzing ciblé**
- Corréler les résultats **DAST** avec ceux du **SAST/SCA**
- Évaluer le **risque réel** des vulnérabilités détectées

{: .warning}
> La version actuelle de GhostBeacon ne comporte aucun mécanisme d’authentification. L’analyse DAST sera donc réalisée **exclusivement en tant qu’utilisateur non authentifié**. Il s'agit d'une décison volontaire afin d'apprendre à utiliser l'outil ZAP avant d'ajouter la complexité liée à l'authentification.

---

## Préparation

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-950-ma-24636/exercice-seances-13-14.git
```
ou
```bash
git clone https://github.com/ophenix-420-950-ma-24636/exercice-seances-13-14.git
```

### 2. Lancez le projet Java

Dans l'environnement applicatif, lancez l'application GhostBeacon comme nous l'avons fait dans les dernières semaines :
```bash
mvn clean package
java -jar target/ghostbeacon-1.2.0.jar
```

### 3. Installez et lancez OWASP ZAP

- Dans l'[environnement de sécurité offensive](../notes/environnement-pentest), installez OWASP ZAP
   ```bash
   sudo apt update && sudo apt install zaproxy
   ```
- Lancer **OWASP ZAP** (zaproxy)
- Conserver la configuration de proxy par défaut (`127.0.0.1:8080`)
- Vérifiez que vous arrivez à communiquer avec GhostBeacon en pointant votre navigateur vers :
   - `http://<ip environnement applicatif>:8080`

---

## 1. Analyse passive (*passive scan*)

### Objectif

Observer le trafic HTTP **sans modifier les requêtes**, afin de détecter automatiquement :

- des fuites d’information
- des en‑têtes HTTP dangereux
- des comportements révélateurs de faiblesse de configuration

---

### Étapes

1. Dans OWASP ZAP, vérifiez que l’application est en cours d’exécution et que le proxy est actif.
2. Configurez votre navigateur ou Postman pour utiliser ZAP comme proxy HTTP.
3. Dans ZAP, ouvrez l’onglet **History** afin d’observer les requêtes interceptées.
4. Interagissez normalement avec GhostBeacon :
   - envoyer un message
   - consulter une boîte de réception
   - téléverser un fichier
   - consulter ou modifier un statut
5. Pendant ces interactions :
   - observez les échanges HTTP dans l’onglet **History**
   - consultez les alertes générées dans l’onglet **Alerts**

---

### Questions de réflexion

- Quels types de vulnérabilités peuvent être détectés sans attaquer activement l’application ?
- Pourquoi ce type d’analyse est‑il généralement considéré comme peu risqué ?
- Quelles classes de vulnérabilités ne peuvent pas être détectées passivement ?

---

## 2. Cartographie de la surface d'attaque (*Spider*)

### Objectif

Découvrir automatiquement la **surface d’attaque exposée** par GhostBeacon.

---

### Étapes

1. Dans OWASP ZAP, ouvrez l’onglet **Sites**.
2. Repérez le noeud correspondant à l’application  
   (ex. `http://localhost:8080`).
3. Cliquez avec le bouton droit sur le noeud racine.
4. Sélectionnez :  
   **Attack → Spider...**
5. Conservez les paramètres par défaut et lancez le Spider.
6. Suivez la progression :
   - dans l’onglet **Spider**
   - dans l’arbre **Sites**, où apparaissent les nouvelles routes découvertes

---

### Questions de réflexion

- Toutes les fonctionnalités connues ont‑elles été découvertes ?
- Que se passe‑t‑il si une route n’est pas découverte par le Spider ?
- En quoi cette phase conditionne‑t‑elle l’efficacité du reste du DAST ?

---

## 3. Analyse active (*active scan*)

### Objectif

Identifier des **vulnérabilités exploitables** à l’aide d’attaques automatisées.

---

### Étapes

1. Ouvrez l’onglet **Sites**.
2. Cliquez avec le bouton droit sur le noeud correspondant à GhostBeacon  
   (ex. `http://localhost:8080`).
3. Sélectionnez :  
   **Attack → Active Scan...**
4. Dans la fenêtre qui s’ouvre :
   - vérifiez que la cible est correcte
   - conservez la politique de scan par défaut
   - ne modifiez pas les options avancées
5. Cliquez sur **Start Scan**.
6. Pendant l’exécution :
   - observez l’onglet **Active Scan** pour la progression
   - consultez les alertes dès leur apparition dans l’onglet **Alerts**
7. Une fois le scan terminé :
   - analysez chaque alerte
   - examinez les requêtes et réponses associées

---

### Questions de réflexion

- Quelles vulnérabilités sont détectées dynamiquement ?
- Lesquelles vous semblent réellement exploitables ?
- Identifiez‑vous des faux positifs ? Pourquoi ?
- Quelles vulnérabilités ne peuvent pas être détectées par un scan automatisé ?

---

## 4. Fuzzing

### Objectif

Tester le comportement de l’application face à des **entrées malformées ou inattendues**.

---

### Étapes

1. Identifiez une requête pertinente dans :
   - l’onglet **History**
   - ou l’onglet **Sites**
2. Cliquez avec le bouton droit sur la requête choisie.
3. Sélectionnez :  
   **Attack → Fuzz...**
4. Dans la fenêtre du fuzzer :
   - sélectionnez le paramètre à fuzzzer
   - ajoutez un générateur de payloads fourni par ZAP
   - conservez les paramètres par défaut
5. Lancez le fuzzing et observez :
   - les codes de réponse HTTP
   - les variations de contenu
   - les erreurs ou comportements anormaux

---

### Questions de réflexion

- Pourquoi ce paramètre est‑il intéressant à fuzzzer ?
- En quoi le fuzzing diffère‑t‑il de l’analyse active standard ?
- Le fuzzing met‑il en évidence des comportements non détectés précédemment ?

---

## 5. Corrélation SAST / DAST

### Objectif

Comparer les vulnérabilités détectées dynamiquement avec celles identifiées statiquement lors de la **phase 1**.

---

### Questions de réflexion

- Quelles vulnérabilités ont été :
  - détectées uniquement par le SAST ?
  - détectées uniquement par le DAST ?
  - détectées par les deux approches ?
- Pourquoi certaines vulnérabilités existent‑elles dans le code sans être exploitables dynamiquement ?
- Que révèle cette comparaison sur la complémentarité des méthodes ?

## Réflexion finale

Expliquez brièvement :

- En quoi le DAST complète, sans remplacer, le SAST et le durcissement
- Pourquoi les résultats DAST doivent toujours être interprétés dans leur contexte
- Quels risques résiduels subsistent après ces analyses