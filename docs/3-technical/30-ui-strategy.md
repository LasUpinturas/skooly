# Stratégie d'Interface et Expérience Utilisateur (UI/UX)

## 1. La Philosophie : "L'Application Métier comme Outil de Précision"

Le design de Skooly refuse le minimalisme vide. Nous cherchons la **densité utile** inspirée par des outils comme Linear ou les terminaux Bloomberg.

### Principes Directeurs
*   **Vitesse Perçue** : Utilisation d'Optimistic Updates (le changement s'affiche avant même la confirmation serveur).
*   **Zéro Perte de Contexte** : Utilisation intensive de tiroirs (Drawers) latéraux au lieu de naviguer entre des pages entières.
*   **Navigation au Clavier** : Support complet des raccourcis clavier et d'une barre de commande (`Cmd+K`).

---

## 2. Dualité de l'Écosystème

Skooly se divise en deux interfaces distinctes répondant à des besoins différents.

### A. La Vitrine (www.skooly.io)
*   **Objectif** : Marketing et conversion.
*   **Ton** : Innovant, sécurisant, inspirant.
*   **Action** : Parcours d'inscription institutionnelle (Onboarding).

### B. L'Espace Opérationnel (app.skooly.io)
*   **Inspiration** : Apple Launchpad / Odoo Enterprise.
*   **Dashboard Modulaire** : Une grille d'applications. Chaque utilisateur voit uniquement les outils liés à son rôle (Finance ne voit pas HR).
*   **Ergonomie** : Filtres avancés persistants, tri multi-colonnes et vues personnalisables (Liste ou Kanban).

---

## 3. Tokens de Design (Identité Visuelle)

Nous utilisons une palette sobre et institutionnelle garantissant la lisibilité sur tout type d'écran.
*   **Primaire** : Bleu Profond (Sérieux académique).
*   **Accent** : Emeraude (Actions de succès, Validation).
*   **Alerte** : Corail (Dettes, Erreurs, Alertes IA).
*   **Typographie** : `Geist Sans` pour sa clarté technique et sa lisibilité à petite taille.

---

## 4. Mobile First : Approche Responsive
Bien que Skooly soit un ERP complexe, il doit fonctionner parfaitement sur mobile pour les cas d'usage terrain :
*   **Professeur** : Prise de présence en un clic.
*   **Étudiant** : Consultation de ses notes et paiement.
*   **Admin** : Notification d'urgence et validation de documents.
