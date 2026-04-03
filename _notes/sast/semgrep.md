---
layout: default
title: Semgrep
parent: SAST - Analyse statique de sécurité
nav_order: 1
---

# Semgrep

**Semgrep** est un outil d’analyse statique moderne conçu pour être **simple**, **rapide**, **léger** et **extrêmement flexible**. Contrairement aux outils SAST traditionnels qui reposent sur des moteurs complexes, Semgrep utilise une approche unique basée sur les **patterns structurés** : des règles qui ressemblent directement au code des développeurs. Il ne s’agit donc pas d’une recherche par mots‑clés ou de regex approximatives : Semgrep comprend réellement la structure syntaxique du code.

L’outil prend en charge plus de trente langages (Java, Python, JavaScript, Go, Terraform, Docker, YAML, etc.) et s’intègre aisément dans les environnements de développement modernes.  
Ses forces principales :  
- **vitesse d’exécution exceptionnelle** (analyse en quelques secondes),  
- **règles faciles à écrire en YAML**,  
- **écosystème riche** (des milliers de règles prêtes à l’emploi),  
- **intégration fluide** dans les pipelines CI/CD,  
- **mode taint‑analysis** pour détecter des vulnérabilités plus complexes liées aux flux de données,  
- **philosophie DevSecOps** qui encourage les développeurs à corriger les problèmes immédiatement.

Avec Semgrep, les équipes détectent les mauvaises pratiques et vulnérabilités **au moment où elles sont introduites**, favorisant une sécurité proactive.  
C’est un outil conçu pour rendre la sécurité accessible, visible et opérationnelle dans le quotidien du développement.

---

## Installation
```bash
pipx install semgrep
```

---

## Commandes courantes

### Lancer un scan avec règles par défaut
```bash
semgrep scan --config auto .
```
ou plus simplement

```bash
semgrep scan
```

*Semgrep tentera de détecter automatiquement le langage et les règles à utiliser*


### Scan avec des règles spécifiques du registre Semgrep
```bash
semgrep scan --config=p/owasp-top-ten
```

{: .highlight}
> Par défaut, **Semgrep** ne fonctionne qu'avec un sous-ensemble limité de règles. Pour profiter de toutes les règles, il suffit de se créer un compte à l'adresse [https://semgrep.dev/signup](https://semgrep.dev/signup). Il faut ensuite connecter Semgrep à son compte :
>
> ```bash
>semgrep login
>```

### Scan avec un fichier de règles locales
Il est aussi possible de définir des règles personnalisées et de les inclure dans l'analyse. Pour plus d'informations sur la syntaxe à utiliser pour définir des règles, consulez la documentation officielle ici : [https://semgrep.dev/docs/writing-rules/overview](https://semgrep.dev/docs/writing-rules/overview)
```bash
semgrep --config=<répertoire contenant les fichiers de règles>
```

