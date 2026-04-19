---
layout: default
title: CycloneDX
parent: SBOM - Nomenclature logicielle
nav_order: 1
---

# OWASP CycloneDX

CycloneDX est un standard de nomenclature logicielle développé par OWASP pour répondre aux besoins croissants de **transparence**, de **sécurité** et de **traçabilité** dans la chaîne d'approvisionnement logicielle. Contrairement à une simple liste de dépendances, CycloneDX vise à fournir une représentation riche et complète des composants d’un système.

Un SBOM CycloneDX peut inclure :
- les dépendances directes et transitives
- les licences associées à chaque composant
- des métadonnées complètes sur les modules
- les relations entre composants
- les empreintes et signatures cryptographiques
- les potentielles vulnérabilités référencées

CycloneDX n’est pas qu’un format, mais un **écosystème complet**, avec :
- des outils CLI
- des *plugins* dédiés pour Maven, Gradle, npm, pip, etc.
- des intégrations dans les pipelines CI/CD
- une compatibilité avec des outils comme Dependency‑Track pour le suivi continu.

Il répond aux exigences modernes des audits, de la conformité et des normes de cybersécurité qui demandent de plus en plus aux organisations de fournir une traçabilité de tous les composants logiciels utilisés dans leurs produits.

Dans un contexte où les attaques sur la chaîne d'approvisionnement logicielle se multiplient, CycloneDX fournit un standard d'interopérabilité entre la grande majorité des outils utilisant les SBOM.

---

## Installation
```bash
pipx install cyclonedx-bom
pipx ensurepath
```

---

## Commandes courantes
### Générer un SBOM pour un projet Java (*Maven*)

**Générer un SBOM à la volée**
```bash
mvn org.cyclonedx:cyclonedx-maven-plugin:makeAggregateBom
```

**Intégrer la génération du SBOM au *build* de façon permanente**
1. Ajouter le *plugin* cyclonedx-maven au `pom.xml` du projet

    ```maven
    <!-- uses default configuration -->
    <plugins>
        <plugin>
            <groupId>org.cyclonedx</groupId>
            <artifactId>cyclonedx-maven-plugin</artifactId>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>makeAggregateBom</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
    ```
2. Lancer le *build* comme à l'habitude

    ```bash
    mvn clean package
    ```


### Générer pour un projet Python

À partir d'un répertoire où un environnement virtuel Python existe, lancer la commande suivante :

```bash
cyclonedx-py venv <dossier de l'environnement virtuel> -o sbom.json
```

## Sunshine

Les nomenclatures logicielles sont généralement produites dans l'un des deux principaux formats standards (CycloneDX ou SPDX). Il existe donc une multitude d'outils de visualisation permettant d'interpréter les SBOM de façon plus lisible et claire. L'outil [CycloneDX Sunshine](https://cyclonedx.github.io/Sunshine/), disponible gratuitement en ligne, est un bon exemple.


---

## Liens utiles

- Documentation officielle de CycloneDX : [https://cyclonedx.org/](https://cyclonedx.org/)
- Sunshine : [https://cyclonedx.github.io/Sunshine/](https://cyclonedx.github.io/Sunshine/)