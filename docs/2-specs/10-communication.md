# Spécification Module Hub de Communication

## 1. Le Problème
La communication au sein de l'université est souvent fragmentée et inefficace.
*   **Silos d'Information** : Les annonces importantes se perdent dans des groupes WhatsApp non officiels ou sur des panneaux d'affichage physiques.
*   **Manque de Ciblage** : Difficile d'envoyer un message uniquement aux "Étudiants en Master 2 ayant une dette de scolarité".
*   **Pas de Preuve de Lecture** : L'administration ne sait pas si une annonce critique a été vue.

## 2. La Solution : Hub de Notification Centralisé & Multi-canal

### A. Segmentation Intelligente
L'administration peut envoyer des messages ciblés basés sur les données réelles du système :
*   Par **Filière/Niveau** (ex: "Annulation de cours en L3 Info").
*   Par **Statut Financier** (ex: "Relance impayés").
*   Par **Rôle** (ex: "Réunion urgente des enseignants").

### B. Omnicanalité (SMS, Email, Push)
Le système choisit le meilleur canal selon l'urgence et le profil :
*   **Push App** : Gratuit, instantané, parfait pour le quotidien.
*   **SMS** : Payant, mais garantit la réception même sans internet (Urgence, Sécurité).
*   **Email** : Classique, pour les documents longs et formels.

### C. Confirmation de Réception
Pour les annonces légales ou disciplinaires, le système suit :
*   **Délivré** (Ack technique).
*   **Lu** (Ouverture de la notification).
*   **Approuvé** (Si le message demande une confirmation explicite).

## 3. Modèle de Données
*   `NotificationTemplate` : Messages pré-enregistrés.
*   `Broadcast` : Campagne de communication envoyée à un segment.
*   `UserFeed` : Historique personnel des messages reçus.
