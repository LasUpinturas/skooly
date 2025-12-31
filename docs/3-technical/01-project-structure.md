# Structure du Projet et Organisation du Code

## 1. Philosophie de Dossiers
Skooly utilise une structure de **Monorepo** gérée par Turborepo. Cela permet de séparer les responsabilités tout en partageant le code entre le serveur et l'interface.

```text
skooly/
├── apps/
│   ├── api/          # Serveur Backend (NestJS)
│   └── web/          # Interface Utilisateur (Next.js)
├── packages/
│   ├── database/     # Prisma Client & Schémas
│   ├── shared/       # Types TypeScript & Validation
│   └── ui/           # Components Library (Shadcn UI)
└── docs/             # Documentation (Phase de réflexion)
```

---

## 2. Organisation du Backend (api/)
Le backend suit une approche modulaire stricte. Chaque module (Finance, Academic, etc.) est contenu dans son propre dossier.

```text
apps/api/src/modules/
├── core/             # Auth, User, Tenant (Fondations)
├── finance/          # Facturation, Paiements
├── academic/         # Inscriptions, LMD
└── ...               # Autres modules
```

Chaque module contient :
*   `controllers/` : Les points d'entrée API.
*   `services/` : La logique métier pure.
*   `dto/` : Définition des objets d'entrée (Data Transfer Objects).
*   `entities/` : Modèles de données locaux.

---

## 3. Organisation du Frontend (web/)
Nous utilisons le **App Router** de Next.js pour une navigation fluide et performante.

```text
apps/web/app/
├── (auth)/           # Pages de connexion/inscription
├── (dashboard)/      # L'application ERP principale
│   ├── finance/
│   ├── students/
│   └── ...
└── api/              # API Routes internes (SSR)
```

## 4. Standards de Développement
*   **Langage** : TypeScript (Strict mode).
*   **Style** : Eslint (Config Turborepo) + Prettier.
*   **Commits** : Conventional Commits (`feat:`, `fix:`, `docs:`).
