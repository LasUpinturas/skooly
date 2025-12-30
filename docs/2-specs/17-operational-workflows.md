# ⚙️ Workflows Opérationnels : Le Cycle de Vie des Données

> Ce document répond à la question : "Concrètement, comment les données entrent, bougent et changent dans le système ?"

---

## 🏗️ 1. La Genèse : Créer l'Écosystème (Admin Setup)

Avant d'inscrire des élèves, il faut bâtir les murs.

### Q: Comment créer un nouveau Département / Salle ?
**Réponse :** Approche Top-Down (Hiérarchique).

1.  **Institution Setup** (Fait une fois) : Création du Tenant.
2.  **Infrastructure Physique** :
    *   Création des **Campus** -> **Bâtiments** -> **Salles** (`Classroom`).
    *   *Attributs Salle* : Capacité (50 places), Type (Labo/Amphi), Équipement (Projecteur).
3.  **Infrastructure Académique** :
    *   **Département** ("Génie Info") -> **Program** ("Licence GL").
    *   **Structure** : Définition des UEs et ECs pour l'année.

**Le Workflow "Rentrée Académique" :**
L'admin clique sur **"Dupliquer année N-1"**.
Tout est cloné : les filières, les cours, les salles. Il n'a plus qu'à ajuster les petits changements.

---

## 👥 2. L'Onboarding Humain (Peupler le Système)

### Q: Comment enregistrer le Personnel (Enseignants, Cadres) ?
**Réponse :** Invitation par Email (Flow Sécurisé).

1.  **RH initie** : Saisie Email + Rôle ("Enseignant") + Département.
2.  **Système** : Envoie un lien d'invitation unique (Magique).
3.  **Utilisateur** : Clique -> Définit son mot de passe -> Complète son profil (Photo, RIB, Bio).
4.  **Validation** : Le RH valide le profil complet -> `Status: ACTIVE`.

*Pourquoi pas de création manuelle par l'Admin ?* Pour éviter les erreurs de saisie de mot de passe et responsabiliser l'utilisateur.

### Q: Comment enregistrer les Étudiants ?
**Réponse :** Deux voies.
1.  **Masse (Première fois)** : Import Excel via le *Wizard* (`Module Data Management`).
2.  **Au fil de l'eau (Candidats)** :
    *   Le candidat crée un compte "Prospect" sur le portail public.
    *   Il paie ses frais de concours (Mobile Money).
    *   Si admis, son compte "Prospect" mute en compte "Étudiant".

---

## 📆 3. L'Orchestration (Assignation des Ressources)

### Q: Comment assigner une Salle à un Programme ?
**Réponse :** C'est le module **Scheduling**.

Le système ne lie pas "Une salle à un programme". Il lie :
> **Session Cours** = ( **Matière** + **Enseignant** + **Salle** + **Groupe Étudiants** + **Créneau** )

**Le Workflow :**
1.  Le Responsable Pédagogique ouvre la vue "Planning L3 Info".
2.  Il glisse l'UE "Java" sur le créneau "Lundi 8h".
3.  **Le Système (Conflict Solver)** :
    *   Vérifie si le Prof est libre.
    *   Vérifie si la Salle est libre.
    *   Suggère la meilleure salle (capacité >= taille du groupe).
4.  **Réservation** : La salle est bloquée ("Booked").

---

## 🔄 4. La Synchronisation Externe (Le Cas du Paiement UBA)

### Q: Comment le système sait qu'un élève a payé si c'est géré par UBA ?
**Réponse :** Le principe de la **Réconciliation Asynchrone**.

Le système ne "sait" pas instantanément. Il "apprend".

**État Initial :**
*   Étudiant : Inscrit.
*   Facture Scolarité : 50,000 FCFA.
*   Status : `WAITING_PAYMENT`.
*   Droits : Accès limité (Pas de carte, pas de certificat).

**L'Événement Déclencheur (Le Pont UBA) :**
1.  L'étudiant paie à la banque. Il reçoit un reçu papier.
2.  Le soir, UBA transmet un fichier (Excel/API) à l'Université.
3.  **Job de Nuit Skooly** :
    *   Lit le fichier UBA.
    *   Cherche le Matricule dans le fichier.
    *   Trouve la Facture correspondante.
    *   Crée un `Payment` interne.
    *   Passe la Facture à `PAID`.

**Conséquence (Event Driven) :**
*   L'événement `InvoicePaid` est émis.
*   Le module **AccessControl** écoute -> Débloque l'impression de la carte.
*   Le module **Notification** écoute -> Envoie un SMS "Paiement reçu ✅".

### Q: Et si l'étudiant ment (Faux reçu) ?
Il peut uploader une photo de faux reçu dans l'app, mais le statut restera `PENDING_VERIFICATION` tant que le fichier officiel de la banque ne confirme pas.
**La banque est la seule source de vérité.**
