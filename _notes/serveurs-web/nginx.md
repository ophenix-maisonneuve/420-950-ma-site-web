---
layout: default
title: Nginx
parent: Sécurisation d'un serveur web
nav_order: 1
---

# Nginx

**Nginx** est un serveur web performant largement utilisé pour héberger des sites et des applications. L’une de ses forces réside dans sa capacité à gérer efficacement une configuration HTTPS moderne et sécurisée. Cette page présente les étapes essentielles pour installer Nginx, ajouter un certificat X.509 et configurer un site web sécurisé.

---

## Installation de Nginx
Sous Debian ou Ubuntu, Nginx peut être installé via :

```bash
sudo apt update && sudo apt install nginx -y
```

Une fois installé, le service peut être contrôlé avec :
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
```

---

## Ajout du certificat et de la clé privée
Les certificats peuvent être placés dans les répertoires standards...

- `/etc/ssl/certs/` pour les certificats signés, par exemple :
```bash
sudo cp certificat.crt /etc/ssl/certs/
```

- `/etc/ssl/private/` pour les clés privées :
```bash
sudo cp cle_privee.key /etc/ssl/private/
```

... ou encore des répertoires spécifiques à nginx (à condition de bien les sécuriser), par exemple :

- `/etc/nginx/certs/` pour les certificats signés, par exemple :
```bash
sudo mkdir /etc/nginx/certs
sudo cp certificat.crt /etc/nginx/certs/
```

- `/etc/nginx/keys/` pour les clés privées :
```bash
sudo mkdir /etc/nginx/keys
sudo chown nginx:nginx /etc/nginx/keys
sudo chmod 500 /etc/nginx/keys
sudo cp cle_privee.key /etc/nginx/keys
```

---

## Création d’un site web simple
On crée d'abord un dossier pour héberger le site :
```bash
sudo mkdir -p /var/www/example.com
sudo nano /var/www/example.com/index.html
```
Puis on y ajoute une page HTML simple.citeturn2search2

---

## Configuration du serveur HTTPS
Pour activer HTTPS, on crée un fichier de configuration dédié :

```bash
sudo nano /etc/nginx/sites-available/example.com.conf
```
Avec le contenu :

```nginx
server {
    listen 443 ssl;
    http2;
    server_name example.com;

    location / {
        root /var/www/example.com;
        index index.html index.htm;
    }

    # Certificats (obligatoire pour HTTPS)
    ssl_certificate /etc/ssl/certs/certificat.crt;
    ssl_certificate_key /etc/ssl/private/cle_privee.key;
}

# Redirection HTTP → HTTPS (basique mais essentielle)
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```

Cette configuration :
- active le service HTTPS sur le port 443 avec TLS
- sert le contenu du répertoire `/var/www/example.com`
- redirige tout le trafic HTTP vers HTTPS


## Ajout d'options de sécurité
On peut ensuite ajouter plusieurs directives pour améliorer la sécurité :

```nginx
server {
    listen 443 ssl;
    http2;
    server_name example.com;

    location / {
        root /var/www/example.com;
        index index.html index.htm;
    }

    # Certificats (obligatoire pour HTTPS)
    ssl_certificate /etc/ssl/certs/certificat.crt;
    ssl_certificate_key /etc/ssl/private/cle_privee.key;


    # Protocoles SSL sécurisés (évite les vieux protocoles vulnérables)
    ssl_protocols TLSv1.2 TLSv1.3;

    # HSTS : force l’usage du HTTPS
    add_header Strict-Transport-Security "max-age=31536000" always;

    # Anti-clickjacking
    add_header X-Frame-Options "DENY" always;

    # Empêche l'analyse dangereuse du type MIME
    add_header X-Content-Type-Options "nosniff" always;

    # Masque la version de nginx (pas d’info pour les attaquants)
    server_tokens off;

    # Empêche l’accès aux fichiers sensibles (à modifier au besoin)
    location ~ /\.(git|ht|env) {
        deny all;
    }
}

# Redirection HTTP → HTTPS (basique mais essentielle)
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```

---

## Activation du site
Pour activer le site :
```bash
sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
```
Puis redémarrer Nginx :
```bash
sudo systemctl restart nginx
```

Le site est maintenant disponible en HTTPS.

---

Nginx permet de mettre en place rapidement une configuration HTTPS robuste utilisant des certificats X.509. Ces étapes constituent la base d’une installation sécurisée pour un serveur web moderne.
