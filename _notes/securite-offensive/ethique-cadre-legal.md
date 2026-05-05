---
layout: default
title: Éthique et cadre légal
parent: Sécurité offensive
nav_order: 1
has_toc: false
---

# Éthique et cadre légal  

La sécurité offensive est un domaine plutôt paradoxal : elle utilise les mêmes techniques que les attaquants... mais dans une intention radicalement différente.

Comprendre une faille implique souvent de reproduire le geste qui permettrait de l’exploiter. Interagir avec un système comme un attaquant, tester ses limites, manipuler ses entrées, détourner ses mécanismes, etc; toutes ces actions peuvent, selon le contexte, relever soit d’une démarche légitime, soit d’une activité illégale.

C’est pourquoi il est crucial de bien définir le cadre éthique et légal qui encadre toute pratique de sécurité offensive.

---

## Une question de contexte  

Contrairement à ce que l’on pourrait croire, les outils et les techniques utilisés en cybersécurité offensive ne sont pas nécessairement illégaux. Des outils comme **nmap**, **Burp Suite**, **sqlmap** ou **Metasploit** sont librement accessibles et largement utilisés par des professionnels de la sécurité.

Ce qui détermine la légitimité d’une action, ce n’est pas la technique utilisée, mais **le contexte dans lequel elle est appliquée**.

Une même action peut être :

- légitime, si elle est effectuée avec autorisation dans un environnement contrôlé  
- illégale, si elle est réalisée sans consentement sur un système réel  

{: .remarque}
> *Un test autorisé est une activité de sécurité.*  
> *Une intrusion non autorisée est une infraction.*  

---

## Responsabilité individuelle  

L’apprentissage de la sécurité offensive implique également une responsabilité personnelle. Les connaissances acquises dans ce domaine peuvent être utilisées à des fins très différentes. Elles peuvent contribuer à sécuriser des systèmes, protéger des organisations et améliorer la qualité des logiciels... ou, à l’inverse, être utilisées de manière malveillante.

{: .warning}
> Les techniques de sécurité offensive ne doivent donc être utilisées que dans un contexte autorisé.  

Cette responsabilité s’applique à plusieurs niveaux :

- respecter les lois en vigueur  
- respecter les systèmes et les données  
- ne jamais tester une cible sans autorisation explicite  
- limiter les tests aux environnements prévus à cet effet  

Dans un contexte professionnel, ces principes prennent souvent la forme de contrats, ou, au minimum, de mandats et d'environnements bien définis. Dans un contexte académique, ils reposent sur le respect des consignes et du cadre du cours.

---

## Types de *hackers*  

Afin de mieux comprendre les différentes postures possibles en cybersécurité, on utilise souvent une classification basée sur les intentions et les pratiques des individus. Ces catégories sont souvent désignées par des termes comme *white hat*, *black hat* ou *grey hat*.

### White hat  

Les ***white hat hackers*** (ou *hackers* éthiques) utilisent leurs compétences pour améliorer la sécurité des systèmes. Ils interviennent généralement dans un cadre autorisé, avec un objectif clair : identifier des vulnérabilités afin qu’elles puissent être corrigées. Ils peuvent travailler comme :

- analystes en sécurité  
- analystes en tests d'intrusion (*pentesters*)  
- chercheurs en sécurité  
- participants à des programmes de *bug bounty*  


### Black hat  

Les ***black hat hackers*** agissent dans un but malveillant ou illégal. Leurs actions visent souvent à :

- voler des données  
- compromettre des systèmes  
- générer un gain financier  
- perturber des services  

Contrairement aux ***white hats***, ils opèrent sans autorisation et sans cadre légal.

### Grey hat  

Les ***grey hat hackers*** se situent entre les deux extrêmes. Ils peuvent identifier des vulnérabilités sans autorisation préalable, mais sans intention malveillante directe. Dans certains cas, ils divulguent ces failles aux organisations concernées, parfois sans suivre un cadre formel.

Cependant, même en l’absence d’intention malveillante, leurs actions peuvent rester problématiques d’un point de vue légal.


### Autres profils  

Au-delà de cette classification traditionnelle, plusieurs autres types de profils existent :

- ***Script kiddie*** : utilise des outils sans comprendre en profondeur leur fonctionnement  
- ***Hacktivist*** : agit pour défendre une cause idéologique ou politique  
- **Acteur étatique** : opère dans le cadre d’opérations stratégiques, souvent à grande échelle

Ces distinctions permettent de comprendre que la cybersécurité ne repose pas uniquement sur des compétences techniques, mais aussi sur des intentions, des motivations et des contextes variés.


## Organisation des équipes en cybersécurité  

Dans un contexte professionnel, les activités liées à la sécurité offensive et défensive sont souvent structurées en équipes distinctes.


### Red Team  

La **Red Team** adopte une posture offensive.

Son objectif est de simuler des attaques réalistes afin d’évaluer la capacité d’un système ou d’une organisation à résister à une intrusion.

Les membres d’une Red Team :

- utilisent des techniques offensives réelles  
- reproduisent le comportement d’un attaquant  
- cherchent à identifier des failles exploitables  


### Blue Team  

La **Blue Team** se concentre sur la défense.

Elle est responsable de :

- détecter les attaques  
- analyser les incidents  
- mettre en place des mécanismes de protection  
- renforcer la sécurité des systèmes  


### Purple Team  

La **Purple Team** vise à faciliter la collaboration entre Red Team et Blue Team.

Plutôt que de fonctionner en opposition, ces équipes travaillent ensemble afin de :

- partager les connaissances  
- améliorer les défenses  
- valider les correctifs  

{: .highlight}
> Dans le contexte red/blue/purple team en entreprise, attaquer et défendre ne sont pas des rôles opposés, mais complémentaires; on cherche à protéger l'entreprise et ses environnements applicatifs.  

---

## Conclusion  

La sécurité offensive ne peut exister sans un cadre éthique solide. Elle repose sur une frontière claire entre ce qui est permis et ce qui ne l’est pas, entre l’apprentissage et l’abus, entre la compréhension et l’exploitation malveillante. Comprendre cette distinction est essentiel pour toute personne souhaitant évoluer dans le domaine de la cybersécurité.


{: .highlight}
> Ce n’est pas l'action qui est bonne ou mauvaise en tant que tel, mais plutôt le contexte et les autorisations qui encadrent cette action.
