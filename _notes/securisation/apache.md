---
layout: default
title: Apache
parent: Sécurisation d'un serveur web
nav_order: 2
---

# Apache

**Apache HTTP Server** est l’un des serveurs web les plus utilisés au monde. Il offre une grande flexibilité et permet de mettre en place facilement une configuration HTTPS basée sur des certificats X.509. Cette page présente les étapes essentielles pour activer SSL/TLS sur Apache, importer un certificat et sécuriser un site web.citeturn2search2

---

## Activation du module SSL
Avant de pouvoir utiliser HTTPS, il faut activer le module SSL intégré à Apache :

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
```
Ce module permet à Apache de gérer les connexions HTTPS et les certificats X.509.citeturn2search2

---

## Ajout du certificat et de la clé privée
Comme pour Nginx, Apache utilise les répertoires standards :

- `/etc/ssl/certs/` pour le certificat signé :
```bash
sudo cp certificat_signe.crt /etc/ssl/certs/
```citeturn2search2

- `/etc/ssl/private/` pour la clé privée :
```bash
sudo cp cle_privee.pem /etc/ssl/private/
```citeturn2search2

Les permissions de `/etc/ssl/private/` doivent empêcher tout accès non autorisé.

---

## Configuration du VirtualHost HTTPS
On crée un fichier de configuration pour le port sécurisé :

```bash
sudo nano /etc/apache2/sites-available/serveurlinux3.conf
```
Avec le contenu suivant :

```apache
<VirtualHost *:443>
    ServerName serveurlinux3.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificat_signe.crt
    SSLCertificateKeyFile /etc/ssl/private/cle_privee.pem

    DocumentRoot /var/www/serveurlinux3

    <Directory /var/www/serveurlinux3>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
citeturn2search2

Cette configuration :
- active HTTPS sur le port 443 ;
- associe le certificat et la clé privée ;
- définit le dossier racine du site web ;
- autorise l'accès via Apache.

---

## Redirection de HTTP vers HTTPS
Pour forcer tout le trafic à utiliser HTTPS :

```apache
<VirtualHost *:80>
    ServerName serveurlinux3.com
    Redirect / https://serveurlinux3.com/
</VirtualHost>
```
Une fois ce bloc ajouté au fichier de configuration, toutes les requêtes HTTP seront redirigées vers HTTPS.

---

## Activation du site et redémarrage
On active ensuite le site :

```bash
sudo a2ensite serveurlinux3.conf
sudo systemctl reload apache2
```
citeturn2search2

Si tout est correct, Apache sert maintenant le site en HTTPS.

---

Apache fournit une grande flexibilité pour déployer des sites sécurisés en HTTPS. Grâce à son VirtualHost sur port 443 et à la gestion intégrée de SSL/TLS, il constitue un choix solide pour sécuriser des services web en production.citeturn2search2
