# Spécification Module Emploi du Temps (Scheduling)

## 1. Le Problème
L'organisation des cours dans une université de grande taille est un casse-tête logistique.
*   **Conflits de Ressources** : Deux cours prévus dans la même salle au même moment, ou un enseignant assigné à deux amphis simultanément.
*   **Immobilité** : Toute modification (absence d'un prof, salle indisponible) demande des heures de réajustement manuel.
*   **Visibilité** : Les étudiants et enseignants n'ont pas accès à leur planning en temps réel, menant à des déplacements inutiles.

## 2. La Solution : Moteur de Placement et Calendriers Dynamiques

### A. Gestion des Contraintes
Le système modélise les entités comme des ressources avec des disponibilités :
*   **Salles** : Capacité, Équipements (Projecteur, Labo), Localisation.
*   **Enseignants** : Charge horaire maximale, Créneaux d'indisponibilité.
*   **Groupes d'Étudiants** : Parcours académique, Taille du groupe.

### B. Moteur de Détection de Conflits
Lors de la création d'un événement (Session de cours), le système vérifie en temps réel :
1.  **Collision Professeur** : L'enseignant est-il déjà en cours ?
2.  **Collision Salle** : La salle est-elle libre ?
3.  **Adéquation Capacité** : La salle peut-elle accueillir tous les étudiants du groupe ?

### C. Diffusion Multi-Canal
L'emploi du temps n'est plus une affiche sur un mur.
*   **Dashboard Personnel** : Chaque utilisateur voit son propre calendrier (Format "Google Calendar").
*   **Notifications Push** : Alerte immédiate sur mobile en cas de changement de salle ou d'annulation de cours.
*   **Export iCal** : Synchronisation avec les calendriers personnels (Outlook, Apple Calendar).

## 3. Modèle de Données
*   `TimetableSlot` : Définition des créneaux de l'établissement.
*   `CourseSession` : L'événement réunissant Matière, Prof, Salle et Groupe.
*   `ResourceCollision` : Log des tentatives de placement invalides.
