# Argus — CTI Intelligence Platform

Argus est une plateforme de **Cyber Threat Intelligence** personnelle composée de deux modules complémentaires : un agrégateur de données CTI multi-sources couplé à un LLM pour la génération automatique d'IOC, et une instance OpenCTI locale pour l'analyse et la visualisation des menaces.

---

## Objectifs du projet

- Agréger automatiquement des données CTI depuis des sources publiques (OTX, Abuse.ch, VirusTotal, NVD…)
- Utiliser un LLM (Claude API) pour extraire et structurer des IOC depuis des rapports bruts
- Normaliser les données en **STIX 2.1** pour l'interopérabilité
- Ingérer les IOC dans une instance **OpenCTI** locale via un connecteur custom
- Pratiquer l'analyse de menaces, le pivoting et la création de règles de détection

---

## Architecture

```
Sources CTI (OTX, Abuse.ch, VT, NVD...)
        │
        ▼
  collectors/          ← Fetch + normalisation brute
        │
        ▼
  normalizer/          ← Conversion STIX 2.1
        │
        ▼
   storage/            ← PostgreSQL + Redis (dedup)
        │
        ▼
     llm/              ← Analyse Claude API → IOC structurés
        │
        ▼
   pipeline/           ← Orchestration + export STIX
        │
        ▼
  opencti/             ← Connecteur pycti → OpenCTI local
```

---

## Installation

### Prérequis

- Python 3.11+
- Docker Desktop avec WSL2 activé (Windows)
- Git

### 1. Cloner le repo

```bash
git clone https://github.com/<ton-username>/argus.git
cd argus
```

### 2. Créer l'environnement virtuel

```bash
python -m venv venv
# Windows
venv\Scripts\activate
```

### 3. Installer les dépendances

```bash
pip install -r requirements.txt
```

### 4. Configurer les variables d'environnement

```bash
cp .env.example .env
# Éditer .env avec tes clés API
```

Variables requises :

```env
# Sources CTI
OTX_API_KEY=
VT_API_KEY=
SHODAN_API_KEY=

# LLM
ANTHROPIC_API_KEY=

# Base de données
DB_URL=postgresql://argus:argus@localhost:5432/argus

# Redis
REDIS_URL=redis://localhost:6379

# OpenCTI
OPENCTI_URL=http://localhost:8080
OPENCTI_TOKEN=
```


---

## Apprentissages visés

Ce projet couvre les domaines suivants :

- Écosystème CTI (standards STIX/TAXII, MITRE ATT&CK, OpenCTI)
- Développement Python avancé (async, ORM, API REST)
- Intégration et prompt engineering LLM
- Infrastructure Docker & déploiement local
- Analyse de menaces, pivoting sur IOC
- Workflow CTI → Détection (purple team)

---

## Avertissement

Ce projet est à vocation **éducative et personnelle**. Les IOC générés sont issus de sources publiques ouvertes. Ne pas utiliser dans un contexte de production sans validation approfondie.

---

## Licence

MIT
