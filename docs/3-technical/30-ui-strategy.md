# 🎨 Stratégie UX Globale & UI : L'Écosystème Skooly

> **Correction de Tir** : Skooly n'est pas juste une "App". C'est une plateforme double-face.
> 1. **Commercial (Vitrine)** : Pour vendre le rêve (`www.skooly.io`).
> 2. **Opérationnel (ERP)** : Pour gérer la réalité (`app.skooly.io`).

---

## 🌍 1. La Vitrine Commerciale (`www.skooly.io`)

Avant de gérer une école, il faut convaincre le Recteur de signer.
Le site marketing doit crier "Modernité & Sécurité".

### A. Structure du Site
*   **Hero Section** : "L'OS des Universités Africaines Modernes". Pas de jargon, une promesse.
*   **Product Tour interactif** : Pas de vidéo statique. Une démo cliquable (Scribe/Loom).
*   **Pricing Page (Open Core)** :
    *   *Community* : Gratuit (Host yourself).
    *   *Cloud* : Prix par étudiant/an.
*   **Call to Action (CTA)** : "Créer mon Institution" (Setup Wizard instantané).

### B. Le Workflow "Sign-Up" (L'Onboarding)
1.  Utilisateur arrive sur `www.skooly.io`.
2.  Clique sur "Démarrer Gratuitement".
3.  **Wizard Multi-step** :
    *   Step 1 : Nom de l'Institution ("IUT Douala").
    *   Step 2 : Domaine souhaité (`iut-douala.skooly.io`).
    *   Step 3 : Admin Account setup.
4.  **Magic Moment** : Le système déploie le Tenant en 3 secondes et redirige vers `app.skooly.io`.

---

## 🚀 2. L'Application ERP (`app.skooly.io`)

C'est ici que le travail se fait.
**Inspiration UX** : Odoo (Modularité) + Linear (Fluidité).

### A. Le "Home Screen" (Le Launcher Odoo, mais en v2.0)
Comme sur iOS ou Odoo Enterprise.
Pas de dashboard par défaut rempli de graphiques inutiles.
**L'App Launcher** :
*   Une grille d'icônes magnifiques (Glassmorphism).
*   [🎓 Scolarité] [💰 Finance] [📅 Planning] [🏥 Infirmerie].
*   *Pourquoi ?* Chaque employé a un métier différent. Le comptable clique sur 💰, le prof sur 📅. C'est clair, net, focus.

### B. La Navigation (Breadcrumb Navigation)
Une fois dans un module (ex: Finance), on ne veut plus voir les icônes des autres modules (Distraction).
*   **Fil d'Ariane Actif** : `Home > Finance > Factures #INV-2024-001`.
*   **Switcher Rapide (Cmd+K)** : Pour changer de module sans repasser par l'accueil.

### C. La Vue "Kanban vs List" (L'Héritage Odoo)
Pour chaque entité (Étudiants, Factures, Cours), l'utilisateur **choisit** sa vue :
1.  **Vue Liste (Excel)** : Pour l'administration pure et dure. (Dense, Triable).
2.  **Vue Kanban** : Pour les workflows.
    *   *Exemple Inscriptions* : Colonnes "Brouillon" -> "Validé" -> "Payé". On drag & drop les dossiers.
3.  **Vue Calendrier** : Pour les cours.
4.  **Vue Activité** : Pour le suivi ("Qui doit appeler ce parent ?").

---

## 🧠 3. UX Patterns "Power User"

### A. Le Contextual Sidebar (Le "Volet Droit")
Quand on clique sur une ligne dans un tableau, on ne charge pas une nouvelle page.
Un **Volet Latéral (Drawer)** s'ouvre à droite.
*   On voit le détail de l'étudiant.
*   On peut modifier, envoyer un message, voir ses notes.
*   On ferme (`Esc`), on est toujours sur la liste. **Zéro perte de contexte**.

### B. Les Filtres Avancés ("Smart Search")
La barre de recherche n'est pas juste un "text search".
*   On tape : `Filière:GL` `Status:Inscrit` `Solde > 0`.
*   Le système filtre instantanément.
*   On peut "Sauvegarder ce filtre" comme "Favori" (ex: "Mes Impayés GL").

---

## 🎨 4. Identité Visuelle (Design Tokens)

*   **Primaire** : `Deep Indigo` (Sérieux, Institutionnel).
*   **Secondaire** : `Vibrant Teal` (Action, Validation).
*   **Danger** : `Rose Red` (Erreur, Dette).
*   **Radius** : `md` (Ni trop rond, ni trop carré).
*   **Font** : `Geist Sans` (Moderne, Lisible, Tech).
