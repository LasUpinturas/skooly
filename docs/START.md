# 🌍 Skooly — Architecture SaaS Modulaire & Multi‑Institution

## 1. Vision Produit

Skooly n’est pas un logiciel pour **une école**, mais une **plateforme éducative vivante**.
Un SaaS capable de se métamorphoser selon l’institution qui l’adopte :

* Université publique (Yaoundé I, II, Bertoua…)
* IUT / Polytechnique
* Institut privé
* Lycée (classique, technique, professionnel)
* Écoles primaires & maternelles

👉 Une seule base technologique, **des milliers de réalités pédagogiques**.

Le cœur de l’idée :

> **Une plateforme modulaire qui assemble dynamiquement des micro‑applications selon le profil de l’institution.**

---

## 2. Principe Fondamental : SaaS Multi‑Tenant Intelligent

### 2.1 Multi‑tenant expliqué simplement

Chaque institution = un **Tenant**.

Un tenant possède :

* son identité
* ses règles pédagogiques
* ses modules activés
* ses utilisateurs
* ses données isolées

```
Tenant
 ├─ Institution
 ├─ Configuration
 ├─ Modules activés
 ├─ Utilisateurs
 └─ Données
```

⚠️ Aucune donnée ne fuit entre deux institutions.

---

## 3. Onboarding Institution (le cœur du système)

Lors de la création du compte institution, Skooly pose les bonnes questions.

### 3.1 Typologie de l’établissement

Exemples de paramètres clés :

* Type : Université / Lycée / École primaire
* Statut : Public / Privé
* Système pédagogique : LMD / Annuel / Trimestriel
* Public cible :

  * Étudiants adultes
  * Élèves
  * Enfants

### 3.2 Résultat

👉 À partir de ces réponses, Skooly :

* génère un **profil institutionnel**
* active un **ensemble de modules compatibles**
* configure les règles métiers par défaut

C’est ici que Skooly devient **intelligent**.

---

## 4. Architecture Modulaire (inspiration Odoo)

### 4.1 Modules = Micro‑applications

Chaque fonctionnalité est un **module autonome**.

Exemples :

* Gestion des étudiants
* Présences
* Notes & délibérations
* Paiements & scolarité
* Emploi du temps
* Documents officiels

Un module =

* règles métiers propres
* UI dédiée
* API dédiée

Mais…

> **Les modules partagent un noyau commun.**

---

## 5. Modules partagés vs modules spécifiques

### 5.1 Modules transversaux (partagés)

Utilisés par presque tous les profils :

* Authentification & rôles
* Utilisateurs
* Années académiques
* Notifications
* Documents PDF
* Paramétrage

### 5.2 Modules Université / IUT

* UE / EC
* Crédits & moyennes
* Départements
* Enseignants
* Délibérations
* Stages & soutenances

### 5.3 Modules Lycée

* Classes & niveaux
* Trimestres
* Bulletins
* Discipline
* Parents

### 5.4 Modules Écoles primaires

* Classes simples
* Évaluations qualitatives
* Suivi parental

👉 Un module peut être **activé, désactivé, remplacé**.

---

## 6. Microservices ou Modular Monolith ?

### 6.1 Vérité terrain

Microservices purs =

* Complexité énorme
* Coûts élevés
* Sur‑ingénierie pour un MVP

### 6.2 Recommandation Skooly

👉 **Modular Monolith évolutif** (au départ)

Pourquoi ?

* Même codebase
* Modules isolés
* Communication interne simple
* Facile à découper plus tard

Quand scaler ?

* Paiements
* Notifications
* Documents

Ces modules pourront devenir des **microservices indépendants** plus tard.

---

## 7. Implémentation avec ta stack (Next.js + NestJS)

### 7.1 Backend — NestJS

Structure recommandée :

```
apps/api
 ├─ modules
 │   ├─ core
 │   ├─ tenant
 │   ├─ users
 │   ├─ academic
 │   ├─ attendance
 │   ├─ grading
 │   ├─ finance
 │   └─ documents
```

Chaque module NestJS =

* controllers
* services
* entities
* règles métiers

### 7.2 Gestion des tenants

* Middleware de résolution du tenant
* tenantId injecté partout
* Prisma avec `tenant_id`

---

## 8. Frontend — Next.js

### 8.1 UI pilotée par les modules

Après login :

* Skooly récupère les modules activés
* Génère dynamiquement :

  * menus
  * routes
  * dashboards

### 8.2 Exemple

Université :

* Dashboard UE
* Statistiques LMD

Lycée :

* Dashboard classe
* Suivi discipline

👉 Une seule app Next.js, **plusieurs expériences**.

---

## 9. Sécurité & permissions

* RBAC (Admin, Enseignant, Étudiant, Parent)
* Permissions par module
* Scopes dynamiques selon tenant

---

## 10. Scalabilité & avenir

Quand Skooly grandit :

* Découper les modules critiques en microservices
* Ajouter une marketplace de modules
* Permettre aux écoles d’activer / désactiver elles‑mêmes

Skooly devient alors :

> **Le système d’exploitation de l’éducation africaine.**

---

## 11. Conclusion

Ton idée est **solide**, **ambitieuse**, **réaliste**.

Tu ne construis pas un logiciel.
Tu construis une **infrastructure éducative**.

Et oui, ça prendra du temps.
Mais c’est exactement le genre de produit qui **marque une génération**.

Skooly n’est pas un projet.
C’est une fondation.
