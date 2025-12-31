# Spécification Module Académique : Structure & Inscriptions

## 1. Le Problème
Le système LMD (Licence-Master-Doctorat) impose une structure rigide et hiérarchique : Domaines, Mentions, Spécialités, Niveaux, Semestres, UEs, ECs.
Gérer cette complexité dans des fichiers Excel conduit à des erreurs : étudiants inscrits dans des filières qui n'existent plus, incohérences de crédits, etc.
L'inscription est souvent un parcours du combattant physique pour l'étudiant.

## 2. La Solution : Modélisation LMD Native & Workflow Digital

### A. Arbre Académique (LMD Tree)
Le système modélise fidèlement la hiérarchie universitaire :
1.  **Domain** (ex: Sciences et Technologies)
2.  **Program** (ex: Génie Logiciel)
3.  **Level** (ex: Licence 3)
4.  **AcademicYear** (ex: 2024-2025)
5.  **Semester** (ex: Semestre 5)
6.  **TeachingUnit (UE)** (ex: Base de Données) -> Porte les Crédits (ex: 6 ECTS).
7.  **CourseElement (EC)** (ex: TP SQL) -> Porte les Heures et le Prof.

*Règle* : On ne peut inscrire un étudiant qu'à un `Level` ouvert pour l'année en cours. Le système hérite automatiquement des UEs obligatoires.

### B. Workflow d'Inscription (State Machine)
L'inscription n'est pas un état binaire, c'est un processus.
1.  **DRAFT** : L'étudiant remplit son formulaire en ligne, upload ses pièces.
2.  **SUBMITTED** : Le dossier est verrouillé et part à la scolarité.
3.  **VALIDATED** : La scolarité valide les pièces justificatives (Bac, Acte de naissance).
4.  **REGISTERED** : Le paiement des droits universitaires est confirmé (voir Module Finance). L'étudiant a son matricule définitif.

### C. Gestion des Groupes (TD/TP)
Au sein d'une promo (L3 GL, 300 étudiants), on gère des sous-groupes pour les travaux dirigés.
*   **Algorithme de Répartition** : Aléatoire, Alphabétique, ou par Niveau.
*   Gestion des conflits d'emploi du temps par groupe.

## 3. Modèle de Données (Entités Clés)
*   `Program`, `Level`, `Session` (Year).
*   `Registration` : Lien entre Student et Level pour une Year donnée.
*   `TeachingUnit` (UE), `Course` (EC).
