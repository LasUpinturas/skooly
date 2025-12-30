# 📚 Module Library : Le Savoir Organisé

## Physique & Numérique
La bibliothèque moderne prête des livres ET des accès PDF/Ebooks.

## Entités Principales

### 1. `Resource`
*   `type`: `BOOK`, `EBOOK`, `LAPTOP`, `THESIS`.
*   `isbn`: Code barre scannable via app mobile (Caméra).
*   `stock`: Nombre d'exemplaires disponibles.

### 2. `Loan` (Emprunt)
*   **Workflow** : `REQUESTED` -> `ISSUED` (Check-out) -> `RETURNED` (Check-in).
*   **Late Fees** : Calcul automatique des pénalités (100 FCFA / jour).
    *   Les pénalités s'ajoutent à la "Dette financière" de l'étudiant.
    *   Il ne peut pas s'inscrire l'année suivante tant qu'il doit 500 FCFA à la biblio. (Intégration Finance).

## Fonctionnalités Avancées
*   **Réserve Professeur** : Un prof met 10 livres "De Côté" pour son cours. Ils deviennent non-empruntables (Consultation sur place).
*   **Dépôt Mémoires** : Les étudiants uploadent leur mémoire (PDF) qui devient une ressource `THESIS` consultable par les futurs étudiants (si note > 14).
