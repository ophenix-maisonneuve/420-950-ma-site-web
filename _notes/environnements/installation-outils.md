---
layout: default
title: "Installation manuelle"
parent: "Environnement applicatif"
nav_order: 1
published: true
has_toc: false
---

# Installation des outils de l’environnement applicatif

Ce document regroupe, pour chaque outil utilisé dans le cours, les méthodes d’installation sous :
- **Linux (Debian / LMDE)**
- **Windows (natif)** — lorsque cela est possible
- **Windows (WSL)**
- **macOS**

{: .warning}
> Pour chaque outil, il existe fort probablement des méthodes d'installation différentes. Ces étapes sont fournies exclusivement à titre informatif pour les étudiants qui veulent tenter d'installer l'environnement applicatif directement sur leur poste de travail. La méthode officiellement prise en charge pour le déroulement du cours est l'utilisation de la [machine virtuelle pré-configurée](../notes/environnement-applicatif).

---

# Développement & IDE

## Visual Studio Code
### Extensions recommandées (Java & Python)
- **Extension Pack for Java** (Microsoft) - inclut :
  - Language Support for Java (Red Hat)
  - Debugger for Java
  - Test Runner for Java
  - Maven for Java
  - Project Manager for Java
- **Python** (Microsoft) - inclut :
  - Support pour le langage Python
  - Python Debugger
  - Pylance
  - Python Environments

### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /usr/share/keyrings/ms.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/ms.gpg] https://packages.microsoft.com/repos/vscode stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install code
```

### Windows
Téléchargement : https://code.visualstudio.com/

### Windows (WSL)
Installer côté Linux :
```bash
sudo apt update && sudo apt install code
```
Ou exécuter directement depuis Windows :
```bash
code .
```

### macOS
```bash
brew install --cask visual-studio-code
```

---

## Python / pip / pipx
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv pipx
```

### Windows
Téléchargement : https://www.python.org/downloads/

### Windows (WSL)
Identique à Debian.

### macOS
```bash
brew install python
brew install pipx
```

---

## Java (JRE et JDK) / Maven
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install openjdk-25-jdk
cd /tmp
wget https://dlcdn.apache.org/maven/maven-3/3.9.14/binaries/apache-maven-3.9.14-bin.tar.gz
sudo tar -xzf apache-maven-3.9.14-bin.tar.gz -C $HOME/.local/share/
ln -s ~/.local/share/apache-maven-3.9.14/bin/mvn ~/.local/bin/mvn
```

### Windows
Téléchargement : https://learn.microsoft.com/en-us/java/openjdk/download
Configuration des variables d’environnement :
  - Ajoutez le chemin du dossier bin du JDK à la variable PATH
  - Créez une variable JAVA_HOME pointant vers le dossier d’installation du JDK

Vérification de l'installation :

```shell
java -version
javac -version
```

### Windows (WSL)
Identique à Debian.

### macOS
```bash
brew install openjdk
```

## Git
### Linux (Debian / LMDE)
```bash
sudo apt install git
```
---

# Cryptographie & Réseau

## OpenSSL
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install openssl
```

### Windows
Via Chocolatey :
```powershell
choco install openssl
```

### Windows (WSL)
Identique à Debian.

### macOS
```bash
brew install openssl
```

---

## Nginx
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install nginx
```

### Windows
Suivre les étapes ici : https://nginx.org/en/docs/windows.html

### Windows (WSL)
Identique à Debian.

### macOS
```bash
brew install nginx
```

---

## Wireshark
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install wireshark
sudo usermod -aG wireshark $USER
```

### Windows
Téléchargement : https://www.wireshark.org/download.html

### macOS
```bash
brew install --cask wireshark
```

---

# SAST / SCA / SBOM

## Semgrep
### Linux (Debian / LMDE)
```bash
sudo apt update
sudo apt install pipx
pipx install semgrep
```

### Windows
```powershell
pipx install semgrep
```

### macOS
```bash
pipx install semgrep
```

---

## OWASP Dependency-Check
### Linux (Debian / LMDE)
```bash
sudo apt install default-jdk unzip
wget https://github.com/dependency-check/DependencyCheck/releases/download/v12.2.0/dependency-check-12.2.0-release.zip
unzip dependency-check-12.2.0-release.zip -d $HOME/.local/share/
ln -s ~/.local/share/dependency-check/bin/dependency-check.sh ~/.local/bin/dependency-check
```

### Windows
Télécharger ZIP : https://github.com/dependency-check/DependencyCheck/releases/download/v12.2.0/dependency-check-12.2.0-release.zip 

### macOS
```bash
brew install dependency-check
```

---

## CycloneDX (SBOM)
### Linux (Debian / LMDE)
```bash
pipx install cyclonedx-bom
pipx ensurepatht
```

### Windows (natif)
```bash
pipx install cyclonedx
pipx ensurepath
```

### macOS
```bash
pipx install cyclonedx
pipx ensurepath
```

---

# Durcissement & Opérations

## UFW
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install ufw
```

### Windows
N/A

### macOS
N/A

---

## fail2ban
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install fail2ban
```

### Windows
N/A

### macOS
```bash
brew install fail2ban
```

---

# Applications vulnérables

## Juice Shop
### Linux (Debian / LMDE)  / WSL
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
nvm install node --lts
git clone https://github.com/juice-shop/juice-shop.git
cd juice-shop
npm install
npm start
```

### Windows
Via Node.js.

### macOS
Identique à Debian.

---

## WebGoat
### Linux (Debian / LMDE)  / WSL / macOS
```bash
wget https://github.com/WebGoat/WebGoat/releases/download/v2025.3/webgoat-2025.3.jar
java -jar webgoat-2025.3.jar
```

### Windows (natif)
Téléchargement : https://github.com/WebGoat/WebGoat/releases/download/v2025.3/webgoat-2025.3.jar

```bash
java -jar webgoat-2025.3.jar
```
---

# Outils réseau & utilitaires

## nmap
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install nmap
```

### Windows
Téléchargement : https://nmap.org/download.html

### macOS
```bash
brew install nmap
```

---

## netcat
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install netcat-openbsd
```

### macOS
```bash
brew install netcat
```

---

## dnsutils
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install dnsutils
```

### macOS
```bash
brew install bind
```

---

## traceroute
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install traceroute
```

### macOS
```bash
brew install traceroute
```

---

## whois
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install whois
```

### macOS
```bash
brew install whois
```

---

## jq
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install jq
```

### macOS
```bash
brew install jq
```

---

## tree
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install tree
```

### macOS
```bash
brew install tree
```

---

## vim / nano
### Linux (Debian / LMDE) / WSL
```bash
sudo apt install vim nano
```

### macOS
```bash
brew install vim
```

---

## rsyslog
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install rsyslog
```

### Windows / macOS
N/A

---

## tcpdump
### Linux (Debian / LMDE) / WSL
```bash
sudo apt update
sudo apt install tcpdump
```

### Windows
Installer WinDump : https://www.winpcap.org/windump/

### macOS
```bash
brew install tcpdump
```