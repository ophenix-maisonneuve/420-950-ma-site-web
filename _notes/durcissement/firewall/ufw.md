---
layout: default
title: "UFW"
parent: "Pare-feu Linux"
nav_order: 2
---

# UFW

**UFW** (*Uncomplicated Firewall*) est un utilitaire bâti sur iptables qui en facilite l'utilisation.

Objectifs :
- simplicité
- sécurité par défaut
- lisibilité

---

## Installation

```bash
sudo apt install ufw
```

---

## Commandes UFW

Une commande UFW prend la forme suivante :
```bash
ufw <ACTION> [DIRECTION] [CRITÈRES]
```

où

```

ufw
│
├─ ACTION
│   ├─ allow
│   ├─ deny
│   ├─ reject
│   ├─ delete
│
├─ DIRECTION (optionnelle)
│   ├─ in
│   └─ out
│
└─ CRITÈRES (lisibles, proches du langage naturel)
    ├─ service        (ssh, http, https)
    ├─ port           (22, 80, 443)
    ├─ plage de ports (6000:6007/tcp)
    ├─ adresse IP     (from 192.168.1.100)
    ├─ sous-réseau    (from 192.168.1.0/24)
    └─ interface      (in on ens33)

```

### Modification des politiques par défaut

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

---

### Autorisation de SSH

```bash
sudo ufw allow ssh
sudo ufw allow 22
```

### Autorisation pour un serveur Web

```bash
sudo ufw allow http
sudo ufw allow https
```

---

###  Ports et plages

```bash
sudo ufw allow 6000:6007/tcp
```

---

### IP et sous-réseaux

```bash
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24 to any port 22
```

---

### Interfaces réseau

```bash
sudo ufw allow in on ens33 to any port 80
```

---

### Activation et statut

```bash
sudo ufw enable
sudo ufw status verbose
```

---

### Suppression de règles

```bash
sudo ufw status numbered
sudo ufw delete 2
```