---
layout: default
title: A04:2025 — Cryptographic Failures
parent: OWASP Top 10 (2025)
nav_order: 4
has_toc: true
---

# A04:2025 — Cryptographic Failures (Défaillances cryptographiques)

## Description

Les défaillances cryptographiques recouvrent l’ensemble des erreurs de **conception, de configuration ou d’implémentation** des mécanismes cryptographiques : absence de chiffrement là où il est nécessaire, algorithmes ou protocoles **obsolètes**, clés **faibles** ou **exposées**, génération de nombres aléatoires **non cryptographiquement sûre**, validation **incorrecte** des certificats, vecteurs d’initialisation (IV) inadaptés, modes d’opération **dangereux** (ex. ECB), ou encore utilisation d’un **mot de passe brut** comme clé sans KDF. Ces défaillances mènent typiquement à la **divulgation de données sensibles** ou à la **compromission** de systèmes. 

**Prévalence 2025.** La catégorie descend à la **4e position** (était #2 en 2021). Les données indiquent qu’en moyenne **3,80 %** des applications présentent **≥ 1** des **32 CWE** rattachées à A04. Les CWE récurrentes incluent : **CWE‑327 (algorithme cassé/risqué)**, **CWE‑331 (entropie insuffisante)**, **CWE‑1241 (algorithme prédictible pour RNG)**, **CWE‑338 (PRNG cryptographiquement faible)**. citeturn5search13turn5search20

Au‑delà du transport (TLS), il faut décider **quelles données** nécessitent un **chiffrement au repos** et quand prévoir un **chiffrement applicatif** en sus du transport (p. ex. pour secrets, PII/PHI, cartes de paiement, secrets d’affaires). Les plateformes modernes facilitent l’adoption : instructions CPU pour accélérer le chiffrement (ex. AES‑NI), gestion de certificats simplifiée (ex. ACME/Let’s Encrypt) et services cloud intégrés ; cela n’exonère toutefois pas de **valider la configuration** (protocoles, versions, suites, certificats, HSTS/CSP, etc.). 

---

## Comment se protéger

1. **Chiffrer systématiquement en transit**  
   Forcer TLS à jour (versions/protocoles/cipher suites sûrs), **vérifier la chaîne de confiance** et les attributs du certificat (nom d’hôte, validité). Interdire les retours au clair et bloquer les downgrades. 

2. **Protéger les données sensibles au repos**  
   Identifier les données nécessitant un chiffrement au repos et appliquer des schémas éprouvés (AEAD quand pertinent). **Éviter ECB**, choisir des modes/authentifications adaptés (ex. GCM/CCM), gérer **IV/nonce** correctement (aléatoire/unique selon le mode). 

3. **Gestion des clés**  
   Générer des **clés fortes**, **ne jamais** les commiter dans les dépôts, assurer **rotation** et **segmentation** (HSM/KMS quand possible). Valider la **longueur** et l’**usage** des clés par algorithme. 

4. **Aléa cryptographique**  
   Utiliser uniquement des **générateurs cryptographiquement sûrs** (CSPRNG) et **suffisamment d’entropie** ; bannir les PRNG prévisibles pour la génération de clés, IV, tokens, liens de réinitialisation, etc. 

5. **Mots de passe et dérivation de clés**  
   **Ne jamais** utiliser un mot de passe brut comme clé ; employer un **KDF** (p. ex. Argon2, scrypt, PBKDF2 avec paramètres robustes), saler et itérer correctement. 

6. **Algorithmes et protocoles**  
   Remplacer les **algorithmes/devises obsolètes** (p. ex. RC4/MD5/SHA‑1) par des alternatives robustes et standardisées ; surveiller l’obsolescence et migrer au besoin. 

7. **Hygiène de configuration & durcissement**  
   Interdire les suites faibles, activer les **en‑têtes** et **directives** de sécurité pertinents (p. ex. HSTS), valider les bibliothèques et versions de TLS côté serveur comme côté client. Tester la configuration (intégration CI/CD). 

---

## Exemples d'attaques

### Scénario 1 — Validation de certificat incomplète (MITM)
Le client **n’effectue pas** la validation complète du certificat (ex. **mismatch du nom d’hôte**). Un attaquant en position d’homme‑du‑milieu présente un certificat non conforme mais accepté ; le trafic est **décrypté** et **altéré**.  
**Causes** : validation TLS incomplète, chaînes de confiance non vérifiées. 

### Scénario 2 — Jeton prévisible à cause d’un PRNG faible
Un **PRNG non cryptographique** sert à générer des **tokens de session**. L’attaquant, en observant quelques sorties, **prédit** la séquence et **vole** des sessions.  
**Causes** : **CWE‑338/CWE‑331**, absence de CSPRNG et d’entropie suffisante. 

### Scénario 3 — Mode ECB et IV réutilisé
Des données sensibles sont chiffrées en **ECB** ; des **motifs** apparaissent et trahissent la structure des données. Ailleurs, un schéma CBC réutilise le **même IV**, facilitant des attaques **connues‑clair** et la **corrélation** entre messages.  
**Causes** : choix de mode inadapté, **IV non unique**. 

### Scénario 4 — Mots de passe mal stockés
Les mots de passe sont **hachés sans KDF** moderne (ex. **MD5** sans sel). Après une fuite, l’attaquant **craque** rapidement la base et compromet d’autres systèmes par **recyclage** d’identifiants.  
**Causes** : absence de KDF résistant (Argon2/scrypt/PBKDF2), pas de sel/paramètres. 

---

## CWE liées

Exemples de CWE emblématiques dans A04 :

- **CWE‑327 — Use of a Broken or Risky Cryptographic Algorithm**   
- **CWE‑331 — Insufficient Entropy**   
- **CWE‑1241 — Use of Predictable Algorithm in RNG**   
- **CWE‑338 — Use of Cryptographically Weak PRNG**   

Pour la **liste complète (32 CWE)** et les métriques (incidence/couverture/exploit/impact), voir la page officielle A04. 

---

## Liens utiles

- **Page officielle OWASP — A04:2025 Cryptographic Failures**  
  [https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/](https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/)

- **OWASP Top 10:2025 — Page principale / contexte**  
  [https://owasp.org/Top10/2025/](https://owasp.org/Top10/2025/)

---

## Attribution

Contenu dérivé à partir des documents OWASP (licence **CC BY‑SA 4.0**).  
Informations de licence : [https://owasp.org/Top10/2025/0x01_2025-About_OWASP/](https://owasp.org/Top10/2025/0x01_2025-About_OWASP/)