# 🧠 Module IA : L'Invisible Assistant

## Strategie : Augmenter, pas Remplacer
Skooly n'utilise pas de "Magic AI" qui décide à la place des humains.
Skooly utilise l'IA pour **surveiller, alerter et suggérer**.

## 1. Anti-Fraude (Computer Vision)

### Le Problème
Les étudiants uploadent des faux matricules, des fausses photos, ou des faux scans de reçus de banque.

### La Solution Skooly
*   **Document Analysis** :
    *   Vérification de la cohérence des pixels (Décection Photoshop).
    *   OCR du reçu de banque -> Comparaison avec le montant déclaré.
*   **Attendance Anti-Spoofing** :
    *   Si l'étudiant utilise la reconnaissance faciale, on vérifie la "Liveness" (qu'il ne scanne pas une photo).

## 2. Détection du Décrochage (Predictive Analytics)

### Le Modèle "At-Risk"
Chaque nuit, un job Python (Sidecar) analyse :
*   La baisse de la moyenne.
*   L'augmentation des absences.
*   Le retard de paiement.

Si le score dépasse 0.7, une alerte est envoyée au **Conseiller d'Orientation** :
"Attention, l'étudiant X risque d'abandonner dans les 3 semaines."

## 3. Assistant Administratif (LLM / RAG)

### Le Problème
Les secrétaires passent leur vie à répondre : "Combien d'étudiants en L2 ?"

### La Solution
Un chat interface "Skooly Bot" connecté à la base de données (read-only) via un moteur RAG.
*   **User** : "Donne moi la liste des étudiants insolvables en Génie Logiciel."
*   **Skooly** : Génère la requête SQL -> Affiche le tableau.

## Architecture Technique IA
On ne met pas le code Python Lourd dans l'API NestJS.
On utilise un microservice **Skooly-Brain** (FastAPI) qui communique via RabbitMQ.
