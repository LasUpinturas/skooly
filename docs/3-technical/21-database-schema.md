# Spécification Architecture Base de Données

## 1. Philosophie de Modélisation
La base de données est le socle de l'intégrité de Skooly. Nous utilisons PostgreSQL pour sa robustesse et sa gestion avancée des relations.

### A. Intégrité Référentielle Stricte
Toutes les relations sont protégées par des clés étrangères (`Foreign Keys`). Aucune donnée orpheline n'est tolérée.
*   **On Delete Restrict** : On ne peut pas supprimer un étudiant qui a des notes ou des paiements liés.

### B. Multi-Tenancy (Isolation)
Chaque table contenant des données spécifiques à une institution possède une colonne `tenantId`.
*   Un index est systématiquement présent sur cette colonne pour garantir des performances optimales lors du filtrage par établissement.

---

## 2. Schémas Principaux (Architecture Core)

### Identité (IAM)
*   `User` : Données de connexion.
*   `Partner` : Identité physique unique.
*   `Role` / `Permission` : Matrice des droits.

### Académique (LMD)
*   `Program` -> `Level` -> `Registration`.
*   `TeachingUnit` (UE) -> `Course` (EC).
*   `Grade` : Table des notes avec lien vers l'Anonymat.

### Finance (Audit)
*   `Invoice` : Facture émise.
*   `Payment` : Versement reçu.
*   `LedgerEntry` : Écriture comptable immuable.

---

## 3. Optimisation pour la Lecture
Pour les rapports complexes (ex: Moyennes générales de 10 000 élèves), nous utilisons des **Vues SQL Matérialisées** ou des tables d'agrégation indexées pour éviter de recalculer les données à chaque consultation.
