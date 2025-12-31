# Architecture Technique & Stack Technologique

## 1. Choix Technologiques (La Rationale)
L'architecture a été choisie pour maximiser la vitesse de développement sans sacrifier la stabilité à long terme.

### Backend : NestJS (Node.js) & TypeScript
*   **Pourquoi ?** Cadre de travail structuré imposant des patterns propres (Modules, Injectables). TypeScript garantit la sécurité du code lors du refactoring de grands volumes de données.
*   **Performance** : Modèle non-bloquant capable de gérer des milliers de requêtes concurrentes (connexion des étudiants pendant les résultats).

### Frontend : Next.js (App Router) & React
*   **Pourquoi ?** Rendu hybride (Server/Client) permettant des performances SEO excellentes pour la vitrine et une réactivité de type "Application Desktop" pour le dashboard.
*   **Design System** : Utilisation de **Tailwind CSS** et **Shadcn UI** pour une interface moderne, accessible et responsive nativement.

### Base de Données : PostgreSQL & Prisma
*   **Pourquoi ?** PostgreSQL est le standard industriel pour l'intégrité relationnelle et les transactions complexes. **Prisma** (ORM) permet une manipulation des données typée et sécurisée, évitant 90% des erreurs SQL classiques.

---

## 2. Infrastructure & Déploiement

### Conteneurisation (Docker)
L'application entière est "Dockerisée".
*   **Avantages** : Identité stricte entre l'environnement de développement et la production. Déploiement facile sur n'importe quel Cloud ou serveur local de l'université.

### Caching (Redis)
Une couche Redis est utilisée pour :
*   La gestion des sessions utilisateurs.
*   Le cache des droits d'accès (Permissions).
*   La gestion des files d'attente (Queues) pour l'envoi de SMS/Emails massifs.

---

## 3. Sécurité de la Stack
*   **Authentification** : JWT (JSON Web Tokens) avec rotation de clés.
*   **Protection** : Forçage HTTPS, Protection contre les attaques XSS, CSRF et Rate Limiting natif.
