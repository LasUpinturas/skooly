# Spécifications Module IA : Anti-Fraude & Aide à la Réussite

## 1. Le Problème
Avec des milliers d'étudiants, l'humain ne peut plus tout surveiller.
*   **Fraude Indétectable** : Des patterns de fraude aux notes ou aux présences passent inaperçus dans la masse.
*   **Échec Prévisible mais Non Vu** : Un étudiant qui va échouer montre des signes (absences répétées, notes en baisse) des mois avant l'examen, mais personne n'a le temps d'agréger ces données.

## 2. La Solution : L'IA comme Assistant de Vigilance

### A. Détection d'Anomalies (Anti-Fraude)
Algorithmes analysant les patterns suspects :
*   **Notes Suspectes** : Alerte si une note est modifiée plusieurs fois par le même utilisateur sur un court laps de temps, ou si une moyenne de classe est statistiquement impossible.
*   **Fraude à la Présence** : Détection de scans de QR Code provenant de comptes différents mais sur le même ID de mobile physique.

### B. Algorithme de Prédiction du Décrochage (Student Success)
Un score de risque est calculé pour chaque étudiant :
*   Variables : Assiduité, Historique académique, Temps de retard de paiement.
*   *Action* : Si le risque dépasse 70%, le conseiller pédagogique reçoit une alerte pour convoquer l'étudiant.

### C. Smart Import Assistant
Utilisation de LLM (IA générative) pour aider l'administration à "mapper" des fichiers Excel mal structurés venant des anciens systèmes vers la structure Skooly.

## 3. Éthique & Transparence
L'IA dans Skooly ne prend JAMAIS de décision seule (pas d'exclusion automatique). Elle émet uniquement des **alertes** qu'un humain doit valider après enquête.
