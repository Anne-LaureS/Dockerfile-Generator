# 🚀 Dockerfile Generator — Python & PowerShell

Ce repository propose deux scripts permettant de générer automatiquement un Dockerfile à partir d'une image Docker choisie par l'utilisateur.  
Il s'agit d'un outil simple et utile pour les environnements DevOps ou les labs.
---

## 📦 Fonctionnalités

- Choix interactif de l'image Docker (ex: `python:3.11-slim`)
- Génération automatique d'un `Dockerfile`
- Création d'un fichier `app.py` minimal
- Disponible en **Python** et en **PowerShell**
- Compatible Windows, Linux et macOS

---

## ✅ Prérequis

- Docker installé (recommandé pour tester l’image)
- Au choix :
  - Python 3.x  
  - PowerShell 5+ (Windows) ou PowerShell Core 7+ (Linux/macOS)

---

## 🧭 Étapes d’utilisation

### 1️⃣ Cloner le repository

```bash
git clone <URL_DU_REPO>
cd <NOM_DU_REPO>
```

### 2️⃣ Choisir le langage d’exécution

#### Option A — Utiliser le script Python

```bash
python generate_dockerfile.py
```

#### Option B — Utiliser le script PowerShell

Sous Windows PowerShell :

```powershell
.\generate_dockerfile.ps1
```

Sous PowerShell Core (Linux/macOS) :

```powershell
pwsh ./generate_dockerfile.ps1
```

### 3️⃣ Répondre aux questions

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

## 📁 Structure du projet

```text
/
├── generate_dockerfile.py      # Script Python
├── generate_dockerfile.ps1     # Script PowerShell
├── Dockerfile                  # Généré automatiquement
└── app.py                      # Généré automatiquement
```

## 🐳 Exemple de Dockerfile généré

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY app.py .

CMD ["python", "app.py"]
```

## 🎯 Objectif

- Accélérer la création de Dockerfiles simples
- Standardiser des labs ou démos DevOps
- Servir de support pédagogique minimal et clair

```

