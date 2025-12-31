# Processus Opérationnels et Cycle de Vie des Données

Ce document décrit comment les données sont créées et évoluent au sein de Skooly pour répondre aux réalités du terrain universitaire.

---

## 1. Mise en Place Institutionnelle (Setup)

### Problématique
Une configuration initiale complexe conduit à l'abandon de l'outil par les administrateurs.

### Solution
Skooly propose une approche hiérarchique :
1.  **Création du Tenant** (L'Établissement).
2.  **Configuration des Infrastructures** : Campus -> Bâtiments -> Salles.
3.  **Structure Académique** : Départements -> Filières -> Niveaux.
4.  **Initialisation** : Import des données historiques (voir module Data Management).

---

## 2. Gestion de l'Identité (IAM)

### Problématique
La multiplicité des adresses emails et l'oubli des mots de passe saturent le support technique.

### Solution
*   **Emails Hybrides** : Support des adresses institutionnelles et personnelles (Gmail/Yahoo) pour les vacataires.
*   **Magic Links** : Connexion sans mot de passe via lien sécurisé envoyé par email, réduisant drastiquement les demandes de reset.
*   **Rôles Cumulatifs** : Une même personne peut être Enseignant et Chef de Département sans avoir deux comptes séparés.

---

## 3. Synchronisation avec le Système Bancaire (UBA)

### Problématique
La fraude aux reçus de banque falsifiés est un manque à gagner majeur.

### Solution (Réconciliation Asynchrone)
1.  **Paiement Physique** : L'étudiant paie à la banque et reçoit un reçu papier.
2.  **Import des Flux** : L'administration importe quotidiennement le fichier officiel de la banque (UBA).
3.  **Appariement (Matching)** : Skooly lie automatiquement la transaction bancaire à la facture de l'étudiant via son Matricule.
4.  **Validation** : La facture passe au statut "Payée" uniquement après confirmation par le fichier de la banque. **La banque est la seule source de vérité.**

---

## 4. Audit et Traçabilité (Suivi)

Toute modification de donnée sensible (changement de note, annulation de facture) laisse une trace numérique indélébile contenant l'auteur, la date, l'ancienne valeur, la nouvelle valeur et le motif. Cette transparence décourage la fraude interne.
