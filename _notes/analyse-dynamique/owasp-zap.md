---
layout: default
title: OWASP ZAP
parent: Analyse dynamique
nav_order: 3
has_toc: false
---


# OWASP ZAP

**OWASP Zed Attack Proxy (ZAP)** est un outil open source de test de sécurité applicative, maintenu par la fondation OWASP. ZAP est conçu à l’origine comme un proxy intercepteur(*intercepting proxy*), permettant d’observer, modifier et rejouer les échanges entre un client et une application cible.

Avec le temps, ZAP a évolué pour devenir une plateforme complète d'analyse dynamique de sécurité (*DAST*), intégrant des fonctionnalités de découverte, d’analyse passive, d’analyse active, de fuzzing et d’automatisation.

ZAP se distingue par l'étendue de ses fonctionnalités, son orientation gratuite et *open-source* et sa forte intégration avec les pratiques DevSecOps.

---

## Découverte de la surface d’attaque (*Spider*)

La phase de spidering constitue le point d’entrée de toute analyse DAST avec ZAP. Le spider explore automatiquement les ressources accessibles afin de construire une cartographie de l’application.

### *Spider* traditionnel

Le spider traditionnel analyse les réponses HTTP et extrait les liens présents dans le HTML, les formulaires, les fichiers robots.txt ou sitemap.xml. Il est adapté aux applications web classiques.

### *Spider* AJAX

L’*AJAX Spider* utilise un navigateur sans interface pour exécuter le JavaScript et découvrir des routes dynamiques, ce qui le rend plus efficace sur les applications modernes de type SPA.

### *Spider* client

Le *Client Spider* s’appuie sur une extension navigateur et un accès direct au DOM. Il vise à devenir la méthode privilégiée pour explorer les applications web modernes.

---

## Analyse passive (*passive scan*)

L’analyse passive inspecte les requêtes et réponses sans modifier le trafic. Elle est non intrusive et généralement sûre pour les environnements sensibles. Elle permet notamment d’identifier des problèmes de configuration, des en-têtes de sécurité manquants, des cookies mal protégés ou des fuites d’informations.

---

## Analyse active (*active scan*)

L’analyse active consiste à envoyer des requêtes malveillantes afin de tester la réaction de l’application face à des attaques connues. Cette phase est offensive et doit être menée uniquement sur des cibles autorisées.

### Politiques d'analyse (*scan policies*)

Les politiques d'analyse définissent les règles exécutées, l’intensité des attaques et les seuils d’alerte. Elles permettent d’adapter l’analyse aux besoins spécifiques de l’application. Voici les principales politiques prédéfinites dans ZAP : 


| Politique | Description | Usage typique |
|------------|-------------------|---------------|
| **Default Policy** | Politique de scan par défaut de ZAP. Active toutes les règles installées avec une agressivité modérée offrant un bon compromis entre couverture, durée et bruit. | Premier scan DAST, tests généraux, contexte pédagogique |
| **Developer CICD Policy** | Politique légère orientée CI/CD développeur. Cible les vulnérabilités à fort impact avec un nombre limité de requêtes pour un retour rapide. | Pipelines CI côté dev, feedback rapide |
| **Developer Standard Policy** | Politique développeur intermédiaire offrant une couverture plus large que la CICD tout en conservant un temps d’exécution raisonnable. | Tests réguliers en environnement de développement |
| **Developer Full Policy** | Politique développeur la plus complète. Active un grand nombre de règles applicatives au prix d’un scan plus long et plus bruyant. | Audits approfondis en développement |
| **QA CICD Policy** | Politique CI/CD adaptée aux environnements QA. Plus de couverture que les policies développeur tout en restant rapide. | Pipelines CI en QA ou staging |
| **QA Standard Policy** | Politique QA équilibrée visant une couverture accrue des vulnérabilités applicatives et serveur. | Tests de sécurité en QA avant livraison |
| **QA Full Policy** | Politique QA la plus exhaustive. Inclut règles applicatives, environnementales et serveur. Scan agressif et long. | Pré‑production, audits encadrés |
| **API Policy** | Politique spécialisée pour les API. Désactive les règles orientées interface utilisateur et cible les vulnérabilités propres aux services REST/JSON/XML. | Tests DAST sur API et microservices |
| **Penetration Tester Policy** | Politique la plus agressive. Active toutes les règles d’active scan disponibles afin de maximiser la surface testée. | Tests d’intrusion automatisés, audits formels |


