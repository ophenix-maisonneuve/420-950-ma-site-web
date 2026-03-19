---
layout: default
title: "Au secours du Lobby des Braves"
nav_order: 2
has_toc: false
published: false
---

# Exercice : Au secours du Lobby des Braves

## Mise en situation
Une ancienne camarade de classe a lancé, il y a quelques mois, un nouveau jeu de rôle en ligne de type MMORPG (*Massively Multiplayer Online Role-Playing Game*).Chaque jour, le **Lobby des Braves**, le **lobby d'accueil** du jeu , bourdonne d’activité. Des milliers d’aventuriers s’y connectent pour :
- joindre des parties pour commencer de nouvelles quêtes
- consulter les classements
- s'échanger des messages via le système de messagerie

Mais en ce matin inhabituellement sombre, le lobby est bien silencieux. Personne n'arrive à s'y connecter...

Ayant eu vent de vos nouveaux super-pouvoirs en cybersécurité, votre camarade vous appelle en panique. 

Vous êtes le dernier espoir du royaume. Votre mission commence ici...

---

## Objectifs

- diagnostiquer une erreur TLS réelle
- analyser la relation entre une clé privée et un certificat X.509
- générer une nouvelle clé privée et un CSR correct
- signer ce CSR à l’aide d’une CA racine
- installer proprement ce certificat sur un serveur Nginx
- appliquer une configuration HTTPS sécuritaire

---
## Préparation

 1. Dans votre environnement applicatif, créez un nouveau répertoire `/var/www/portal.lobbydesbraves.test` pour le site web du portail
  ```bash
  mkdir -p /var/www/portal.lobbydesbraves.test`
  ```
 1. Copiez les fichiers web du portail dans `/var/www/portal.lobbydesbraves.test`
   - [index.html](../assets/files/seance3/index.html) : fichier HTML du portail
   - [style.css](../assets/files/seance3/style.css) : fichier CSS pour le style du portail
   - [lobby-braves.png](../assets/files/seance3/lobby-braves.png) : *sublime* logo du Lobby des Braves

1. Copiez le fichier suivant dans `/etc/nginx/sites-available/`
   - [portal.lobbydesbraves.test.conf](../assets/files/seance3/portal.lobbydesbraves.test.conf)

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

Cependant :
- Nginx échoue à charger le certificat,  
- le navigateur refuse la connexion,  
- la commande `openssl` révèle une anomalie critique.

Le technicien ayant effectué une maintenance a accidentellement régénéré une nouvelle clé privée, sans regénérer de certificat correspondant.

Résultat :
```
SSL: error:0B080074:x509 certificate routines:X509_check_private_key:key values mismatch
```
Votre rôle est de reproduire, comprendre et réparer.

---
## 4. Tâches à réaliser (détaillées)

### 🕵️ Étape 1 — Diagnostic précis de l’incident
1. Accédez au portail dans un navigateur.
2. Inspectez la clé privée :
```
openssl rsa -noout -modulus -in /etc/nginx/certs/wrong.key
```
3. Inspectez le certificat brisé :
```
openssl x509 -noout -modulus -in /etc/nginx/certs/broken.crt
```
4. Comparez les valeurs.
5. Confirmez le mismatch.

**Livrables :** erreurs navigateur, extraits modulus, explication.

---
### 🗝️ Étape 2 — Génération d’un certificat serveur valide
1. Générer une clé privée :
```
openssl genrsa -out server.key 2048
```
2. Générer un CSR :
```
openssl req -new \ 
  -key server.key \ 
  -subj "/CN=portal.lobbydesbraves.test" \ 
  -addext "subjectAltName=DNS:portal.lobbydesbraves.test" \ 
  -out server.csr
```
3. Signer le certificat via CA racine :
```
openssl x509 -req \ 
  -in server.csr \ 
  -CA /etc/nginx/certs/CA/rootCA.pem \ 
  -CAkey /etc/nginx/certs/CA/rootCA.key \ 
  -CAcreateserial \ 
  -days 365 \ 
  -sha256 \ 
  -out server.crt
```
4. Vérifier :
```
openssl x509 -in server.crt -text -noout
```

**Livrables :** commandes, extraits CSR/certificat, explication SAN.

---
### 🔧 Étape 3 — Installation et configuration Nginx
Copier les fichiers :
```
/etc/nginx/certs/server.crt
/etc/nginx/certs/server.key
```
Configuration sécurisée :
```
server {
    listen 80;
    server_name portal.lobbydesbraves.test;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name portal.lobbydesbraves.test;

    ssl_certificate /etc/nginx/certs/server.crt;
    ssl_certificate_key /etc/nginx/certs/server.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    ssl_ciphers         'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:'         'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';

    ssl_prefer_server_ciphers on;

    add_header Strict-Transport-Security "max-age=63072000" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    root /var/www/lobbydesbraves;
    index index.html;
}
```
Redémarrer Nginx :
```
sudo systemctl restart nginx
```

**Livrables :** configuration complète + preuve de démarrage.

---
### ✔️ Étape 4 — Validation complète
1. Tester via curl :
```
curl -vk https://portal.lobbydesbraves.test/
```
2. Tester via OpenSSL :
```
openssl s_client \ 
  -connect portal.lobbydesbraves.test:443 \ 
  -servername portal.lobbydesbraves.test \ 
  -showcerts
```
3. Vérifier via navigateur.

**Livrables :** extraits curl/s_client, cadenas HTTPS, conclusion.

---
## ⭐ 5. Étape bonus (non évaluée)
Démonstration : AC intermédiaire, fullchain.

---
## 📄 6. À remettre
PDF ou Markdown contenant :
- analyses ;
- commandes ;
- extraits de certificats ;
- configuration Nginx ;
- preuves de validation ;
- conclusion.

---
## 🏁 7. Conclusion
Grâce à votre intervention, le portail du royaume renaît et les aventuriers reprennent leurs quêtes.
