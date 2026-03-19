---
layout: default
title: "Corrigé - Mission : ECHO SILENCIEUX"
parent: "Mission : ECHO SILENCIEUX"
nav_order: 1
has_toc: false
published: true
---

# Exercice : Mission ECHO SILENCIEUX

## 1. Vérification d’intégrité
Les attaquants pourraient tenter de modifier le message original de Shadow : changer une coordonnée, altérer une date, substituer une direction… Une altération minime pourrait entraîner une opération désastreuse sur le terrain.

1. Récupérez le message `shadow.txt` [ici](../assets/files/seance2/shadow.txt) et son condensé `shadow.hash` [ici](../assets/files/seance2/shadow.hash) dans le matériel du cours
1. Générez un hash SHA‑256 nommé `shadow.sha256`
   ```bash
   openssl dgst -sha256 > shadow.sha256
   ```
1. Consultez le fichier `shadow.sha256`
   - Quelle est la longueur du condensé produit ? **256 bits (soit 64 caractères en hexadécimal)**
1. Comparez le hash généré avec celui que Shadow a transmis (`shadow.hash`)
   - Que constatez-vous ? **Il est identique**
1. Faites une légère modification dans le fichier `shadow.txt` (un seul caractère), puis répétez les étapes ci-haut.
   - Que constatez-vous ? **Le condensé produit est drastiquement différent**
1. À cette étape, pouvons-nous être certains que le message provient bel et bien de Shadow ? Pourquoi ? **Non, car un attaquant aurait très bien pu substituer à la fois le message et son condensé pour nous berner. Pour valider l'identité de Shadow, il aurait fallu que le message soit signé.**

## 2. Authentification (signature numérique)
Vous désirez répondre à Shadow, et pour une sécurité accrue, vous décidez de signer votre message afin qu'elle sache qu'il provient bien du quartier général de **CyberMax**.

1. Générez une paire de clés RSA de 2048 bits
   ```bash
   openssl genpkey -algorithm rsa -pkeyopt rsa_keygen_bits:2048 -out cybermax.key
   openssl rsa -pubout -in cybermax.key -out cybermax.pub
   ```
   - Dans les fichiers générés, lequel correspond à la clé privée et lequel correspond à la clé publique ? ***cybermax.key* est la clé privée et *cybermax.pub* est la clé publique**

2. Signez le message `reponse.txt` disponible [ici](../assets/files/seance2/reponse.txt)
   ```bash
   openssl dgst -sha256 -sign cybermax.key -out reponse.bin reponse.txt
   ```
   - Quelles opérations ont été effectuées par cette commande ? **OpenSSL a d'abord calculé un hash avec SHA-256, puis a appliqué la signature en utilisant la clé privée *cybermax.key***

3. Effectuez une validation de votre signature

   C'est cette opération que devra effectuer Shadow pour valider l'authenticité de votre réponse. *Pour les besoins de l'exercice, nous assumons que Shadow connaît déjà votre clé publique.*

   ```bash
   openssl dgst -sha256 -verify cybermax.pub -signature reponse.bin reponse.txt
   ```
   - Quelles opérations ont été effectuées par cette commande ? **OpenSSL "déchiffre" d'abord le condensé signé à l'aide de la clé publique *cybermax.pub*, puis calcule le condensé sur le fichier à valider. Si le condensé reçu et le condensé calculé sont identiques, la signature est validée.**

4. Modifiez légèrement le fichier de signature (un seul caractère suffit), puis refaites l'étape de la validation.
   - Décrivez et expliquez ce qui se produit. **Même la plus minime des modifications fait en sorte que la signature n'est plus valide.**

## 3. Confidentialité (chiffrement)
Avant de transmettre votre réponse signée à Shadow, vous vous souvenez d'un détail : le canal est compromis. Vous devez supposer que quelqu’un surveille.

Votre objectif : *que seul Shadow puisse lire votre réponse.*

### Option A — Chiffrement RSA simple
1. Obtenez la clé publique de Shadow [ici](../assets/files/seance2/shadow.pub)
1. Chiffrez votre réponse avec sa clé privée.
   ```bash
   openssl pkeyutl -encrypt -inkey shadow.pub -pubin -in reponse.txt -out reponse.crypt
   ```
   - Qu'observez-vous ? **Une erreur se produit, car le message à chiffrer est plus long que la clé. Pour chiffrer à l'aide d'un algorithme asymétrique comme RSA, la clé doit avoir sensiblement la même longueur que la taille des données à chiffrer, ce qui est inefficace.**
1. Simulez une réponse beaucoup plus courte, et refaites l'étape précédente.
   - Obtenez-vous un résultat différent ? **Avec une réponse plus courte que la taille de la clé, OpenSSL arrive à chiffrer correctement**
3. Simulez le déchiffrement que Shadow fera avec sa clé privée disponible [ici](../assets/files/seance2/shadow.key)
   
   *Normalement, seule Shadow connaîtrait sa clé privée!*

   ```bash
   openssl pkeyutl -decrypt -inkey shadow.key -in reponse-courte.crypt -out reponse-courte.txt
   ```

### Option B — Schéma hybride AES et RSA (méthode recommandée)
1. Générez une clé AES aléatoire :
   ```bash
   openssl rand -hex -out aes.key 32
   ```
2. Chiffrez votre message avec la clé AES
   ```bash
   openssl enc -aes-256-ecb -salt -in reponse.txt -out reponse.crypt -K $(cat aes.key)
   ```
3. Chiffrez la clé AES à l'aide de la clé publique de Shadow afin de la lui transmettre de façon confidentielle.
   ```bash
   openssl pkeyutl -encrypt -inkey shadow.pub -pubin -in aes.key -out aes.crypt
   ```
4. Simulez les étapes que Shadow devra effectuer pour déchiffrer votre message.
   ```bash
   openssl pkeyutl -decrypt -inkey shadow.key -in aes.crypt -out aes.key
   openssl enc -d -aes-256-ecb -in reponse.crypt -out reponse.txt -K $(cat aes.key)
   ```

{: .highlight}
> Dans le monde réel, presque toutes les communications sécurisées (HTTPS, VPN, messageries chiffrées) utilisent un schéma hybride comme celui-ci, où la clé symétrique est partagée d'une façon sécurisée. Vous êtes en train d’utiliser les mêmes techniques que les analystes professionnels.



