# Protocole de Fonctionnement Hors-Ligne (Offline)

## 1. Le Problème
La connectivité internet peut être instable. Un ERP éducatif ne peut pas s'arrêter de fonctionner parce que le wifi a l'université a coupé. Le personnel doit pouvoir continuer à marquer les présences ou saisir des notes sans interruption.

## 2. La Solution : PWA & Synchronisation Optimiste

### A. L'Architecture PWA (Progressive Web App)
Skooly utilise les technologies web modernes pour fonctionner sans connexion native :
*   **Service Workers** : Mise en cache locale de l'application (Assets) permettant son chargement initial même sans réseau.
*   **IndexedDB** : Utilisation d'une base de données locale dans le navigateur pour stocker les données de travail immédiates (Liste d'appel, relevé de notes).

### B. Workflow de Synchronisation
1.  **Action Offline** : L'utilisateur effectue une modification. L'UI réagit instantanément (Mise à jour optimiste) et stocke l'action dans une file d'attente (Sync Queue).
2.  **Détection de Connexion** : Le système surveille l'état du réseau en continu.
3.  **Réconciliation** : Dès le retour du signal, les actions en attente sont envoyées au serveur par ordre chronologique.

---

## 3. Gestion des Conflits de Données

Dans un système décentralisé, des conflits peuvent survenir (ex: deux personnes modifient la même note en même temps offline).

**Règle de Résolution : "Server Authority"**
Pour garantir l'intégrité académique, le serveur reste la source de vérité finale.
*   Si un conflit est détecté (Version mismatch), le serveur rejette la modification offline.
*   L'utilisateur reçoit une notification de conflit et doit choisir manuellement la version à conserver après avoir rafraîchi ses données.

## 4. Périmètre de Fonctionnement Dégradé
Le mode offline est restreint aux opérations de saisie de données. Les fonctions de consultation de rapports massifs ou de paiements en temps réel (Mobile Money) sont désactivées tant que la connexion n'est pas rétablie.
