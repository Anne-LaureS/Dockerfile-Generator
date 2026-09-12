<img src="https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png" alt="Docker Logo" width="60" align="left">

# Dockerfile Generator — Python & PowerShell

---

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![PowerShell](https://img.shields.io/badge/PowerShell-7.x-5391FE?logo=powershell)
![Docker](https://img.shields.io/badge/Dockerfile-Generator-2496ED?logo=docker)
![Status](https://img.shields.io/badge/Status-Fonctionnel-brightgreen)
![Projet](https://img.shields.io/badge/Projet-Pédagogique-orange)

---

Ce repository propose deux scripts permettant de générer automatiquement un **Dockerfile** ainsi qu’un fichier `app.py` minimal à partir d’une image Docker choisie par l’utilisateur.
Il inclut également une **stack Docker multi‑services** basée sur Docker Compose, comprenant :

- une application Python (générée via un script)
- un reverse proxy Nginx
- un service Redis
- un mode alternatif où Nginx sert un `index.html` statique
  
---

## 📦 Fonctionnalités

- Génération automatique d'un `Dockerfile`
- Création d'un fichier `app.py` minimal
- Scripts disponibles en **Python** et en **PowerShell**
- Compatible **Windows, Linux et macOS**
- Stack Docker complète via `docker-compose.yml`
- Reverse proxy Nginx + service Redis
- Mode statique Nginx (serveur HTML)

---

## ✅ Prérequis

- Docker installé
- Au choix :
  - Python 3.x  
  - PowerShell 5+ (Windows) ou PowerShell Core 7+ (Linux/macOS)
  - Git (pour cloner le repo)

---

## 📁 Structure du projet

```text
/
├── docker-compose.yml
├── nginx.conf
├── html/
│   └── index.html
├── feature/
│   ├── powershell-script/
│   │   └── generate-dockerfile.ps1
│   └── python-script/
│       └── generate_dockerfile.py
└── README.md             
```

---

## 🧭 Utilisation des scripts

### 1️⃣ Cloner le repository

```bash
git clone https://github.com/Anne-LaureS/Dockerfile-Generator.git
cd Dockerfile-Generator
```

### 2️⃣ Générer les fichiers Docker (Dockerfile + app.py)

⚠️ **Lancer la commande depuis la racine du repo** (pas depuis `feature/...`) — les deux
scripts écrivent `Dockerfile`/`app.py` dans le dossier courant, et `docker-compose.yml`
(étape 4) s'attend justement à les trouver à la racine.

#### Option A — Utiliser le script Python

```bash
python3 feature/python-script/generate_dockerfile.py --image python:3.12-slim --app-name monapp
```

#### Option B — Utiliser le script PowerShell

```powershell
.\feature\powershell-script\generate-dockerfile.ps1
```

### 3️⃣ Instructions

Les deux scripts fonctionnent de la même façon :

1. Saisir le nom de l’image Docker de base  
   Exemple : `python:3.11-slim`
2. Le script génère automatiquement :
   - `Dockerfile`
   - `app.py` (script Python minimal)

### 4️⃣ Construire et lancer le conteneur Docker

```bash
docker build -t my-app .
docker run --rm my-app
```

---

## 🐳 Stack Docker multi‑services
Une fois le Dockerfile généré, on lance la stack complète :

- app : ton application Python
- nginx : reverse proxy qui redirige vers app
- redis : service cache

---

## 🔀 Modes de fonctionnement Nginx
Ce projet supporte **deux modes** selon ce que l'on veut démontrer.

## 1️⃣ Mode Reverse Proxy (Nginx → Application Python)
Dans ce mode :
- Nginx écoute sur `localhost:8080`
- Il redirige vers `app:80`
- Le fichier `nginx.conf` est utilisé

## ▶️ Lancer la stack

```bash
docker compose up -d
```

▶️ Accéder à l’application
```bash
http://localhost:8080
```

▶️ Arrêter
```bash
docker compose down
```
---

## 2️⃣ Mode Serveur Statique (Nginx → index.html)
Dans ce mode :
- Nginx sert directement `html/index.html`
- Aucun backend requis

## ▶️ Activer ce mode
Dans `docker-compose.yml`, ajouter dans le service `nginx` :

```yaml
volumes:
  - ./html:/usr/share/nginx/html:ro
```

### ▶️ Accéder
```bash
http://localhost:8080
```

## 🐳 Exemple de Dockerfile généré

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY app.py .

USER 1000:1000

CMD ["python", "app.py"]
```

---

## 🔐 Sécurité

- **Utilisateur non-root** : chaque Dockerfile généré tourne sous un UID/GID numérique
  (`1000:1000`) plutôt qu'en root par défaut — réduit la surface d'attaque en cas de
  compromission du conteneur. Choix volontairement numérique (pas `adduser`/`addgroup`,
  dont la syntaxe diffère entre images Debian et Alpine) pour rester portable quelle que
  soit l'image de base choisie.
- **Images pinnées** : `nginx:1.31-alpine` et `redis:8.10.1-alpine` plutôt que `:latest`,
  pour des builds reproductibles.
- **Redis non exposé au host** : le service `redis` n'ouvre plus de port sur la machine
  hôte (Redis n'a pas d'authentification activée par défaut) — seul `app`, sur le même
  réseau Docker interne, peut l'atteindre.
- **`server_tokens off`** dans `nginx.conf` : la version nginx n'apparaît plus dans les
  en-têtes de réponse.

---

## 📦 Image publiée (GitHub Container Registry)

L'outil (script Python) est aussi publié en image Docker prête à l'emploi — pas besoin de
Python installé localement :

```bash
mkdir mon-projet && cd mon-projet
docker run --rm -v "$(pwd)":/output ghcr.io/anne-laures/dockerfile-generator:latest \
  --image python:3.11-slim --app-name monapp
```

Le `Dockerfile` et `app.py` générés apparaissent directement dans `mon-projet/` sur ta
machine (le volume monté sur `/output` fait le lien). Image reconstruite automatiquement à
chaque changement du script (`.github/workflows/publish-image.yml`).

## 🎯 Objectifs

- Comprendre la génération automatisée de Dockerfiles
- Manipuler Docker et Docker Compose
- Déployer une stack multi‑services
- Illustrer une architecture simple type micro‑services
- Standardiser des labs ou démos DevOps
