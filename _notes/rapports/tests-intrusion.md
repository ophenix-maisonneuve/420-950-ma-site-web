---
layout: default
title: Rapport de test d'intrusion
parent: Rapports de sécurité
nav_order: 1
has_toc: true  
---

# Tests d’intrusion  

Un test d’intrusion, communément appelé *pentest* (de l'anglais *penetration testing*), est une démarche de sécurité offensive structurée visant à simuler une attaque réelle sur un système afin d’identifier et exploiter ses vulnérabilités. 

Cette approche repose sur une idée fondamentale : une vulnérabilité n’est réellement importante que si elle peut être exploitée. Ainsi, le test d'intrusion ne s’arrête pas à dire qu’une faiblesse existe; il montre comment elle peut être utilisée, et quelles en sont les conséquences.

Dans un contexte professionnel, le test d'intrusion est souvent réalisé avant une mise en production ou un livrable (ex.: une nouvelle version d'une application). Il permet d’évaluer le niveau de sécurité réel d’une application ou d’une infrastructure, d’identifier les risques critiques et de prioriser les correctifs. Il sert également de base à la prise de décision : un système est-il prêt à être mis en production, ou présente-t-il encore des risques trop importants ?

{: .astuce}
> Dans la plupart des entreprises, le test d'intrusion est réalisé par une firme externe, ce qui entraîne des coûts supplémentaires. Ainsi, bien que l'on préconise un cycle de développement où la sécurité est intégrée à chacune des étapes, il est souvent préférable d'exécuter le test d'intrusion seulement une fois que tous les mécanismes de sécurité prévus ont été intégrées. Sinon, on risque de détecter beaucoup de faux positifs (et aussi de faire grimper la facture...)

---

## Types de tests d’intrusion  

Un test d’intrusion peut prendre différentes formes selon le niveau d’information fourni au testeur. Cette distinction est essentielle, car elle influence directement la profondeur de l’analyse, le réalisme du test et les résultats obtenus.

### Test boîte noire (*Black Box*)

Dans un test **boîte noire**, le testeur n’a aucune information préalable sur le système. Il se retrouve dans la même position qu’un attaquant externe qui découvrirait la cible pour la première fois. Il ne connaît ni l’architecture, ni les technologies utilisées, ni les comptes utilisateurs existants. Toute information doit être obtenue par observation et exploration.

Ce type de test est particulièrement intéressant pour simuler des attaques réalistes provenant d’Internet. Le testeur doit effectuer une phase de reconnaissance complète, identifier les services exposés, découvrir les endpoints disponibles et comprendre progressivement le fonctionnement de l’application. 

Cependant, cette approche présente certaines limites. Elle est plus longue et certaines vulnérabilités internes peuvent passer inaperçues, simplement parce qu’elles ne sont pas accessibles depuis l’extérieur. Le test *black box* ne donne donc pas une couverture complète, mais il reflète fidèlement la réalité d’une attaque externe.

{: .highlight}
> *Le test en boîte noire permet de répondre à la question : “Que peut accomplir un attaquant qui ne sait rien du système ?”*

### Test boîte blanche (*White Box*)

Dans un test **boîte blanche**, le testeur dispose d’un accès complet au système, incluant le code source, l’architecture et parfois même des privilèges administratifs. Cette approche permet une analyse extrêmement approfondie, puisqu’aucune information n’est cachée.

Le testeur peut examiner le code pour identifier des erreurs logiques, analyser les flux de données, vérifier les contrôles de sécurité et détecter des vulnérabilités qui seraient invisibles de l’extérieur. C’est l’approche la plus exhaustive, mais aussi la moins réaliste dans le contexte d’une attaque.

La boîte blanche est souvent utilisée dans des audits de sécurité internes ou avant une mise en production critique. Il permet d’assurer une couverture maximale et de corriger des problèmes en profondeur.

{: .highlight}
> *Le test white box répond à la question : “Quelles sont toutes les failles possibles dans ce système, même les plus cachées ?”*

### Test boîte grise (*Grey Box*)

Le test **boîte grise** représente un compromis entre le réalisme du *black box* et la profondeur du *white box*. Dans ce scénario, le testeur dispose d’un accès partiel au système, par exemple sous la forme d’un compte utilisateur standard ou d’une documentation limitée.

