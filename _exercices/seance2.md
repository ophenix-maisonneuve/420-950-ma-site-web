---
layout: default
title: "Mission : ECHO SILENCIEUX"
nav_order: 1
has_toc: false
published: true
---

# Exercice : Mission ECHO SILENCIEUX

## Mise en situation
Vous êtes analyste principal au quartier général de l'unité technique **CyberMax**, une unité secrète spécialisée dans le support logistique aux opérations clandestines. Il est 03h17. Votre écran s’allume soudainement avec une notification prioritaire rouge - niveau **maximal**. 

Un message vient d’être reçu de l’agente infiltrée **Shadow**, actuellement en mission sous couverture dans un port contrôlé par un groupe criminel international. Shadow signale avoir obtenu des informations critiques concernant un transfert illégal imminent. Toutefois, le canal sécurisé utilisé habituellement semble compromis : plusieurs tentatives d’interception ont été détectées, et certaines métadonnées observées par vos systèmes suggèrent des manipulations possibles.

Votre équipe doit donc suivre le protocole **ECHO SILENCIEUX**, conçu pour maintenir l’intégrité, l’authenticité et la confidentialité des communications, même en environnement hostile. Chaque erreur pourrait compromettre la mission ou même la sécurité de Shadow...

Votre mission :
1. Vérifier que le message transmis n’a pas été corrompu.
2. Signer votre réponse à Shadow afin de lui confirmer votre identité.
3. Envoyer une réponse confidentielle... sans que les attaquants ne puissent la lire.

Le destin de l’opération est entre vos mains!

---

## Objectifs

- Comprendre les concepts de hachage, de signature numérique et de chiffrement.
- Apprendre à utiliser OpenSSL pour manipuler du matériel cryptographique.
- Reconnaître différents types de falsification de données et comment s'en protéger.

---

## 1. Vérification d’intégrité
Les attaquants pourraient tenter de modifier le message original de Shadow : changer une coordonnée, altérer une date, substituer une direction… Une altération minime pourrait entraîner une opération désastreuse sur le terrain.

1. Récupérez le message `shadow.txt` [ici](../assets/files/seance2/shadow.txt) et son condensé `shadow.hash` [ici](../assets/files/seance2/shadow.hash) dans le matériel du cours
1. Générez un hash SHA‑256 nommé `shadow.sha256`
   - Quelle commande avez-vous utilisée ?
1. Consultez le fichier `shadow.sha256`
   - Quelle est la longueur du condensé produit ?
1. Comparez le hash généré avec celui que Shadow a transmis (`shadow.hash`)
   - Que constatez-vous ?
1. Faites une légère modification dans le fichier `shadow.txt` (un seul caractère), puis répétez les étapes ci-haut.
   - Que constatez-vous ?
1. À cette étape, pouvons-nous être certains que le message provient bel et bien de Shadow ? Pourquoi ?

## 2. Authentification (signature numérique)
Vous désirez répondre à Shadow, et pour une sécurité accrue, vous décidez de signer votre message afin qu'elle sache qu'il provient bien du quartier général de **CyberMax**.

1. Générez une paire de clés RSA de 2048 bits
   - Quelle commande avez-vous utilisée ?
   - Dans les fichiers générés, lequel correspond à la clé privée et lequel correspond à la clé publique ?

2. Signez le message `reponse.txt` disponible [ici](../assets/files/seance2/reponse.txt)
   - Quelle commande avez-vous utilisée ?
   - Quelles opérations ont été effectuées par cette commande ?

3. Effectuez une validation de votre signature

   C'est cette opération que devra effectuer Shadow pour valider l'authenticité de votre réponse. *Pour les besoins de l'exercice, nous assumons que Shadow connaît déjà votre clé publique.*

   - Quelle commande avez-vous utilisée ?
   - Quelles opérations ont été effectuées par cette commande ?

4. Modifiez légèrement le fichier de signature (un seul caractère suffit), puis refaites l'étape de la validation.
   - Décrivez et expliquez ce qui se produit.

## 3. Confidentialité (chiffrement)
Avant de transmettre votre réponse signée à Shadow, vous vous souvenez d'un détail : le canal est compromis. Vous devez supposer que quelqu’un surveille.

Votre objectif : *que seul Shadow puisse lire votre réponse.*

### Option A — Chiffrement RSA simple
1. Obtenez la clé publique de Shadow [ici](../assets/files/seance2/shadow.pub)
1. Chiffrez votre réponse avec sa clé privée.
   - Quelle commande avez-vous utilisée ?
   - Qu'observez-vous ?
1. Simulez une réponse beaucoup plus courte, et refaites l'étape précédente.
   - Obtenez-vous un résultat différent ?
3. Simulez le déchiffrement que Shadow fera avec sa clé privée disponible [ici](../assets/files/seance2/shadow.key)
   
   *Normalement, seule Shadow connaîtrait sa clé privée!*

   - Quelle commande avez-vous utilisée ?

### Option B — Schéma hybride AES + RSA (méthode recommandée)
1. Générez une clé AES aléatoire :
   - Quelle commande avez-vous utilisée ?
2. Chiffrez votre message avec la clé AES
   - Quelle commande avez-vous utilisée ?
3. Chiffrez la clé AES pour Shadow la faire parvenir de façon sécuritaire à Shadow.
   - Quelle commande avez-vous utilisée ?
4. Simulez les étapes que Shadow devra effectuer pour déchiffrer votre message.
   - Quelles commandes devez-vous effectuer ?

{: .remarque}
> Dans le monde réel, presque toutes les communications sécurisées (HTTPS, VPN, messageries chiffrées) utilisent un schéma hybride comme celui-ci, où la clé symétrique est partagée d'une façon sécurisée. Vous êtes en train d’utiliser les mêmes techniques que les analystes professionnels.



