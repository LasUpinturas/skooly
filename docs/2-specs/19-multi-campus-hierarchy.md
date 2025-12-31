# 🏛️ Architecture Multi-Niveaux : L'Empire Universitaire

> **Le Cas d'Usage** : L'Université de Douala (UD) est la "Holding".
> L'IUT, l'ENSET, et la FSEGA sont des "Filiales".
> Chaque filiale est autonome (son propre LMD, ses propres règles).
> Mais la Holding veut voir les chiffres globaux.

---

## 1. Modèle de Données Hiérarchique

On ne fait pas juste un `Tenant`. On fait un **Arbre de Tenants**.

### Le Modèle `Organization`
*   **Type**: `HOLDING` (UD) ou `SCHOOL` (IUT).
*   **ParentId**: L'IUT a pour parent `Organization(UD)`.
*   **Path**: `materialized_path` (ex: `/UD/IUT/` ou `/UD/ENSET/`).

### L'Isolation des Données
*   **Isolation Stricte (Down-Up)** : L'IUT ne voit JAMAIS les données de l'ENSET. (Concurrents internes).
*   **Visibilité Descendante (Top-Down)** : L'UD peut voir les données agrégées de l'IUT (Reporting).

---

## 2. Le Super-Dashboard (Vue Recteur)

Quand le Recteur de l'UD se connecte, il ne voit pas une école. Il voit **Le Groupe**.

### A. Consolidation Financière
*   IUT : 50M FCFA
*   ENSET : 30M FCFA
*   **Total Groupe : 80M FCFA** (Calculé à la volée via une vue SQL matérialisée).

### B. Mobilité Étudiante (Transfuge)
Si un étudiant passe de la Licence Info (IUT) au Master Gestion (FSEGA).
*   Il garde le **Même Matricule Universitaire** (Unique au niveau Holding).
*   Son dossier "IUT" est archivé.
*   Son dossier "FSEGA" est actif.
*   L'historique est conservé au niveau Holding.

---

## 3. Le Partage de Ressources (Mutualisation)

Certaines ressources coûtent cher et sont partagées.

*   **Campus Partagé** : L'Amphi 1000 est géré par l'UD, mais réservable par l'IUT et l'ENSET.
*   **Enseignants Partagés** : Dr. Talla enseigne à l'IUT et à l'ENSET.
    *   Il a un seul compte User.
    *   Il a deux "Profils Employé" (Un par Tenant), mais un seul Planning consolidé.

---

## 4. Implémentation Technique (Row Level Security)

Comment on code ça sans devenir fou avec les `WHERE` ?

*   **Tenant Context** : Dans chaque requête, on injecte `CurrentTenantId`.
*   **Si User est Admin École** : `WHERE tenant_id = current_id`.
*   **Si User est Recteur** : `WHERE tenant_id IN (sub_tenants_of(UD))`.

C'est géré par le **Core Module** (Middleware), pas par chaque développeur.
