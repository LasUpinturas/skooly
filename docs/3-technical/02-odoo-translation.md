# Guide de Traduction : Odoo vers Skooly (NestJS/Prisma)

## 1. Objectif
Ce document est destiné aux ingénieurs connaissant les concepts d'Odoo et souhaitant comprendre comment ils sont traduits techniquement dans Skooly. Nous conservons la puissance fonctionnelle d'Odoo en utilisant une stack moderne et performante.

---

## 2. Correspondance des Concepts

| Concept Odoo | Équivalent Skooly | Technologie |
| :--- | :--- | :--- |
| **Model** | Prisma Model | `schema.prisma` |
| **res.partner** | `Partner` Model | Core Module |
| **ORM Method** | Service Method | NestJS Provider |
| **Domain Filter** | Prisma Query | `where: { ... }` |
| **Action / View** | React Route / Component | Next.js |
| **QWeb Report** | PDF Template | Puppeteer / React-PDF |
| **Scheduled Action** | Cron Job | NestJS Tasks |

---

## 3. Patterns de Développement

### A. Modularité vs Héritage
Odoo utilise intensément l'héritage de classes (`_inherit`). Skooly privilégie la **Composition** et les **Événements**.
*   *Architecture* : Pour ajouter un champ à un modèle existant, on utilise une relation `1:1` ou une extension de schéma Prisma plutôt que de modifier le noyau.

### B. Sécurité
Là où Odoo utilise des `ir.model.access`, Skooly utilise des `Guards` NestJS couplés à un système de **RBAC (Role-Based Access Control)** typé.

### C. Performance
Contrairement au modèle synchrone de Python/Odoo, Skooly exploite le modèle asynchrone (Non-blocking I/O) de Node.js, permettant une meilleure montée en charge sur les opérations d'entrée/sortie.
