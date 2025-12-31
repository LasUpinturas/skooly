# Skooly : Système d'Exploitation pour l'Éducation

Skooly est une plateforme ERP (Enterprise Resource Planning) modulaire et souveraine conçue pour moderniser la gestion des universités et des établissements d'enseignement supérieur en Afrique.

---

## Sommaire de la Documentation

Pour assurer une compréhension optimale, la documentation est structurée selon quatre axes stratégiques.

### 1. Concepts et Vision
Comprendre la philosophie du projet et ses choix structurants.
*   **[Vision Produit](./1-concepts/01-vision.md)** : Pourquoi Skooly ? Problématiques et solutions.
*   **[Philosophie d'Architecture](./1-concepts/02-architecture-philosophy.md)** : Modularité et approche événementielle.
*   **[Stratégie des Modules](./1-concepts/03-modules-strategy.md)** : Core Open Source vs Extensions Métier.
*   **[Business Model](./1-concepts/05-open-core-strategy.md)** : Stratégie Open Core et viabilité.
*   **[Conformité & Souveraineté](./1-concepts/06-compliance.md)** : Protection des données et lois nationales.

### 2. Spécifications Fonctionnelles
Le détail opérationnel de chaque métier de l'université.
*   **[Modules de Base (Core)](./2-specs/02-core.md)** : Identité, Utilisateurs et Tenants.
*   **[Gestion Académique LMD](./2-specs/03-academic.md)** : Inscriptions et structure pédagogique.
*   **[Finance & Comptabilité](./2-specs/06-finance.md)** : Paiements UBA et Mobile Money.
*   **[Notes & Délibérations](./2-specs/07-grades.md)** : Algorithmes de calcul et PV.
*   **[Présences & Anti-Fraude](./2-specs/04-attendance.md)** : Suivi par QR Code dynamique.
*   **[Emploi du Temps](./2-specs/05-scheduling.md)** : Gestion des ressources et salles.
*   **[Hiérarchie Multi-Campus](./2-specs/19-multi-campus-hierarchy.md)** : Architecture Université vs Écoles.

### 3. Architecture Technique
Le manuel d'ingénierie pour les développeurs.
*   **[Stack Technologique](./3-technical/20-stack.md)** : Choix des outils et rationale.
*   **[Architecture Base de Données](./3-technical/21-database-schema.md)** : Modélisation et intégrité.
*   **[Stratégie UI/UX](./3-technical/30-ui-strategy.md)** : Design System et expérience utilisateur.
*   **[API & Intégrations](./3-technical/40-integration-api.md)** : Webhooks et connecteurs tiers.
*   **[Stratégie Offline](./3-technical/60-offline-strategy.md)** : Fonctionnement en mode réseau instable.
*   **[Disaster Recovery](./3-technical/50-disaster-recovery.md)** : Continuité de service et sauvegardes.

### 4. Guides et Onboarding
*   **[Developer Journey](./4-guides/DEV-JOURNEY.md)** : Guide d'installation et de contribution.
*   **[Guide de Traduction Odoo](./3-technical/02-odoo-translation.md)** : Concepts pour experts Odoo.
