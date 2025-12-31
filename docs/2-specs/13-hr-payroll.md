# Spécification Module Ressources Humaines & Paie

## 1. Le Problème
La gestion du personnel académique est singulière :
*   **Mix de Statuts** : Permanents (salaire fixe) et Vacataires (payés à l'heure effective).
*   **Comptage des Heures** : Le calcul de la paie des vacataires est souvent source de litiges car basé sur des fiches d'heures manuelles parfois erronées.
*   **Dossiers Éparpillés** : CV, diplômes et contrats des enseignants ne sont pas centralisés.

## 2. La Solution : Gestion de Carrière et Paie basée sur l'Activité

### A. Dossier Employé Centralisé
*   Suivi du cycle de vie : Recrutement, Contrat, Évaluations, Fin de fonction.
*   Gestion des grades académiques (Assistant, Chargé de cours, Maître de Conférences, Professeur).

### B. Calcul de Paie Automatisé (Link with Attendance)
Pour les vacataires, le système calcule la paie directement à partir du module **Présences** et **Scheduling**.
*   `Heures dues` = Somme des cours programmés ET validés par le scan de présence.
*   Élimine les "heures fictives" et garantit un paiement juste.

### C. Self-Service Employé
Chaque membre du personnel dispose de son portail pour :
*   Télécharger ses bulletins de paie.
*   Soumettre ses demandes de congé/absence.
*   Mettre à jour ses coordonnées bancaires et son CV.

## 3. Intégration Comptable
Les écritures de paie sont générées automatiquement et exportables vers le module Finance pour paiement (Virement/Chèque).
