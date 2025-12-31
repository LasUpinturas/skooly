# <div align="center">Skooly : L'OS des Universités Modernes</div>

<div align="center">
  <img src="./assets/schoolmanagemntphoto.jpg" alt="Skooly Banner" width="1000" style="border-radius: 20px;">
  <br />
  <br />
  <strong>Le premier ERP modulaire, souverain et offline-first conçu pour l'Afrique.</strong>
  <br />
  <sub>Digitaliser l'éducation, sécuriser les diplômes et optimiser la finance académique.</sub>
  <br />
  <br />
  <a href="https://github.com/WistantKode/skooly/stargazers"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/WistantKode/skooly?style=for-the-badge&color=blue"></a>
  <a href="./LICENSE"><img alt="License" src="https://img.shields.io/badge/License-MIT+%20Enterprise-magenta?style=for-the-badge"></a>
  <a href="https://github.com/WistantKode/skooly/network/members"><img alt="GitHub forks" src="https://img.shields.io/github/forks/WistantKode/skooly?style=for-the-badge&color=green"></a>
  <br />
  <br />
  <a href="./docs/00-INDEX.md">📖 Documentation</a> · <a href="#-démarrage-rapide">⚡ Quick Start</a> · <a href="./docs/4-guides/DEV-JOURNEY.md">🛠️ Guide Dev</a>
</div>

<br />

<div align="center">
  <img src="./assets/erp.jpg" alt="Skooly Dashboard Showcase" width="1000" style="border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1);">
</div>

<br />

## ✨ Pourquoi Skooly ?

Skooly résout les défis critiques des institutions d'enseignement supérieur en zone CEMAC :

- **Moteur LMD Natif** : Automatisation complète des délibérations, compensations et calculs de crédits.
- **Inclusion Financière** : Réconciliation automatique UBA et Mobile Money (MTN/Orange) pour éliminer la fraude.
- **Souveraineté des Données** : Certification des documents par signature numérique et QR Code infalsifiable.
- **Mode Offline** : Fonctionnement ininterrompu même en cas d'internet instable grâce à la technologie PWA.

## 🚀 Fonctionnalités Clés

- **Gestion des Inscriptions** : Workflow digitalisé du recrutement à la carte d'étudiant.
- **Finance & Comptabilité** : Gestion des droits universitaires avec ledger à partie double.
- **Saisie & Délibération** : Interface de saisie rapide pour enseignants et PV de délibération en 1 clic.
- **Multi-Campus Hierarchy** : Structure "Holding" pour piloter plusieurs écoles au sein d'une université.
- **IA Sentinel** : Détection de fraude aux notes et prédiction précoce du décrochage scolaire.

## 🛠️ Stack Technologique

Skooly utilise une architecture **Modular Monolith** moderne, performante et typée.

| Composant | Technologie |
| :--- | :--- |
| **Backend** | NestJS, TypeScript, BullMQ |
| **Frontend** | Next.js (App Router), TailwindCSS, Shadcn/UI |
| **Persistence** | PostgreSQL, Prisma ORM, Redis |
| **Infrastructure** | Turborepo, Docker, GitHub Actions |

## 📦 Démarrage Rapide

### Pré-requis
*   Node.js v20+
*   pnpm v9+
*   Docker & Docker Compose

### Installation
```bash
# Cloner le projet
git clone https://github.com/WistantKode/skooly.git
cd skooly

# Installer les dépendances
pnpm install

# Démarrer les services (Database & App)
pnpm dev
```

## 🤝 Contributeurs

Nous croyons en la force de la communauté pour transformer l'éducation.

<a href="https://github.com/WistantKode/skooly/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=WistantKode/skooly" />
</a>

## 👤 Maintainer

**[WistantKode](https://github.com/WistantKode)** — Architecte & Lead Developer

## 📊 Statistiques

![Repobeats analytics](https://repobeats.axiom.co/api/embed/b1bf4dc0226458617adbdbf5586f2df953eb0922.svg 'Repobeats analytics image')

## 📄 Licence

[MIT (Core)](https://github.com/WistantKode/skooly/blob/main/LICENSE) & Enterprise (Business Units) — © 2024 WistantKode.