### Limites

L’analyse active ne permet pas de détecter les failles de logique métier ou les contrôles d’accès complexes, qui nécessitent des tests manuels.

**Pour plus de détails** :

- Active Scan : [https://www.zaproxy.org/docs/desktop/start/features/ascan/](https://www.zaproxy.org/docs/desktop/start/features/ascan/)
- Scan Policy : [https://www.zaproxy.org/docs/desktop/start/features/scanpolicy/](https://www.zaproxy.org/docs/desktop/start/features/scanpolicy/)

---

## Fuzzing

ZAP intègre un fuzzer permettant d’injecter de grandes quantités de payloads dans une requête ciblée afin de tester la robustesse des paramètres.

**Pour plus de détails** :
- [https://www.zaproxy.org/docs/desktop/addons/fuzzer/](https://www.zaproxy.org/docs/desktop/addons/fuzzer/)

---

## Configuration de l’authentification

Dans la majorité des applications modernes, une part importante de la surface d’attaque n’est accessible qu’aux utilisateurs authentifiés. Sans configuration explicite de l’authentification, une analyse active avec OWASP ZAP se limite souvent aux pages publiques, ce qui réduit fortement sa pertinence.

OWASP ZAP propose un mécanisme d’authentification flexible basé sur les **Contextes**, permettant de définir le périmètre fonctionnel, la méthode d’authentification, la gestion de session et les utilisateurs utilisés lors des scans.

**Méthodes d’authentification prises en charge**

ZAP prend en charge plusieurs mécanismes courants :
- authentification par formulaire,
- authentification HTTP (Basic, Digest, NTLM),
- authentification JSON pour les API REST,
- authentification par script personnalisé,
- authentification automatisée via navigateur.

**Contextes, utilisateurs et sessions**

Le contexte définit les URLs incluses et exclues, la méthode d’authentification et la gestion de session. Une fois configuré, ZAP est capable de maintenir automatiquement une session valide et de se réauthentifier si nécessaire.


**Authentication Decision Tree**

OWASP fournit un arbre de décision officiel pour choisir rapidement la méthode d’authentification la plus appropriée selon le type d’application.

**Pour plus de détails** :
- Authentification : [https://www.zaproxy.org/docs/desktop/start/features/authentication/](https://www.zaproxy.org/docs/desktop/start/features/authentication/)
- Contexts : [https://www.zaproxy.org/docs/desktop/start/features/contexts/](https://www.zaproxy.org/docs/desktop/start/features/contexts/)
- Session Management : [https://www.zaproxy.org/docs/desktop/start/features/session-management/](https://www.zaproxy.org/docs/desktop/start/features/session-management/)
- Decision Tree : [https://www.zaproxy.org/docs/authentication/decision-tree/](https://www.zaproxy.org/docs/authentication/decision-tree/)

---

## Fonctionnalités d'automatisation (*Automation Framework*)

Toutes les étapes décrites dans les sections ci-dessus peuvent évidemment être effectuées manuellement. Cependant, ce qui rend ZAP particulièrement utile dans les une approche DevSecOps est sa capacité d'automatisation. Ainsi, toutes les étapes effectuées manuellement peuvent également être écrites de façon déclarative dans un plan d'automatisation. 

Le *Automation Framework* permet donc de définir l’ensemble d’un scan ZAP via un fichier YAML versionnable, facilitant l’intégration de ZAP dans les pipelines CI/CD.

**Authentification**

Dans un plan d’automatisation, l’authentification est définie dans la section `env` du fichier YAML.

**Pour plus de détails** :

- Automation framework : [https://www.zaproxy.org/docs/automate/automation-framework/](https://www.zaproxy.org/docs/automate/automation-framework/)
- Automatisation avec authentification : [https://www.zaproxy.org/docs/desktop/addons/automation-framework/authentication/](https://www.zaproxy.org/docs/desktop/addons/automation-framework/authentication/)

---

## Conclusion

OWASP ZAP est une plateforme DAST complète couvrant la découverte, l’analyse passive, l’analyse active, le fuzzing et l’automatisation. Utilisé correctement, il constitue un pilier essentiel d’une stratégie DevSecOps moderne.
