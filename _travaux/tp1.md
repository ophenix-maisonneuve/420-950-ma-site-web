---
layout: default
title: "Travail pratique 1"
nav_order: 1
published: true
---

# Travail pratique 1 : Des soucis chez Oups Technologies!

## Mise en situation
Vous débutez un stage chez **Oups Technologies**, une petite entreprise de développement logiciel reconnue pour son slogan :

> *Développer vite. Casser souvent.*

Le portail de l’entreprise, accessible à l’adresse...

[https://portail.oups.tech.test](https://portail.oups.tech.test)

... permet aux employé(e)s de se connecter pour réserver des locaux et des salles de réunion.

Un incident vient toutefois de se produire : votre collègue **Gilles Sansoucis** signale que son compte a été utilisé à son insu pour se connecter à différents serveurs de l'entreprise; on soupçonne donc que son mot de passe pourrait lui avoir été volé. Gilles mentionne que le seul endroit où il a entré son mot de passe depuis le début des incidents est sur le portail de réservation. Il n'est pas très familier avec les différences entre HTTP et HTTPS, mais il est certain d'avoir utilisé le site parfois en HTTP et parfois en HTTPS...

Votre rôle : **reproduire l’incident**, comprendre **pourquoi** il s’est produit, puis **corriger** entièrement la situation pour éviter qu'elle ne se reproduise.

---

## Objectifs pédagogiques
- Comprendre les risques liés à l’utilisation **non-sécuritaire** d'un site web avec authentification 
- Analyser une fuite de mot de passe au moyen d’une **capture réseau** 
- Diagnostiquer un problème avec un **certificat X.509** existant
- Générer un **certificat X.509** valide
- Générer une **clé privée**, une **requête de signature (*CSR*)** et un **certificat** signé par une **autorité de certification** 
- Sécuriser un serveur web **nginx** à l'aide de **HTTPS**.

---

## Préparation

Afin de simuler une résolution DNS pour l'adresse `portail.oups.tech.test`, il est nécessaire d'y associer l'adresse IP de la machine virtuelle du TP 1. Pour ce faire, voici les étapes à effectuer dans votre environnement applicatif :

### Linux

Sous Linux, le fichier *hosts* se trouve dans `/etc/hosts`

1. Ouvrir un terminal.
2. Éditer le fichier avec `sudo` (utilise l’éditeur de ton choix, ici *nano*) :

```bash
sudo nano /etc/hosts
```

3. Ajouter une ligne au bas du fichier :

```text
192.168.50.10   portail.oups.tech.test  //remplacez par l'IP de votre VM
```

4. Sauvegarder et fermer l’éditeur.

### Windows

Sous Windows, le fichier *hosts* se trouve dans `C:\Windows\System32\drivers\etc\hosts`

1. Ouvrir **Bloc‑notes** en tant qu’administrateur :
   - Menu Démarrer → taper *Bloc‑notes* → clic droit → *Exécuter en tant qu’administrateur*
2. Ouvrir le fichier :

```
C:\Windows\System32\drivers\etc\hosts
```

3. Ajouter la ligne suivante :

```text
192.168.50.10   portail.oups.tech.test  //remplacez par l'IP de votre VM
```

4. Enregistrer.

---

## Tâches à réaliser

### 1. Reproduction de l’incident
À partir de votre environnement applicatif, effectuez les opérations suivantes

1. Connectez-vous au portail de réservation :
   - [https://portail.oups.tech.test](https://portail.oups.tech.test)
   - Utilisez le compte `admin / Oups123!` pour vous connecter
   {: .astuce}
   > Cette adresse est-elle la seule qui permet d'afficher le site du portail ?

2. À l’aide de **Wireshark** ou **tcpdump**, effectuez une capture qui prouve qu'il est possible de récupérer les identifiants (nom d'utilisateur et mote de passe) en clair.

3. **Questions**
   1. Quelle est l'erreur affichée par le navigateur lors du chargement du portail [https://portail.oups.tech.test](https://portail.oups.tech.test) ?
   2. Comment expliquez-vous que le mot de passe de certains utilisateurs ait circulé "en clair", sans chiffrement ?

---

### 2. Diagnostic du problème HTTPS
1. Récupérez le certificat utilisé par le portail de réservation :
   - [https://portail.oups.tech.test](https://portail.oups.tech.test)

2. Inspectez le cerficiat X.509

3. **Questions**
   1. Quelle est la période de validité du certificat ? Est-il présentement valide ?
   1. Quel est le sujet (*Subject*) du certificat ? Correspond-il au nom de domaine du portail ?
   1. Quel est l'émetteur (*Issuer*) du certificat ? S'agit-il d'une autorité de certification valide ?
   1. Expliquez pourquoi, malgré tout, le certificat est refusé par le navigateur.

---

### 3. Génération d’un certificat X.509 valide
À partir de l’autorité de certifiation de **Oups Technologies** fournie :

1. Générez une **clé privée** (ECDSA ou RSA).  
2. Générez une **requête de signature (*CSR*)** valide pour le domaine `portail.oups.tech.test`
3. Signez le ***CSR*** avec l’**autorité de certification**.
   - Le certificat de l'autorité de certification de Oups Technologies est disponible [ici](../assets/files/tp1/oups_ca.crt)
   - La clé privée de l'autorité de certification de Oups Technologies est disponible [ici](../assets/files/tp1/oups_ca.key)
4. Créez une chaîne de certificats en concaténant le certificat signé avec le certificat de l'autorité de certification.
5. **Questions**
   1. Quelle commande avez-vous utilisée pour **générer la clé privée** ?
   1. Quelle commande avez-vous utilisée pour produire la **requête de signature (*CSR*)** ?
   1. Quelle commande avez-vous utilisée pour **signer la requête de signature** ?

---

### 4. Configuration sécurisée du portail (NGINX)
1. Copiez la **chaîne de certificats** et la **clé privée du serveur** sur le serveur web
2. Corrigez la configuration du site web (`/etc/nginx/sites-available/portail.oups.tech.test.conf`) en vous assurant que...
   - Le serveur utilise votre **nouveau certificat**
   - Il est désormais **impossible de se connecter en clair (HTTP)** au portail de réservation
   - Seules les versions de **TLS 1.2 et plus** sont acceptées
3. **Questions**
   1. Pourquoi est-il nécessaire que le portail fournisse la **chaîne de certificats** complète (certificat du serveur + certificat de l'autorité de certification de Oups Technologies) plutôt que seulement le certificat du serveur ? 

---

### 5. Validation finale
À l'aide de captures d'écran, démontrez que :
- Il n'est plus possible de se connecter au portail de réservation en clair (HTTP)
- HTTPS fonctionne maintenant sans erreur dans le navigateur
- La chaîne complète est présentée au navigateur par le serveur (serveur + autorité de certification)
- Le mot de passe n’est plus interceptable

---

## Évaluation
| Critère | Points |
|--------|--------|
| Section 1 : Reproduction de l’incident | 15 |
| Section 2 : Diagnostic du problème HTTPS | 20 |
| Section 3 : Génération du certificat valide | 25 |
| Section 4 : Configuration sécurisée du portail | 20 |
| Section 5 : Validations | 20 |
| **Total** | **100 points (15% de la note finale)** |

---

## À remettre
- Un fichier **.zip** contenant :
  - Les réponses aux **questions** des **sections 1 à 5** (dans un document PDF, Word, LibreOffice ou Markdown)
  - **Section 1** : La capture **Wireshark** ou **tcpdump** montrant les identifiants en clair (non chiffrés)
  - **Section 2** : Le **certificat X.509** récupéré
  - **Section 3** : Le **certificat X.509** valide généré ainsi que la **chaîne de certificats** (certificat du serveur + certificat de l'autorité de certification)
  - **Section 4** : La **configuration nginx** (`/etc/nginx/sites-available/portail.oups.tech.test.conf`) corrigée.
  - **Section 5** : Les captures d'écrans demandées
- Le travail est à remettre sur **Léa (Omnivox)** dans la section **Travaux - Énoncés et remises**
- Le travail peut être fait en équipes de **deux personnes** (une seule remise pour l’équipe)
- Le travail est à remettre au plus tard le **2 avril 2026** en fin de journée.

---

À vous de jouer ! 🔐🔥