# Spécification Module Core : Identité & Structure

## 1. Le Problème
La gestion des identités dans une université est complexe car une même personne peut avoir plusieurs casquettes (Étudiant en L3, Tuteur en L1, Membre du BDE).
De plus, la structure de l'établissement (Campus, Salles, Départements) est la colonne vertébrale sur laquelle tout le reste s'appuie. Une mauvaise modélisation ici rend le système rigide.

## 2. La Solution : Séparation Identité/Accès et Multi-Tenancy

### A. Modèle "Partner vs User" (Inspiré d'Odoo)
Nous séparons la personne physique de son compte utilisateur.
*   **Partner (Entité)** : Représente "Jean Dupont". Contient photo, email, téléphone, bio. Unique dans le système.
*   **User (Compte)** : C'est l'identifiant de connexion (Login/Pass). Un Partner peut avoir un User.
*   **Role (Casquette)** : Un Partner peut être lié à un profil "Student" ET un profil "Employee".

*Avantage* : Si un étudiant devient vacataire, on ne crée pas deux personnes. On ajoute juste un rôle à son Partner existant.

### B. Multi-Tenancy Hiérarchique
Veuillez vous référer à la spécification `19-multi-campus-hierarchy.md` pour les détails.
Le Core Module implémente l'isolation stricte des données par `tenant_id` sur toutes les tables sensibles.

### C. Gestion des Accès (RBAC)
Le contrôle d'accès est basé sur des Rôles et des Permissions granulaires.
*   **Permissions** : `student.create`, `grade.view`, `finance.validate`.
*   **Rôles** : Groupes de permissions (ex: "Scolarité" = `student.*` + `grade.view`).
*   **Scope** : Les permissions sont scopées par Département ou par Campus (ex: "Responsable Info" ne peut pas modifier les notes de "Gestion").

## 3. Modèle de Données (Entités Clés)
*   `User` : Auth credentials (hash password, last login).
*   `Partner` : Données profile (name, phone, avatar).
*   `Tenant` : Établissement (name, domain, config).
*   `Role` : Définition des accès.
