---
layout: default
title: OWASP Dependency-Check
parent: SCA - Analyse de composition logicielle
nav_order: 1
---

OWASP Dependency‑Check est un outil de **Software Composition Analysis (SCA)** spécialisé dans la détection des **vulnérabilités connues** dans les dépendances d’un projet logiciel. Il s’appuie principalement sur la **National Vulnerability Database (NVD)** ainsi que sur d’autres sources pour déterminer si votre application utilise des versions affectées par des CVE connues.

L’un de ses atouts majeurs est sa capacité à analyser, non seulement les dépendances **directes**, mais également les dépendances **transitives**, souvent oubliées et pourtant critiques. Dans un projet Java, cela inclut les fichiers JAR, WAR et EAR, ou les dépendances incluses dans un fichier `pom.xml` (Maven)  Dans un projet Python, il inspecte les fichiers `requirements.txt`, `poetry.lock`, ou toute autre source de gestion de dépendances.

Dependency‑Check occupe une place essentielle dans un paysage logiciel où les chaînes d’approvisionnement sont devenues très complexes. Des incidents comme **Log4Shell** ont montré que la majorité des failles critiques proviennent d’un composant open source enfoui profondément dans une hiérarchie de dépendances.

Intégrable dans les IDE, les pipelines CI/CD, les systèmes de build (Maven, Gradle, etc.) et les automatisations, Dependency‑Check génère des rapports détaillés (HTML, XML, JSON) permettant de comprendre rapidement :
- quelles dépendances sont vulnérables,
- quelles CVE sont associées,
- quelles versions corrigées existent.

C’est un outil incontournable pour instaurer une gestion proactive et rigoureuse des dépendances open source utilisées dans un projet.

---

## Installation
```bash
wget https://github.com/dependency-check/DependencyCheck/releases/download/v12.2.0/dependency-check-12.2.0-release.zip
unzip dependency-check-12.2.0-release.zip -d $HOME/.local/share/
ln -s ~/.local/share/dependency-check/bin/dependency-check.sh ~/.local/bin/dependency-check
```

---

## Commandes courantes
### Scan d'un projet Java (Maven)
```bash
dependency-check --project "Mon projet Java" --scan ./ --format "ALL"
```

### Scan d'un dossier Python
```bash
dependency-check --project "Mon projet Python" --enableExperimental --scan ./ --format "ALL"
```

{: .astuce}
> L'option `--format` peut prendre les valeurs suivantes : `HTML`, `XML`, `CSV`, `JSON`, `JUNIT`, `SARIF`, `JENKINS`, `GITLAB`, ou `ALL` pour produire tous les formats.
