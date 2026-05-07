---
layout: default
title: hydra
parent: Étapes d'une attaque
nav_order: 6
---

# hydra  

L’outil **Hydra** est un outil d’attaque par force brute utilisé pour tester automatiquement un grand nombre de combinaisons d’identifiants et de mots de passe.Contrairement à une attaque ciblée exploitant une vulnérabilité logique, Hydra repose sur un principe beaucoup plus simple : il est possible que le mot de passe utilisé soit tout simplement faible.

Dans de nombreux systèmes, la sécurité ne repose pas uniquement sur la robustesse du code, mais aussi sur le comportement des utilisateurs. Un mot de passe trop simple ou trop commun peut suffire à compromettre un système, même en l’absence de vulnérabilité technique.

---

## Objectif  

L’objectif principal de Hydra est de :

- tester automatiquement des combinaisons identifiant / mot de passe  
- identifier des comptes vulnérables  
- démontrer la faiblesse d’un mécanisme d’authentification  


Hydra est particulièrement efficace lorsque :

- les mots de passe sont faibles  
- aucune limitation de tentative n’est en place (ex.: fail2ban) 
- aucun mécanisme de protection (captcha, MFA, etc.) n’est actif

{: .highlight}
> Hydra n’exploite pas une faille technique complexe: il exploite une faiblesse humaine, soit l'utilisation d'un mot de passe faible.  

---

## Quand l’utiliser ?  

Hydra est utilisé lorsqu’un système repose sur une authentification.

Après avoir...

- identifié une cible  
- analysé une requête de connexion (ex.: avec Burp Suite)  

... on peut tester si cette authentification est robuste. Dans le cycle d’une attaque, Hydra correspond à la phase **Automatiser**.

---

## Fonctionnement  

Hydra fonctionne en envoyant un grand nombre de requêtes d’authentification à un service. Pour chaque tentative :

- un identifiant est sélectionné  
- un mot de passe est testé  
- la réponse du serveur est analysée  

Hydra détecte un succès lorsqu’une réponse diffère de celle d’un échec.

---

## Structure de la commande hydra  

### Syntaxe générale

```text
hydra [OPTIONS] <CIBLE> <MODULE>
```

### Synopsis

```text
hydra
│
├─ IDENTIFIANT
│   ├─ -l          → utilisateur unique
│   └─ -L          → liste d’utilisateurs
│
├─ MOTS DE PASSE
│   ├─ -p          → mot de passe unique
│   └─ -P          → liste de mots de passe
│
├─ CIBLE
│   ├─ adresse IP
│   └─ URL
│
└─ MODULE
    ├─ ssh
    ├─ ftp
    ├─ http-get
    └─ http-post-form
```

### Lecture de la structure  

Une commande Hydra se construit en combinant :

- un ou plusieurs utilisateurs
- une liste de mots de passe  
- une cible  
- un type de service  


**Exemple** :

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ssh
```

**Lecture** :

- `-l admin` → utilisateur ciblé  
- `-P passwords.txt` → liste de mots de passe  
- `192.168.1.10` → cible  
- `ssh` → service ciblé  


{: .highlight}
Hydra teste toutes les combinaisons possibles.

---

## Liens utiles
- Documentation officielle : [https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)
- Documentation Kali : [https://www.kali.org/tools/hydra/](https://www.kali.org/tools/hydra/)