Ce type de test simule une situation très fréquente dans le monde réel : un attaquant qui a déjà obtenu un accès initial (par hameçonnage, fuite d'identifiants, etc.) et qui cherche à étendre ses privilèges ou à explorer davantage le système.

Le test **boîte grise** est particulièrement efficace pour identifier les failles liées à la gestion des permissions, aux contrôles d’accès ou à la logique applicative. Il permet de tester ce qu’un utilisateur malveillant pourrait faire une fois connecté, ce qui est souvent plus critique que les attaques purement externes.

{: .highlight}
*Le test boîte grise répond à la question : “Que peut faire quelqu’un ayant déjà un accès partiel ?”*

---

## Étapes d’un test d’intrusion  

Un test d'intrusion utilise généralement les mêmes outils vus dans les cours de sécurité offensive, mais les utilise dans une démarche plus structurée et encadrée. Ainsi, même si les *hackers* suivent généralement (consciemment ou non) des étapes officieuses, le testeur s'assure quant à lui que sa démarche est claire, méthodique, documentée et donc reproductible. 

### 1. Reconnaissance  

La phase de reconnaissance consiste à collecter un maximum d’informations sur la cible avant toute tentative d’attaque. Cette étape est souvent sous-estimée, mais elle est cruciale : plus l’information recueillie est précise, plus les attaques ultérieures seront efficaces.

Le testeur cherche à identifier les adresses IP associées au système, les ports ouverts, les services exposés et les technologies utilisées. Cette information permet de comprendre la surface d’attaque et d’orienter les étapes suivantes.

Des outils comme **nmap** permettent de découvrir les services ouverts, tandis que **whatweb** aide à identifier les technologies utilisées par une application web. Cette phase en est essentiellement une d'observation : on tente de comprendre avant d'agir.

### 2. Énumération  

Une fois la reconnaissance terminée, le testeur passe à l’énumération. L’objectif est de découvrir les points d’entrée spécifiques du système, notamment les endpoints, les répertoires cachés et les interfaces internes.

Contrairement à la reconnaissance, qui est très large, l’énumération est ciblée. Elle consiste à explorer activement les ressources accessibles pour identifier ce qui peut être attaqué.

Des outils comme **gobuster** permettent de découvrir des routes cachées sur une application web. Cette étape est essentielle, car une vulnérabilité ne peut être exploitée que si l’on connaît son point d’accès.

### 3. Analyse  

La phase d’analyse vise à comprendre le comportement du système. Le testeur observe les requêtes envoyées, les réponses reçues et la manière dont les données sont traitées.

C’est à ce moment que des outils comme **Burp Suite** deviennent très utiles. Ils permettent d’intercepter les requêtes HTTP, de les modifier et d’observer l’impact de ces modifications. Le testeur identifie les paramètres, teste les validations et cherche des comportements inattendus.

Cette phase marque le passage de l’observation à l’interaction active avec le système.

### 4. Exploitation  

L’exploitation consiste à tirer parti des vulnérabilités identifiées. Le testeur ne se contente plus d’observer, il agit pour compromettre le système.

Cela peut inclure des injections SQL (avec **sqlmap**), des attaques par brute force (avec **hydra**) ou d’autres techniques permettant d’obtenir un accès.

L’objectif est de démontrer que la vulnérabilité a un impact réel. Par exemple, réussir à contourner une authentification ou accéder à des données sensibles.

### 5. Post‑exploitation  

Une fois l’accès obtenu, la phase de post‑exploitation permet de mesurer l’étendue de la compromission. Le testeur cherche à comprendre ce qu’un attaquant pourrait réellement faire une fois à l’intérieur du système.

Cela inclut l’exécution de commandes, l’exploration des fichiers, la récupération d’informations sensibles et, dans certains cas, l’escalade de privilèges.

Des outils comme **Metasploit** permettent d’automatiser cette phase et de démontrer des scénarios d’attaque plus avancés.

### 6. Rapport  

La phase finale est la rédaction du rapport. Elle est souvent la plus importante, car c’est elle qui permet de convertir les résultats techniques en information exploitable pour l’organisation.

Un pentest sans rapport n’a aucune valeur. Le rapport est le seul livrable qui permet de comprendre les failles, leur impact et les actions à entreprendre.

---

## Rapport de test d’intrusion  

Le rapport de pentest est un document structuré qui doit être compréhensible à la fois par des experts techniques et par des décideurs non techniques. Il constitue la base des actions correctives et des décisions stratégiques. Il existe une multitude de façons de structurer ce rapport; la prochaine section propose une structure suffisamment générique couvrant les sections les plus importantes et pouvant servir de modèle de base.

### Résumé exécutif  

Le résumé exécutif est destiné aux gestionnaires et aux décideurs. Il doit présenter une vision globale de la sécurité du système, sans entrer dans des détails techniques.
Il met en évidence les risques principaux, leur impact potentiel et les décisions à prendre. C’est souvent la seule partie du rapport lue par la direction.


{: .highlight-title}
> Exemple
>
> Le test d’intrusion réalisé sur la plateforme Juice Shop a mis en évidence plusieurs vulnérabilités critiques, notamment
> - Vulnératilité 1 + risque
> - Vulnérabilité 2 + risque
> - Vulnérabilité 3 + risque
> - ...
> - [Se limiter ici seulement aux vulnérabilités jugées critiques]
>
> L'une de ces vulnérabilités permet à un attaquant non authentifié d’obtenir un accès administrateur et de compromettre l’ensemble de l’application. Globalement, le niveau de risque est évalué comme critique. **La mise en production ne devrait pas être autorisée sans correction.**
> 
> *La liste complète des vulnérabilités identifiées est disponible dans la section *Résultats détaillés*

---

### Portée  

La portée du test précise les systèmes évalués, les limites de l’analyse et le contexte du test. Cela permet d’éviter toute ambiguïté sur ce qui a été testé ou non.

{: .highlight-title}
> Exemple
>
> Le test a été réalisé sur la plateforme Juice Shop selon les paramètres suivants :
>
> - **Environnement** : http://pentest.juiceshop.local:3000
> - **Période** : 2026-05-04 au 2026-05-08 (5 jours ouvrables)
> - **Inclusion** : Interface web, services REST exposés
> - **Exclusions** : Applications tierces utilisant la plateforme, attaques provenant de l'interne (hypothèse de périmètre sécuritaire)

### Méthodologie  

Cette section décrit l’approche utilisée pour réaliser le test. Elle inclut le type de pentest (black, grey ou white box), les étapes suivies et les outils employés. Elle permet de comprendre le contexte des résultats et d’assurer la reproductibilité.

{: .highlight-title}
> Exemple
>
> - **Approche** : boîte noire (aucun accès au code)
> - **Outils utilisés**:
>    - Reconnaissance : nmap
>    - Énumération : gobuster
>    - Analyse : Burp Suite
>    - Exploitation : sqlmap, hydra, Metasploit
>    - Post-exploitation : Metasploit

### Analyse globale  

L’analyse globale offre une vue d’ensemble des résultats. Elle met en évidence les points faibles du système, les tendances observées et les risques majeurs. Elle est plus technique que le résumé exécutif, mais moins détaillée que la prochaine section (résultats détaillés). 

{: .highlight-title}
> Exemple
>
> Globalement, les principales vulnérabilités observées peuvent être regroupées selon les catégories suivantes :
> - **Injection (SQL, script, commandes, etc)** : 6 vulnérabilités critiques ou hautes
> - **Échec d'authentification** : 2 vulnérabilités critiques ou hautes
> - **Mauvais contrôle des accès** : 2 vulnérabilités critiques ou hautes
> - **Cryptographie** : 1 vulnérabilité critique ou haute
> - Plusieurs autres vulnérabilités de niveau moyen ou inférieur

### Résultats détaillés  

Chaque vulnérabilité identifiée est décrite en détail. Cela inclut la nature de la faille, la manière dont elle a été exploitée, des preuves concrètes et une évaluation de son impact.Cette section est essentielle pour comprendre la gravité des problèmes et prioriser les corrections. Si elle est très longue, cette section peut parfois être scindée en deux : une liste sommaire des vulnérabilités les plus graves directement dans le rapport, et la liste complète en annexe.

{: .highlight-title}
> Exemple
>
> **Injection SQL `/rest/user/login`**
> Le paramètre `email` est vulnérable à une attaque par injection SQL.
> - Valeur utilisée : ' OR 1=1--
> - Résultat : accès administrateur sans authentification
> - Impact : Critique (compromission complète du système) 

### Recommandations  

Les recommandations proposent des solutions concrètes pour corriger les vulnérabilités identifiées. Elles doivent être claires, priorisées et adaptées au contexte.

{: .highlight-title}
> Exemple
>
> 1. Remplacement des requêtes SQL manuelles par l'utilisation d'un ORM et/ou de *prepared statement*
> 1. Mise en place d'une authentification utilisant JWT avec signature forte
> 1. Supplémentation de l'authentification avec une 2e source (*multi-factor authentication*)
> 1. etc.

### Conclusion  

La conclusion synthétise l’état de sécurité du système et permet de guider la décision, par exemple pour savoir s’il est prêt à être mis en production ou s’il nécessite des corrections supplémentaires.

{: .highlight-title}
> Exemple
>
> En conclusion, la plateforme contient des vulnérabilités critiques faisant courir un risque élevé à l'organisation et ses clients. En l'état actuel, elle présente un risque technique, légal et réglementaire jugé **très élevé**.

---

## Prochaines étapes

Un test d'intrusion ne marque pas la fin du processus de sécurité, mais plutôt le début d’un cycle d’amélioration continue.

1. **Correction des vulnérabilités** : Les failles identifiées doivent être corrigées, normalement en ordre de sévérité. Cette phase implique souvent les équipes de développement et d’infrastructure.
1. **Re-test** : Une fois les correctifs appliqués, il est essentiel de vérifier leur efficacité. Un nouveau test permet de s’assurer que les vulnérabilités ont réellement été corrigées.
1. **Amélioration continue** : La sécurité ne doit pas être une activité ponctuelle. Elle doit s’intégrer dans les processus de développement et de déploiement.
1. **Sensibilisation** : Les résultats du test d'intrusion doivent servir à améliorer les pratiques. Former les équipes permet de réduire le nombre de vulnérabilités à long terme.

{: .astuce-title}
>Priorisation des risques  
>
>Toutes les vulnérabilités ne sont pas équivalentes. Il est important de prioriser les correctifs en fonction de leur impact et de leur probabilité d’exploitation.
