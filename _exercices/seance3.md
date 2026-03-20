---
layout: default
title: "Au secours du Lobby des Braves"
nav_order: 2
has_toc: false
published: true
---

# Exercice : Au secours du Lobby des Braves

## Mise en situation
Une ancienne camarade de classe a lancé, il y a quelques mois, un nouveau jeu de rôle en ligne de type MMORPG (*Massively Multiplayer Online Role-Playing Game*). Chaque jour, le **Lobby des Braves**, le lobby d'accueil du jeu, bourdonne d’activité. Des milliers d’aventuriers s’y connectent pour :
- joindre des parties pour commencer de nouvelles quêtes
- consulter les classements
- s'échanger des messages via le système de messagerie

Mais en ce matin inhabituellement sombre, le lobby est bien silencieux. Personne n'arrive à s'y connecter depuis que l'administrateur de systèmes de la compagnie (aussi connu sous le pseudo de **UltimateNoob2000** dans le jeu) a tenté de faire une rotation du certificat X.509, qui arrivait bientôt à échéance...

Ayant eu vent de vos nouveaux superpouvoirs en cybersécurité, votre camarade vous appelle en panique. 

Vous êtes le dernier espoir du royaume. Votre mission commence ici!

---

## Objectifs

- diagnostiquer une erreur HTTPS réelle
- analyser la relation entre une clé privée et un certificat X.509
- générer une nouvelle clé privée et un CSR correct
- signer ce CSR à l’aide d’une CA racine
- installer proprement ce certificat sur un serveur Nginx
- appliquer une configuration HTTPS sécuritaire

---

## Préparation

 1. Dans votre environnement applicatif, créez un nouveau répertoire `/var/www/portal.lobbydesbraves.test` pour le site web du portail
    ```bash
    sudo mkdir -p /var/www/portal.lobbydesbraves.test`
    ```
 1. Copiez les fichiers web du portail dans `/var/www/portal.lobbydesbraves.test`
    - [index.html](../assets/files/seance3/index.html) : fichier HTML du portail
    - [style.css](../assets/files/seance3/style.css) : fichier CSS pour le style du portail
    - [lobby-braves.png](../assets/files/seance3/lobby-braves.png) : *sublime* logo du Lobby des Braves

1. Copiez le fichier suivant dans `/etc/nginx/sites-available/`
   - [portal.lobbydesbraves.test.conf](../assets/files/seance3/portal.lobbydesbraves.test.conf)

1. Activez le site web
    ```bash
    sudo ln -s /etc/nginx/sites-available/portal.lobbydesbraves.test.conf /etc/nginx/sites-enabled/portal.lobbydesbraves.test.conf
    ```

1. Copiez le certificat dans `/etc/nginx/certs`
    - [lobby.crt](../assets/files/seance3/lobby.crt)
    - Si le dossier n'existe pas :
      ```bash
      sudo mkdir /etc/nginx/certs
      ```

1. Copiez la clé privée dans `/etc/nginx/keys`
    - [lobby.crt](../assets/files/seance3/lobby.key)
    - Si le dossier n'existe pas :
        ```bash
        sudo mkdir /etc/nginx/keys
        sudo chown -R www-data:www-data /etc/nginx/keys
        sudo chmod -R 500 /etc/nginx/keys
        ```

1. Redémarrer le serveur web
    ```bash
    sudo systemctl restart nginx
    ```

1. Finalement, simulez la résolution DNS pour le domaine `portal.lobbydesbraves.test` en modifier le fichier d'hôtes

   ```bash
   sudo nano /etc/hosts
   ```

   ```text
   192.168.50.10   portal.lobbydesbraves.test  //remplacez par l'IP de votre VM
   ```

---

## 1. Diagnostic de l’incident
1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)

1. Récupérez le certificat qui est fourni par le portail.
    - Comment avez-vous réussi à récupérer le certificat ?

1. Inspectez le certificat récupéré...
    - à l'aide du navigateur ou de l'explorateur système
    - à l'aide d'une commande OpenSSL
      - Quelle est la commande qui a été utilisée ?


1. Générez la clé publique correspondante à la clé privée utilisée (`lobby.key`)
    - Quelle commande OpenSSL a été utilisée ?

1. Comparez la clé publique générée à l'étape précédente et la clé publique contenue dans le certificat.
    - Que remarquez-vous ?
    - Formulez une hypothèse quant à ce qui s'est produit au moment du renouvellement du certificat.



---

## 2. Génération d’un certificat auto-signé
1. Générez une nouvelle clé privée appelée `lobby.key` utilisant l'un des algorithmes suivants : RSA, DSA, EC ou ED25519
    - Quelle est la commande OpenSSL utilisée ?
1. Générez une requête de signature de certificat (*CSR*) appelée `lobby.csr`
    - Quelle est la commande OpenSSL utilisée ?
1. Inspectez votre requête de signature de certificat
    - Le fichier est-il lisible en texte clair ?
    - Existe-t-il une commande OpenSSL permettant de l'afficher de façon plus lisible ? Laquelle ?
1. Signez la demande de certificat avec la même clé privée et appelez le certificat résultant `lobby.crt`
    - Quelle commande OpenSSL avez-vous utilisée ?
1. Remplacez le certificat et la clé sur le serveur et redémarrer nginx
    ```bash
    sudo cp lobby.crt /etc/nginx/certs/
    sudo cp lobby.key /etc/nginx/keys/
    sudo systemctl restart nginx
    ```
1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)
    - Est-ce que le certificat a complètement réglé le problème ? Pourquoi ?

---

## 3. Génération d'une autorité de certification
1. Générez une nouvelle clé privée nommée `ca_lobby.key` utilisant l'un des algorithmes suivants : RSA, DSA, EC ou ED25519
    - Quelle est la commande OpenSSL utilisée ?
1. Générez un certificat auto-signé pour une nouvelle autorité de certification interne nommé `ca_lobby.crt`...
    - en y ajoutant les extensions suivantes :
      - basicConstraints=CA:TRUE
      - keyUsage=digitalSignature,cRLSign,keyCertSign
    - et en précisant le **Autorite de certifiation Lobby des Braves** lorsque l'on vous demande le **Common Name**
1. Inspectez votre nouveau certificat
    - Quelle commande OpenSSL avez-vous utilisée ?
    - Quelle est la différence principale entre ce certificat et le certificat du serveur généré précédemment ?
1. Signez la demande de certificat avec votre nouvelle autorité de certification et appelez le certificat résultant `lobby.crt`
    - Quelle commande OpenSSL avez-vous utilisée ?
    - Pourquoi devez-vous fournir le certificat et la clé privée du CA pour effectuer cette opération ?
1. Remplacez le certificat sur le serveur et redémarrer nginx
    ```bash
    sudo cp lobby.crt /etc/nginx/certs/
    sudo systemctl restart nginx
    ```

1. Simulez que votre CA est une autorité de certification reconnue en installant manuellement le certifiat `ca_lobby.crt` dans votre navigateur

1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)
    - Est-ce que le problème est maintenant résolu ?

## 4. Sécurisation de la configuration NGINX

1. Ajoutez des options de sécurité dans la configuration du portail
    ```bash
    sudo nano /etc/etc/nginx/portal.lobbydesbraves.test.conf
    ```

    - Activez **HSTS** (*HTTP String Transport Security*) pour forcer l’usage de HTTPS
    - Activez uniquement les versions sécuritaires de TLS (TLSv1.2 et TLSv1.3)
    - Forcez explicitement la redirection de HTTP vers HTTPS

---

**Félicitations! Vous avez sauvé le royaume!**
