# 📡 Stratégie Offline : Survivre sans Réseau

> **Le Contexte** :
> L'Université de Dschang a des coupures de courant. L'IUT de Douala a des zones blanches (sous-sols).
> Skooly **DOIT* marcher sans internet.

---

## 1. L'Approche PWA (Progressive Web App)

Même sans app mobile native, le navigateur fait le job.

### A. Le Service Worker (Le Gardien)
*   **Asset Caching** : Au premier chargement, on télécharge tout le CSS/JS/Fonts.
*   **Comportement** :
    *   Si Online : On tape le serveur + on met à jour le cache (Strategy: *Stale-while-revalidate*).
    *   Si Offline : On sert le cache instantanément. L'app se charge en 0.5s même en mode avion.

### B. Le Data Caching (TanStack Query)
On utilise `PersistQueryClient` avec **IndexedDB**.
*   Quand un prof charge sa liste d'élèves, elle est sauvée en local.
*   S'il revient 2h plus tard sans internet, on affiche la liste stockée.
*   *TTL (Time To Live)* : On garde les données 24h. Au-delà, on force un refresh (ou on affiche un warning "Données périmées").

---

## 2. L'Optimistic Updates (L'Illusion de Vitesse)

C'est le secret pour une UX fluide.

**Scénario : Le Prof note un élève.**
1.  **Action** : Prof tape "15/20" et valide.
2.  **UI Immédiate** : La case devient verte ✅. Le prof passe au suivant.
3.  **Back-office (Invisible)** :
    *   La requête `POST /grades` est mise dans une **Sync Queue** (IndexedDB).
    *   Le Service Worker tente de l'envoyer.
    *   *Si Offline* : La requête reste dans la queue. "1 élément en attente de sync".
    *   *Si Online* : La requête part.

---

## 3. Conflict Resolution (La Bagarre)

Que se passe-t-il si 2 personnes modifient la même donnée offline ?

**Scénario** :
*   Admin (Online) change le nom de l'élève en "Talla".
*   Prof (Offline) note l'élève "Tala".
*   Prof revient Online.

### La Stratégie V1 : "Server Wins" (Sécurité)
Si le serveur détecte que la donnée a changé depuis la dernière lecture du client, il **rejette** l'écriture offline avec une erreur `409 Conflict`.
*   **UX** : Une notification apparaît chez le Prof : *"Conflit de version sur l'étudiant Talla. Veuillez rafraîchir."*
*   C'est chiant, mais c'est **Safe**. On ne corrompt pas la donnée.

### La Stratégie V2 (Futur) : "Last Write Wins" (Risqué)
On écrase tout. (À éviter pour les notes).

---

## 4. Limitation du Mode Offline

On ne peut pas tout faire sans internet.

| Feature | Offline ? | Comment ? |
| :--- | :---: | :--- |
| **Voir emploi du temps** | ✅ | Cache local (J-7 à J+7) |
| **Saisir des notes** | ✅ | Queue de Sync |
| **Faire l'appel (QR)** | ✅ | Le scan est stocké localement |
| **Payer (Mobile Money)** | ❌ | Impossible (Besoin API Opérateur) |
| **Générer un PDF** | ❌ | C'est le serveur qui génère |
| **Dashboards Finance** | ⚠️ | Read-only (Dernière version connue) |

---

## 5. Indicateur de Statut

L'utilisateur doit savoir où il habite.
*   🟢 **Online** : Tout va bien.
*   🟠 **Syncing...** : "Envoi de 3 notes..." (Spinner).
*   🔴 **Offline** : "Mode Hors Ligne. Vos modifications seront sauvées plus tard."
