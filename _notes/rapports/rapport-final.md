---
layout: default
title: Rapport final
parent: Rapports de sécurité
nav_order: 2
has_toc: false  
published: true
---

# Rapport final de sécurité

Au moment d'un déploiement ou d'une livraison majeure, les équipes de développement sont normalement tenues de fournir un certain nombre d'artefacts qui seront conservés dans les archives de l'entreprise et, au besoin, présentées aux clients ou aux équipes de conformité lors d'un audit. Parmi ces artefacts, on retrouve notamment :

- Une documentation (un ou plusieurs guides selon le type d'utilisateur)
- Une liste des bogues connus (*known issues*) toujours présents dans l'application
- La liste des licences utilisées par les librairies tierces ainsi que les avis (*notice*) correspondants (souvent intégrée au *SBOM*)
- Une nomenclature logicielle (*SBOM* - parfois incluse dans le rapport final de sécurité)
- Les approbations formelles des dirigeants des différentes unités d'affaires impliquées (légal, ingénierie, marketing, etc)
- Un rapport final de sécurité.

Le rapport final de sécurité vise principalement à attester que les étapes prévues au [cycle de développement](../notes/cycle-developpement-securise) sécurisé ont été suivies rigoureusement. Il permet également de synthétiser l’ensemble des activités de sécurité réalisées afin de présenter une vision globale du niveau de risque.

Ce rapport n’est pas uniquement technique : il sert à **appuyer une décision stratégique** concernant la mise en production.


## Structure

Comme pour tous les rapports, il n'existe pas un format unique et chaque entreprise est libre d'implémenter son propre standard. En général, on tentera de cibler chacune des phases du cycle de développement sécurisé afin de démontrer que ses exigences ont été respectées. Ceci se traduit souvent par un assemblage de plus petits rapports, souvent placés en annexe, résumés dans chacune des sections. Les prochaines sections proposent une structure pouvant servir de base à l'élaboration d'un rapport standard.

### 1. Sommaire exécutif

Le sommaire exécutif doit fournir suffisamment d'information aux dirigeants pour qu'il puissent prendre une décision éclairée (*go* / *no-go*) tout en étant suffisamment succint pour ne pas les perdre dans les détails techniques. Il contiendra généralement la liste des analyses ayant été effectuées ainsi qu'un résultat sommaire, généralement sous forme d'un résultat chiffré ou d'une métrique de succès ou de risque (faible/moyen/élévé ou rouge/jaune/vert).

{: .highlight-title}
> Exemple
>
> Le tableau ci-bas résume les différents requis de sécurité ainsi que le niveau de conformité du projet XYZ :
>
> | Requis | Atteint ? | Recommandation | Responsable | Notes |
> |--------|---------|----------------|-------------|-------|
> | Formation des développeurs | ✅ | Go | Alice A. | Voir section ***2. Exigences internes ou réglementaires*** |
> | Norme SOC 2 | ✅ | Go | Alice A. | Voir section ***2. Exigences internes ou réglementaires*** |
> | Modélisation de la menace | ✅ contrôles implémentés à 100% | Go | Bob B. | Voir section ***3. Modélisation de la menace*** |
> | SAST |✅ 0 vuln. critique/majeure | Go | Camille C. | Voir section ***4. SAST*** |
> | SCA | ✅0 vuln. critique/majeure | Go | Daniel D. | Voir section **5. SCA** |
> | SBOM | ✅ | Go | Emma E. | Voir section ***6. SBOM*** |
> | Licences | ✅ 0 licence incompatibles | Go | Frank F. | Licences présentes: MIT, BSD 3-clause, Apache 2.0, voir section ***6. SBOM***|
> | DAST | ✅ 0 vuln. critique/majeure | Go | Gertrude G. | Voir section ***7. DAST***|
> | Test d'intrusion | ✅, 0 vuln. critique/majeure | Go | Henri H. | Toutes les recommandations majeures intégrées au code, voir section ***8. Test d'intrusion*** |
> | Vulnérabilités restantes | 7 moyen, 13 faible | Go | Iris I. | Voir section ***9. Vulnérabilités restantes*** |
>
>*Un rapport Jira de tous les correctifs de sécurité apportés suite aux découvertes de problèmes de sécurité est également disponible en annexe.*



### 2. Exigences internes ou réglementaires

Cette section permet de démontrer que l’application respecte les exigences internes de l’entreprise ou les réglementations applicables. Elle relie la sécurité technique à des obligations organisationnelles ou légales. Cela peut inclure :

- formations obligatoires en sécurité
- politiques internes
- normes externes (ISO, SOC2, etc.)
- exigences contractuelles

{: .highlight-title}
> Exemple
>
> **2. Exigences internes ou réglementaires**
>
> Le projet XYZ respecte l'ensemble des politiques internes et des normes requises :
> 
> | Requis | Objectif | Atteinte | Notes |
> |--------|---------|----------------|-------|
> | Développeurs ayant complété la formation de sécurité niveau 1 | 100% | 100% | Rapport complet fourni à l'annexe X |
> | Développeurs ayant complété la formation de sécurité niveau 2 | 80% | 82% | Rapport complet fourni à l'annexe X |
> | Norme SOC 2 | 100% en conformité | 100% | Rapport d'audit de la firme MapleLeafsSOC fourni à l'annexe X |


### 3. Modélisation de la menace

Cette section vise à démontrer qu'une modélisation de la menace sérieuse a été effectuée et que des contrôles de sécurité permettant de mitiger les risques significatifs identifiés ont été mis en place. On voudra généralement mettre l'accent sur les principales recommandations de conception qui ont été formulées ainsi que sur la manière dont ces recommandations ont été implémentées, puis joindre la modélisation complète en annexe.

{: .highlight-title}
> Exemple
>
> **3. Modélisation de la menace**
>
> Au moment de la conception, la méthode STRIDE a été utilisée pour modéliser la menace. Le tableau ci-dessous énumère les principales recommandations émanant de cette modélisation :
> 
> | Recommandation | Implémentation | Notes |
> |--------|---------|----------------|-------|
> | Utilisation de l'authentification à 2 facteurs | Mise en place de Authentik | Utilisée pour les applications web et mobiles |
> | Mise en place de limiteur de requêtes (*rate limiter*) | fail2ban avec bannissement exponentiel pour 3 tentatives ratées | Utilisé pour toutes tentatives d'authentification |
> | <Recommandation 3> | <Mitigation 3> | <Explications/Notes> |
>
> *La modélisation STRIDE complète est disponible à l'annexe X.*


### 4. SAST

Dans cette section, on informe le lecteur que des analyses statiques du code ont été effectuées grâce à un outil [SAST](../notes/sast) (et idéalement qu'elles étaient intégrées au processus de *build*, donc répétées fréquemment). On veut l'informer des points suivants :

- Principales catégories de vulnérabilités corrigées
- Vulnérabilités restantes

{: .highlight-title}
> Exemple
>
> **4. Analyse statique du code (SAST)**
>
> Une analyse statique **semgrep** intégrée directement aux pipelines CI/CD a été effectuée à chaque livraison de nouveau code. Tout nouveau code contenant une vulnérabilité ayant un seuil plus élevé que "moyen" était bloqué et devait être corrigé avant de pouvoir intégrer la fonctionnalité à la branche principale. Les principales vulnérabilités de sévérité critique/haute ayant été identifiées pendant le cycle appartenaient aux catégories suivantes :
> 
> - Mauvaise utilisation du matériel cryptographique (principalement utilisation de clés faibles pendant le développement)
> - Présence de code de déboguage
>
> Plusieurs vulnérabilités de sévérité moyenne ou inférieure ont aussi été corrigées. Au moment de la production de ce rapport, voici les vulnérabilités restantes identifiées par l'outil SAST : 
>
> | Sévérité | Nombre | Notes |
> |--------|---------|----------------|-------|
> | Moyen | 9 | Code de gestion d'erreur pouvant être amélioré; aucune exploitation possible (validée par l'équipe de sécurité) | 
> | Faible | 14 | Principalement liées à des améliorations suggérées aux en-têtes HTTP |
>
> *La liste complète des vulnérabilités restantes peut être consultée dans la section **9. Vulnérabilités restantes**.*

### 5. SCA

Dans cette section, on informe le lecteur que le processus de *build* incluait aussi l'analyse de la chaîne d'approvisionnement à l'aide d'un outil [SCA](../notes/sca). On désire ici démontrer que le logiciel ne contient aucune dépendance vulnérable au-delà d'un certain seuil, souvent mesuré à l'aide du score [CVSS](../notes/cvss).

{: .highlight-title}
> Exemple
>
> **5. Analyse de la composition logicielle (SCA)**
>
> Une analyse de composition logicielle à l'aide de l'outil **OWASP Dependency-Check** a été effectuée à chaque livraison de nouveau code via une intégration au processus de *build*. Pour chaque *pull request*, toutes les dépendances du projet ont été analysées afin de déceler des vulnérabilités introduites par les librairies utilisées. Le tableau ci-bas détaille le nombre de vulnérabilités restantes par niveau de sévérité :
>
> | Sévérité | Nombre | Notes |
> |--------|---------|----------------|-------|
> | Critique | 0 | Aucune vulnérabilité critique | 
> | Haut | 0 | Aucune vulnérabilité haute |
> | Moyen | 2 | Liés au *framework* utilisé pour l'interface web, impossible de mettre à jour (déjà à la version la plus à jour). Sera corrigé aussitôt que possible. |
> | Faible | 7 | Problèmes mineurs et non-exploitables |
>
> *La liste complète des vulnérabilités restantes peut être consultée dans la section **9. Vulnérabilités restantes***.


### 6. SBOM (si inclus dans rapport de sécurité)

La [nomenclature logicielle (SBOM)](../notes/sbom) est parfois un livrable indépendant du rapport de sécurité. Qu'il soit fourni seul ou à même le rapport de sécurité, il contient généralement une information importante d'un point de vue légal : les différentes licences des composantes utilisées. Cela permet à l'équipe légale de s'assurer que l'entreprise n'est pas à risque d'enfreindre des conditions d'utilisation ou de devoir publier son code source en totalité ou en partie.

{: .highlight-title}
> Exemple
>
> **6. Nomenclature logicielle (SBOM)**
>
> Une nomenclature logicielle (SBOM) a été produite à l'aide de l'outil CycloneDX. Le tableau ci-dessous détaille les composants utilisés ainsi que leur licence : 
>
> | Licence | Nombre | Notes |
> |--------|---------|----------------|-------|
> | MIT | 21 | Licence compatible avec un produit commercial | 
> | BSD-3 | 13 | Licence compatible avec un produit commercial |
> | Apache 2.0 | 9 | Licence compatible avec un produit commercial |
> | **TOTAL** | **43** |  |
>
> *Le SBOM complet est fourni en annexe X.*

### 7. DAST

Cette section ressemble beaucoup à la section sur le DAST, mais on y informe le lecteur que des analyses dynamiques de l'application ont été effectuées périodiquement à l'aide d'un outil [DAST](../notes/dast). Comme pour le SAST, on veut l'informer des points suivants :

- Principales catégories de vulnérabilités corrigées
- Vulnérabilités restantes

{: .highlight-title}
> Exemple
>
> **7. Analyse dynamique (DAST)**
>
> Une analyse dynamique à l'aide de l'outil OWASP ZAP a été effectuée toutes les nuits sur la version la plus récente de l'application tout au long du processus de développement. Un rapport quotidien a été envoyé à l'équipe de développement et toute vulnérabilité d'une sévérité supérieure à "moyen" nécessitait une correction avant de poursuivre le travail de développement régulier. Les principales vulnérabilités de sévérité critique/haute ayant été identifiées pendant le cycle appartenaient aux catégories suivantes :
> 
> - Problèmes d'authentification (possiblité de contournement)
> - Présence de routes de déboguage
>
> Plusieurs vulnérabilités de sévérité moyenne ou inférieure ont aussi été corrigées. Au moment de la production de ce rapport, voici les vulnérabilités restantes identifiées par l'outil SAST : 
>
> | Sévérité | Nombre | Notes |
> |--------|---------|----------------|-------|
> | Faible | 10 | Principalement liées à des améliorations suggérées aux en-têtes HTTP |
>
> *La liste complète des vulnérabilités restantes peut être consultée dans la section **9. Vulnérabilités restantes***

### 8. Test d'intrusion

On présente ici les conclusions du rapport de test d'intrusion. Il ne s'agit pas de répéter tout le rapport (qui sera normalement joint en annexe), mais plutôt de démontrer que ses principales recommandations ont été comprises et mises en place. 

{: .highlight-title}
> Exemple
>
> **8. Test d'intrusion**
>
> La firme Intruziv inc. a été mandatée pour réaliser un test d'intrusion de type "boîte grise" sur l'application XYZ. Le tableau ci-bas détaille les principales recommandations ainsi que leur implémentation.
> 
> - Problèmes d'authentification (possiblité de contournement)
> - Présence de routes de déboguage
>
> Plusieurs vulnérabilités de sévérité moyenne ou inférieure ont aussi été corrigées. Au moment de la production de ce rapport, voici les vulnérabilités restantes identifiées par l'outil SAST : 
>
> | Recommandation | Mise en place (O/N) ? | Implémentation |
> |--------|---------|----------------|-------|
> | Recommandation 1 | O | Utilisation du framework X |
> | Recommandation 2 | O | Suppression de la fonctionnalité optionnelle Y |
> | Recommandation 3 | O | Validation des entrées pour les routes utilisant la base de données |
>
> *Le rapport complet produit par la firme Intruziv inc. est disponible à l'annexe X.*

### 9. Vulnérabilités restantes

Cette section prend généralement la forme d'un tableau où l'on énumère chacune des vulnérabilités identifiées par les outils SAST, SCA, DAST ou par le test d'intrusion n'ayant pas été corrigées avant le déploiement ou la *release*. On veut généralement présenter les informations suivantes :
- Description de la vulnérabilité
- CVE associé (si disponible)
- Risque pour l'application
- Justification
- Billet associé dans le système de gestion des bogues (Jira, Bugzilla, trac, etc)

### 10. Recommandations finales

Dans cette section, on recommande un *go* ou un *no go* pour le déploiement. Si des mesures spéciales de mitigation ou de contournement doivent être mises en place afin d'éviter certains problèmes, on peut aussi les décrire ici.

### 11. Approbations

Cette section prend généralement la forme d'un tableau où l'on recueille formellement l'approbation des différentes parties prenantes suite aux recommandations. Par exemple, on aura généralement une ligne par unité d'affaire impliquée dans le projet, avec le nom de son dirigeant ou de son représentant, son approbation (O/N) ainsi que la date à laquelle l'approbation a été donnée.

### 12. Annexes

Les annexes contiennent tous les rapports détaillés. On y attache généralement :
- La modélisation de la menace
- Le rapport détaillé SAST
- Le rapport détaillé SCA
- Le rapport détaillé DAST
- Le SBOM
- Le rapport de test d'intrusion complet
- Un rapport du système de gestion des billets (Jira, Bugzilla, Trac, etc) attestant des vulnérabilités corrigées
- Toute autre information complémentaire mais trop détaillée pour être incluse directement dans le rapport.


