# Spécification Module Gestion des Données & Onboarding

## 1. Le Problème
Migrer d'un ancien système (ou de fichiers Excel) vers un nouvel ERP est l'étape la plus risquée d'un projet.
*   **Données Sales** : Doublons, matricules manquants, formats de date incohérents.
*   **Résistance au Changement** : Si l'import des données prend des semaines, les utilisateurs rejettent le produit.

## 2. La Solution : Smart Import Wizard & Nettoyage ETL

### A. Assistant d'Import Intelligent
Skooly propose une interface "Pas-à-Pas" pour importer les données :
1.  **Mapping** : L'utilisateur lie les colonnes de son fichier Excel aux champs Skooly.
2.  **Validation Temps-Réel** : Le système affiche immédiatement les erreurs (ex: "Ligne 45 : Date de naissance invalide").
3.  **Nettoyage Automatique** : Normalisation des noms (MAJUSCULES), des numéros de téléphone (+237...).

### B. Gestion des Doublons
Un algorithme de "Fuzzy Matching" détecte les étudiants potentiellement identiques (même nom, même date de naissance) pour éviter les matricules multiples.

### C. Mass Editing (Édition en Masse)
Pour corriger des erreurs globales (ex: changer le code d'une UE pour tout un semestre), l'administrateur peut effectuer des modifications groupées sans ouvrir chaque fiche individuellement.
