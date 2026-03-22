---
layout: default
title: "Corrigé - Au secours du Lobby des Braves"
parent: "Au secours du Lobby des Braves"
nav_order: 2
has_toc: false
published: false
---

# Exercice : Au secours du Lobby des Braves

## 1. Diagnostic de l’incident
1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)

1. Récupérez le certificat qui est fourni par le portail.
    - Comment avez-vous réussi à récupérer le certificat ?
        - On peut récupérer le certificat directement à partir du navigateur en cliquant dans la barre d'adresse.
        - On peut également utiliser la commande `openssl s_client -connect example.com:443 -showcerts` pour afficher le certificat et ses informations.

1. Inspectez le certificat récupéré...
    - à l'aide du navigateur ou de l'explorateur système
    - à l'aide d'une commande OpenSSL
      ```bash
      openssl x509 -in <certificat> -text -noout
      ```


1. Formulez une hypothèse quant à ce qui s'est produit au moment du renouvellement du certificat. **Les dates de début et de fin ne sont pas bonnes. Il est probable que le serveur sur lequel l'administrateur réseau a généré le certificat n'avait pas la bonne date, et que cela ait eu un impact sur les dates qui ont été insérées dans le certificat**

---

## 2. Génération d’un certificat auto-signé
1. Générez une nouvelle clé privée appelée `lobby.key` utilisant l'un des algorithmes suivants : RSA, DSA, EC ou ED25519
    ```bash
    openssl genpkey -algorithm rsa -out lobby.key -pkeyopt rsa_keygen_bits:4096

    ```
    
1. Générez une requête de signature de certificat (*CSR*) appelée `lobby.csr`
    ```bash
    openssl req -new -key lobby.key -out lobby.csr -addext "subjectAltName = DNS:portal.lobbydesbraves.test"
    ```
1. Inspectez votre requête de signature de certificat
    - Le fichier est-il lisible en texte clair ? **Il est encodé en base64, donc pas vraiment lisible.**
    - Existe-t-il une commande OpenSSL permettant de l'afficher de façon plus lisible ? Laquelle ?
    ```bash
    openssl req -in lobby.csr -test -noout
    ```
1. Signez la demande de certificat avec la même clé privée et appelez le certificat résultant `lobby.crt`
    ```bash
    openssl x509 -req -in lobby.csr -signkey lobby.key -out lobby.crt -days 365 -copy_extensions copyall
    ```
1. Remplacez le certificat et la clé sur le serveur et redémarrer nginx
    ```bash
    sudo cp lobby.crt /etc/nginx/certs/
    sudo cp lobby.key /etc/nginx/keys/
    sudo systemctl restart nginx
    ```
1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)
    - Est-ce que le certificat a complètement réglé le problème ? Pourquoi ? **Non, car un certificat auto-signé n'est pas considéré comme sécuritaire par les navigateurs modernes. Ils doivent être signés par une autorité de certification reconnue ou ajoutée manuellement par l'utilisateur.

---

## 3. Signature avec une autorité de certification
1. Récupérez les fichiers suivants :
    - [Certificat de l'autorité de certification](../assets/files/seance3/ca_lobby.crt)
    - [Clé privée de l'autorité de certification](../assets/files/seance3/ca_lobby.key)
1. Inspectez le certificat récupéré
    - Quelle est la différence principale entre ce certificat et le certificat du serveur généré précédemment ? **La présence des extensions suivantes:**
        - **basicConstraints=CA:TRUE** 
        - **keyUsage=digitalSignature,cRLSign,keyCertSign**
1. Signez la demande de certificat avec l'autorité de certification et appelez le certificat résultant `lobby.crt`
    ```bash
    openssl x509 -req -in lobby.csr -CA ca_lobby.crt -CAkey ca_lobby.key -CAcreateserial -out lobby.crt -days 365 -sha256 -copy_extensions copyall
    ```
    - Pourquoi devez-vous fournir le certificat **et** la clé privée du CA pour effectuer cette opération ?
1. Remplacez le certificat sur le serveur et redémarrer nginx
    ```bash
    sudo cp lobby.crt /etc/nginx/certs/
    sudo systemctl restart nginx
    ```

1. Simulez que votre CA est une autorité de certification reconnue en installant manuellement le certifiat `ca_lobby.crt` dans votre navigateur

1. Accédez au portail dans un navigateur.
    - [https://portal.lobbydesbraves.test](https://portal.lobbydesbraves.test)
    - Est-ce que le problème est maintenant résolu ? **Après avoir ajouté l'autorité de certification à la liste des autorités reconnues, la connexion au portail s'effectue de manière sécurisés.**

## 4. Sécurisation de la configuration NGINX

1. Ajoutez des options de sécurité dans la configuration du portail
    ```bash
    sudo nano /etc/etc/nginx/portal.lobbydesbraves.test.conf
    ```

    - Activez **HSTS** (*HTTP String Transport Security*) pour forcer l’usage de HTTPS
    - Activez uniquement les versions sécuritaires de TLS (TLSv1.2 et TLSv1.3)
    - Forcez explicitement la redirection de HTTP vers HTTPS

    ```nginx                          
    server {
        listen 443 ssl;
        http2 on;
        server_name portal.lobbydesbraves.test;

        ssl_certificate /etc/nginx/certs/lobby.crt;
        ssl_certificate_key /etc/nginx/keys/lobby.key;

        ssl_protocols TLSv1.2 TLSv1.3;

        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

        root /var/www/portal.lobbydesbraves.test;
        index index.html;
    }

    server {
        listen 80;
        server_name portal.lobbydesbraves.test;
        return 301 https://$host$request_uri;
    }
    ```

1. À l'aide de l'outil **Wireshark**, confirmez que toutes les communications avec le portail sont bien sécurisées avec TLS.


**Félicitations! Vous avez sauvé le royaume!**

