# 📥 Module Data Management : L'Onboarding Industriel

## Le Problème de l'Oeuf et la Poule
Skooly est vide. L'école a 50 ans d'archives Excel.
Si l'import est difficile, le projet échoue à J-1.

## 1. Import Wizard (Le Migrateur)

### Architecture ETL (Extract-Transform-Load)
On ne demande pas à l'utilisateur de mapper ses colonnes à la main.
On utilise des **"Smart Importers"**.

1.  **Upload** : L'utilisateur glisse son fichier `Liste_Etudiants_2024.xlsx` (même moche, avec des cellules fusionnées).
2.  **Analyze** : Le backend parse le fichier (SheetJS) et détecte les headers ("Nom", "Prénoms", "Date Naissance").
3.  **Map** : UI de Mapping. "La colonne 'Ddn' correspond à `student.birth_date`".
4.  **Validate** :
    *   Test à blanc. "Ligne 45 : Email invalide".
    *   "Ligne 98 : Matricule doublon".
5.  **Commit** : Import réel en base (Atomic Transaction).

### Templates Supportés
*   Étudiants (avec Photo via ZIP).
*   Enseignants.
*   Structure Académique (Arbre LMD).
*   Historique des Notes (Pour générer les relevés passés).

## 2. Mass Edit (L'Excel-Killer)

Parfois, il faut corriger 50 étudiants d'un coup.
Skooly intègre une **Grid View** éditable (façon Airtable).

*   Sélectionner 50 lignes -> Clic droit -> "Changer le status à 'Inscrit'".
*   Copier-Coller depuis Excel direct dans le navigateur.

## 3. Data Archiving

L'école ne veut pas voir les données de 2012 tous les jours.
*   **Active Data** : Année académique courante.
*   **Archived Data** : Accessible en lecture seule ("Mode Archive").
*   **Cold Storage** : Export JSON sur S3 Glacier pour les données > 10 ans.
