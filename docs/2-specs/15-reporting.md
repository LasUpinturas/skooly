# Spécification Module Reporting & Décisionnel

## 1. Le Problème
Les décideurs (Recteur, Doyen, DAF) naviguent souvent à vue.
*   **Données Opaques** : Il faut des jours pour obtenir un chiffre fiable sur le taux de réussite global ou le montant exact encaissé par le Mobile Money.
*   **Réactivité Nulle** : On découvre les problèmes (décrochage d'une filière, chute des revenus) à la fin de l'année, trop tard pour agir.

## 2. La Solution : Dashboards Temps-Réel et Rapports Analytiques

### A. Tableaux de Bord par Rôle
Chaque décideur a sa propre vue critique :
*   **Recteur** : Effectifs totaux, Taux d'occupation des salles, Global Payment Status.
*   **DAF** : Courbe des rentrées d'argent, Budgets consommés, Alertes impayés majeurs.
*   **Doyen** : Statistiques de notes par département, Taux d'absentéisme profs/élèves.

### B. Moteur d'Export Paramétrable
Au-delà des écrans, le système permet de générer des rapports complexes :
*   Formats supportés : PDF pro (prêt pour signature), Excel (pour analyse externe), CSV.
*   **Envois Programmés** : Recevoir chaque lundi matin par email le "Rapport de Santé" du département.

### C. Qualité de la Donnée
Le système identifie les anomalies de données (ex: Étudiant avec 25/20 de moyenne, ou Paiement sans facture) pour correction immédiate, garantissant la fiabilité des chiffres.

## 3. Spécificités Techniques
Utilisation de vues SQL optimisées pour ne pas ralentir l'application lors de calculs sur des millions de lignes de données.
