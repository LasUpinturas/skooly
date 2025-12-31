# Spécification Module Présences : Anti-Fraude & Suivi

## 1. Le Problème
L'absentéisme est un fléau, mais le suivi manuel (feuilles volantes) est inefficace.
*   **Perte de temps** : L'appel prend 15 minutes sur 2h de cours.
*   **Fraude** : "Signe pour moi". Un étudiant signe pour 5 amis absents.
*   **Saisie** : Les fiches ne sont jamais ressaisies numériquement. Aucune statistique.

## 2. La Solution : QR Code Dynamique & Géolocalisation

### A. Le "Live QR Code" (Côté Prof)
Le professeur projette un QR Code sur le vidéoprojecteur (ou son téléphone).
*   **Sécurité** : Le QR Code est **rotatif** (change toutes les 5 secondes). Il est signé cryptographiquement (TOTP).
*   *Impact* : Impossible de prendre une photo du QR et de l'envoyer sur WhatsApp au groupe. Le temps de l'envoyer, il est expiré.

### B. Le Scan Sécurisé (Côté Étudiant)
L'étudiant scanne avec l'App Skooly.
*   **Géolocalisation** : L'app vérifie que le GPS du téléphone correspond aux coordonnées de l'Amphi.
*   **Device Fingerprint** : Impossible de connecter 2 comptes sur le même téléphone pour scanner deux fois.

### C. Workflow Administratif
1.  **Session Ouverte** : Le prof lance le cours.
2.  **Pointage** : Les étudiants scannent.
3.  **Clôture** : Le prof ferme la session.
4.  **Régularisation** : Les étudiants sans smartphone ou batterie se signalent au prof qui les marque manuellement (Audit logué).

## 3. Analytics
Le système génère des alertes automatiques :
*   "Étudiant X a dépassé le seuil de 30% d'absences dans l'UE Java".
*   Conséquence : Blocage automatique de la convocation à l'examen.
