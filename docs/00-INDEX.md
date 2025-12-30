# 📚 Skooly - Documentation Officielle

**Version**: 1.0.0 (Release Candidate) | **Architecture**: Modular Monolith

Bienvenue dans la documentation de Skooly.
Cette documentation est la **Source de Vérité**. Si ce n'est pas écrit ici, ça n'existe pas.

---

## 🎯 1. Concepts & Vision (Le "Pourquoi")
*À lire pour comprendre ce qu'on construit (et ce qu'on ne construit pas).*

*   **[01. Le Manifeste (Vision)](./1-concepts/01-vision.md)** : Pourquoi on ne veut pas d'un autre ERP médiocre.
*   **[02. Philosophie d'Architecture](./1-concepts/02-architecture-philosophy.md)** : L'approche Event-Driven inspirée d'Odoo.
*   **[03. Stratégie des Modules](./1-concepts/03-modules-strategy.md)** : Ce qui est Core vs ce qui est Bruit.
*   **[05. Business Model (Open Core)](./1-concepts/05-open-core-strategy.md)** : Gratuit (MIT) vs Enterprise.
*   **[06. Conformité & Privacy](./1-concepts/06-compliance.md)** : Souveraineté des données.

---

## 🏗️ 2. Technique (Le "Comment")
*La Bible pour les développeurs.*

*   **[01. Structure du Projet](./3-technical/01-project-structure.md)** : Arborescence Turborepo stricte.
*   **[02. Guide de Traduction Odoo](./3-technical/02-odoo-translation.md)** : Comment porter le génie d'Odoo vers NestJS.
*   **[20. Stack Technique](./3-technical/20-stack.md)** : Next.js, NestJS, Prisma, Docker.
*   **[21. Schema Database](./3-technical/21-database-schema.md)** : Modèles de données (Users, Finance, Academic).

---

## 📋 3. Spécifications Modules (Le "Quoi")
*Règles métier détaillées.*

### Modules CORE (Gratuit)
*   **[02. Socle (Core)](./2-specs/02-core.md)** : Users, Auth, Multi-tenant.
*   **[03. Académique](./2-specs/03-academic.md)** : LMD, Inscriptions.
*   **[04. Présences](./2-specs/04-attendance.md)** : QR Code Anti-fraude.
*   **[06. Finance](./2-specs/06-finance.md)** : Paiements Mobile Money, Compta Double Entrée.

### Modules PREMIUM (Enterprise)
*   **[05. Emploi du Temps](./2-specs/05-scheduling.md)** : Algorithmes de contraintes.
*   **[07. Notes & Délibérations](./2-specs/07-grades.md)** : Calculs complexes LMD.
*   **[08. Documents Sécurisés](./2-specs/08-documents.md)** : Diplômes certifiés QR.
*   **[09. Stages](./2-specs/09-internships.md)** : Workflow entreprise.
*   **[10. Communication](./2-specs/10-communication.md)** : Hub SMS/WhatsApp.
*   **[11. Campus Services](./2-specs/11-campus.md)** : Logement, Resto.
*   **[12. Bibliothèque](./2-specs/12-library.md)** : Prêts et Pénalités.
*   **[13. RH & Paie](./2-specs/13-hr-payroll.md)** : Paie Vacataires.
*   **[14. Alumni](./2-specs/14-alumni.md)** : Insertion pro.
*   **[15. Reporting & Décisionnel](./2-specs/15-reporting.md)** : Dashboards Temps Réel.
*   **[16. Data Management](./2-specs/16-data-management.md)** : Imports & Migration Wizard.

### Intelligence Artificielle
*   **[🧠 Stratégie IA](./2-specs/AI-MODULES.md)** : Anti-fraude & Prédiction décrochage.

---

## 🧭 4. Guides
*Pour démarrer.*

*   **[🗺️ Parcours du Développeur](./4-guides/DEV-JOURNEY.md)** : **COMMENCE ICI**.
*   **[README du Projet](../README.md)** : Installation rapide.
