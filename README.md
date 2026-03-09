# 🚀 Dockerfile Generator — Python & PowerShell

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
│   │   ├── generate-dockerfile.ps1
│   └── python-script/
│       └── generate_dockerfile.py
└── README.md             
```

---

## 🧭 Utilisation des scripts

### 1️⃣ Cloner le repository

```bash
git clone <URL_DU_REPO>
cd Dockerfile-Generator
```

### 2️⃣ Générer les fichiers Docker (Dockerfile + app.py)

#### Option A — Utiliser le script Python

```bash
cd feature/python-script
python3 generate_dockerfile.py --image python:3.12-slim --app-name monapp
```

#### Option B — Utiliser le script PowerShell

Sous Windows PowerShell :

```powershell
.\generate_dockerfile.ps1
```

Sous PowerShell Core (Linux/macOS) :

```powershell
cd feature/powershell-script
.\generate-dockerfile.ps1
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

CMD ["python", "app.py"]
```

---

## 🎯 Objectifs

- Comprendre la génération automatisée de Dockerfiles
- Manipuler Docker et Docker Compose
- Déployer une stack multi‑services
- Illustrer une architecture simple type micro‑services
- Standardiser des labs ou démos DevOps
```
