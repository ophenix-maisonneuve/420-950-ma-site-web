---
layout: default
title: Fuzzing
parent: Analyse dynamique
nav_order: 2
has_toc: false
---

# Fuzzing

Le ***fuzzing***, ou *fuzz testing*, est une technique de test de sécurité qui consiste à soumettre une application à de grandes quantités de données inattendues, malformées ou spécialement conçues, afin d’observer son comportement et d’identifier des défaillances. Contrairement aux tests fonctionnels classiques, le fuzzing ne cherche pas à vérifier un comportement attendu, mais à provoquer des situations anormales ou imprévues.

Dans un contexte de sécurité applicative, le fuzzing vise principalement à détecter :
- des vulnérabilités d’injection,
- des erreurs de validation d’entrées,
- des failles logiques,
- des comportements instables (crash, exceptions, erreurs internes).

Le fuzzing est aujourd’hui considéré comme une **technique dynamique**, et donc un sous‑ensemble du DAST, tout en possédant ses propres particularités.

---

## Historique

Le fuzzing trouve ses origines à la fin des années 1980, donc bien avant les applications web. En 1989, le professeur **Barton P. Miller**, de l’Université du Wisconsin, mène une expérience qui est devenue une référence : il injecte des flux aléatoires de données dans des programmes Unix courants afin d’observer leur stabilité. Résultat : une proportion significative de programmes *crashent* face à des entrées inattendues.

C'est à partir de ce moment que l'on commencera à parler du *fuzz testing* comme approche systématique de découverte de bogues et de failles. À l’origine, le fuzzing est principalement utilisé pour tester :
- des programmes en ligne de commande
- des bibliothèques
- des analyseurs de formats binaires

Avec l’essor des applications réseau et web dans les années 2000, le fuzzing évolue pour s’adapter à des protocoles structurés comme HTTP, SMTP ou DNS. Il devient progressivement un outil de sécurité à part entière, intégré aux pratiques de test applicatif.

Aujourd’hui, le fuzzing est utilisé aussi bien pour :
- la sécurité applicative
- la robustesse des logiciels
- la recherche de vulnérabilités *zero‑day*
- l’assurance qualité avancée

---

## Objectifs

En sécurité, l’objectif principal du fuzzing est d’identifier des comportements dangereux déclenchés par des entrées non anticipées par les développeurs. Le fuzzing cherche ainsi à explorer l’espace des entrées au‑delà des cas fonctionnels normaux.

Plus précisément, le fuzzing permet de :
- tester la robustesse des mécanismes de validation
- forcer l’exécution de chemins de code atypiques
- révéler des hypothèses implicites dans la logique applicative
- identifier des failles difficiles à détecter manuellement

Contrairement aux scans DAST classiques, le fuzzing ne se contente pas de tester quelques payloads connus : il vise la **quantité** et la **variété** des entrées.

---

## Fuzzing vs DAST

Le fuzzing est souvent présenté comme une composante du DAST, ce qui est à la fois vrai mais qui ignore aussi certaines particularités. Les scanners DAST traditionnels exécutent des tests ciblés, basés sur des signatures et des règles connues, alors que le fuzzing adopte une approche plus exploratoire.

Un DAST classique répond généralement à la question :
> *Cette entrée est‑elle vulnérable selon des patrons connus ?*

Le fuzzing, lui, cherche plutôt à répondre à :
> *Que se passe‑t‑il si l’application reçoit quelque chose de totalement inattendu ?*

Dans la pratique :
- le DAST vise l’efficacité et la couverture fonctionnelle
- le fuzzing vise la profondeur et la découverte d’anomalies non anticipées

C’est pour cette raison que le fuzzing est souvent utilisé **en complément**, sur des fonctionnalités critiques ou sensibles.

---

## Types de fuzzing

### Fuzzing naïf (*dumb fuzzing*)

Le fuzzing naïf est la forme la plus simple de fuzzing. Il consiste à envoyer des données essentiellement aléatoires ou prédéfinies, sans connaissance approfondie de la structure de l’entrée attendue par l’application.

Cette approche a l’avantage d’être :
- simple à mettre en oeuvre
- rapide à déployer
- peu coûteuse en configuration

Cependant, le dumb fuzzing présente des limites importantes dans les applications modernes, fortement structurées. Des entrées totalement aléatoires sont souvent rejetées très tôt par les mécanismes de validation, ce qui réduit la profondeur d’exploration.

Le dumb fuzzing reste néanmoins utile pour :
- provoquer des erreurs grossières
- tester des champs peu protégés
- identifier des crashs rapides ou des exceptions non gérées

## Fuzzing intelligent (*smart fuzzing*)

Le fuzzing intelligent repose sur une compréhension partielle – ou complète – de la structure des entrées attendues. Plutôt que de fournir des données entièrement aléatoires, le fuzzing intelligent génère des entrées **structurellement valides**, mais sémantiquement problématiques.

Cette approche permet :
- de franchir les validations initiales
- d’atteindre une logique applicative plus profonde
- de tester des scénarios réalistes mais extrêmes

Le smart fuzzing est particulièrement efficace pour :
- les formulaires complexes
- les API REST structurées
- les paramètres encodés (JSON, XML, JWT)
- les flux métier multi‑étapes

En contrepartie, il nécessite plus de préparation, de compréhension du format et parfois des outils spécialisés.

---

## Génération des données

### Mutation

Le fuzzing par mutation part d’entrées valides existantes, qu’il modifie progressivement. Chaque entrée est altérée selon différentes stratégies : inversion de bits, allongement de chaînes, caractères spéciaux, valeurs limites, etc.

Cette approche présente un excellent compromis entre efficacité et simplicité, car elle repose sur des entrées déjà acceptées par l’application.

### Génération

Le fuzzing par génération, quant à lui, produit des entrées à partir d’un modèle ou d’une grammaire définissant le format attendu. Cette approche est plus complexe, mais aussi plus puissante lorsqu’elle est bien implémentée.

Elle permet de :
- couvrir systématiquement un large espace d’entrées
- tester des combinaisons rares mais valides
- explorer en profondeur des protocoles complexes

---

## Types de vulnérabilités révélées par le fuzzing

Le fuzzing est particulièrement efficace pour détecter :
- des injections non couvertes par des règles standards
- des failles de validation conditionnelle
- des erreurs de logique métier
- des fuites d’information déclenchées par des entrées anormales
- des comportements non déterministes de l’application

Ces vulnérabilités sont souvent absentes des listes classiques de signatures, ce qui explique la valeur du fuzzing dans des contextes avancés.

---

## Limites et risques du fuzzing

Malgré sa puissance, le fuzzing présente des limitations importantes. Il peut générer un volume considérable de trafic et provoquer des effets indésirables, tels que des indisponibilités ou des corruptions de données.

Le fuzzing nécessite également :
- une interprétation humaine des résultats
- une analyse fine des réponses
- une capacité à distinguer anomalies bénignes et failles réelles

Pour ces raisons, le fuzzing est généralement exécuté :
- sur des environnements de test
- de manière ciblée
- avec des garde‑fous stricts

---

## Conclusion

Le fuzzing constitue l’une des techniques les plus puissantes et les plus exploratoires en sécurité applicative. Contrairement aux scans basés sur des règles fixes, il permet de mettre en lumière des vulnérabilités inattendues et des comportements imprévus.

Intégré intelligemment dans une stratégie DAST, le fuzzing devient un outil de **découverte**, complémentaire des approches plus déterministes, et indispensable pour atteindre un haut niveau de maturité en sécurité applicative.
