---
layout: default
title: Nginx
parent: Sécurisation d'un serveur web
nav_order: 1
---

# Nginx

**Nginx** est un serveur web performant largement utilisé pour héberger des sites et des applications. L’une de ses forces réside dans sa capacité à gérer efficacement une configuration HTTPS moderne et sécurisée. Cette page présente les étapes essentielles pour installer Nginx, ajouter un certificat X.509 et configurer un site web sécurisé.citeturn2search2

---

## Installation de Nginx
Sous Debian ou Ubuntu, Nginx peut être installé via :

```bash
sudo apt update && sudo apt install nginx -y
```citeturn2search2

Une fois installé, le service peut être contrôlé avec :
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## Ajout du certificat et de la clé privée
Les certificats doivent être placés dans les répertoires standards :

- `/etc/ssl/certs/` pour les certificats signés, par exemple :
```bash
sudo cp certificat_signe.crt /etc/ssl/certs/
```citeturn2search2

- `/etc/ssl/private/` pour les clés privées :
```bash
sudo cp cle_privee.pem /etc/ssl/private/
```citeturn2search2

Ces répertoires sont protégés et conçus pour héberger les fichiers sensibles liés à TLS.

---

## Création d’un site web simple
On crée d'abord un dossier pour héberger le site :
```bash
sudo mkdir -p /var/www/serveurlinux3
sudo nano /var/www/serveurlinux3/index.html
```
Puis on y ajoute une page HTML simple.citeturn2search2

---

## Configuration du serveur HTTPS
Pour activer HTTPS, on crée un fichier de configuration dédié :

```bash
sudo nano /etc/nginx/sites-available/serveurlinux3.conf
```
Avec le contenu :

```nginx
server {
    listen 443 ssl;
    server_name serveurlinux3.com;

    ssl_certificate /etc/ssl/certs/certificat_signe.crt;
    ssl_certificate_key /etc/ssl/private/cle_privee.pem;

    location / {
        root /var/www/serveurlinux3;
        index index.html index.htm;
    }
}

server {
    listen 80;
    server_name serveurlinux3.com;
    return 301 https://$host$request_uri;
}
```citeturn2search2

Cette configuration :
- active le service HTTPS sur le port 443 avec TLS ;
- sert le contenu du répertoire `/var/www/serveurlinux3` ;
- redirige tout le trafic HTTP vers HTTPS.

---

## Activation du site
Pour activer le site :
```bash
sudo ln -s /etc/nginx/sites-available/serveurlinux3.conf /etc/nginx/sites-enabled/
```
Puis redémarrer Nginx :
```bash
sudo systemctl restart nginx
```citeturn2search2

Le site est maintenant disponible en HTTPS.

---

Nginx permet de mettre en place rapidement une configuration HTTPS robuste utilisant des certificats X.509. Ces étapes constituent la base d’une installation sécurisée pour un serveur web moderne.citeturn2search2
