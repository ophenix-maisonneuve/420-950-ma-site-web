---
layout: default
title: "Travail pratique 1"
nav_order: 1
published: false
---

# Travail pratique 1 : Des soucis chez Oups Technologies!

## Mise en situation
Vous débutez un stage chez **Oups Technologies**, une petite entreprise de développement logiciel reconnue pour son slogan :

> *Développer vite. Casser souvent.*

Le portail interne de l’entreprise, accessible à l’adresse  
**`http://portail.oups.tech.test`**  
permet aux employé(e)s de se connecter pour réserver des locaux et des salles de réunion.

Un incident vient toutefois de se produire : votre collègue **Gilles Sansoucis** signale que son compte a été utilisé à son insu pour se connecter à différents serveurs de l'entreprise; on soupçonne donc que son mot de passe pourrait lui avoir été volé. Gilles mentionne que le seul endroit où il a entré son mot de passe depuis le début des incidents est sur le portail de réservation...

Votre rôle : **reproduire l’incident**, comprendre **pourquoi** il s’est produit, puis **corriger** entièrement la situation pour éviter que cela ne se reproduise.

---

## Objectifs pédagogiques
- Comprendre les risques liés à l’utilisation non-sécuritaire d'un site web avec authentification 
- Analyser une fuite de mot de passe au moyen d’un **sniffing réseau**  
- Diagnostiquer un problème avec un certificat X.509 existant
- Générer un certificat X.509 valide
- Générer une clé privée, un CSR et un certificat signé par une autorité de certification 
- Sécuriser un serveur web **nginx** à l'aide de HTTPS.

---

## Préparation
*Mentionner ici les étapes pour ajouter portail.oups.tech.test dans le host file (Linux et Windows)

## Tâches à réaliser
### 1. Reproduire l’incident
À partir de votre environnement applicatif...

1. Connectez-vous au portail de réservation à l'une des adresses suivantes
   - [https://portail.oups.tech.test](https://portail.oups.tech.test)
   - [http://portail.oups.tech.test](http://portail.oups.tech.test)


2. Connectez‑vous avec le compte administrateur
   - `admin / Oups123!`  

3. À l’aide de Wireshark ou `tcpdump`, **capturez** la requête HTTP contenant les identifiants.  
4. Montrez clairement que le mot de passe circule **en clair**.

> Les preuves doivent inclure une capture d’écran ou un extrait réel de la requête interceptée.

---

### 2. Diagnostic du problème HTTPS
- Tester `https://portail.oups.tech.test`
- Identifier l’erreur affichée par le navigateur
- Expliquer pourquoi le certificat présenté est invalide (SAN manquant)

---

### 3. Génération d’un certificat **valide**
À partir de l’AC intermédiaire fournie :

1. Générez une **clé privée** (ECDSA ou RSA).  
2. Générez un **CSR** contenant :  
   - CN = `portail.oups.tech.test`  
   - SAN = `DNS:portail.oups.tech.test`  
3. Signez le CSR avec l’**AC intermédiaire**.  
4. Créez une **fullchain** (certificat + intermédiaire).

> Le certificat installé sur la VM est volontairement brisé. Vous devez produire une version **corrigée**.

---

### 4. Configuration sécurisée de NGINX
Vous devez :
- Installer le certificat corrigé  
- Installer la **fullchain**  
- Forcer la redirection HTTP → HTTPS  
- Activer **TLS 1.2+ seulement**  
- Ajouter les en‑têtes de sécurité minimales :
  - `Strict-Transport-Security` (HSTS)  
  - `X-Content-Type-Options: nosniff`  
  - `X-Frame-Options: SAMEORIGIN`  

---

### 5. Validation finale
Vous devez démontrer que :
- HTTP redirige vers HTTPS  
- HTTPS fonctionne sans erreur  
- La chaîne complète est présentée (server + intermédiaire)  
- Le mot de passe **n’est plus** interceptable  
- `openssl s_client` affiche bien la chaîne

Exemples utiles :
```bash
curl -vk http://portail.oups.tech.test/
curl -vk https://portail.oups.tech.test/
openssl s_client -connect portail.oups.tech.test:443 -servername portail.oups.tech.test -showcerts
```

---

## Évaluation
| Critère | Points |
|--------|--------|
| Reproduction de l’incident (preuve du mot de passe en clair) | 5 |
| Analyse de l’erreur HTTPS | 5 |
| Génération clé + CSR + certificat signé | 5 |
| Construction du fullchain et installation | 5 |
| Configuration NGINX (HTTPS, redirection, TLS, HSTS, headers) | 5 |
| Validation finale (captures, commandes, explication) | 5 |
| **Total** | **30 points (15%)** |

---

## À remettre
- Un document PDF ou Markdown contenant :
  - Preuve de la fuite HTTP  
  - Diagnostic de l’erreur HTTPS  
  - Commandes utilisées pour la génération du certificat  
  - Configuration finale de NGINX  
  - Preuves de validation (captures, curl, s_client, etc.)
- Le travail est **individuel**  
- À remettre sur **Léa (Omnivox)**  

---

Bonne chance ! 🔐🔥