# 📊 Module Reporting : La Tour de Contrôle

## Pourquoi c'est Vital ?
Un Recteur ne regarde pas les fiches étudiantes individuelles. Il regarde les agrégats.
Skooly ne doit pas être une boîte noire.

## 1. Dashboards Temps Réel (Les "KPIS")

### Dashboard Recteur / DG
*   **Vue Hélicoptère** :
    *   Taux de recouvrement Finance d'aujourd'hui (ex: "5M FCFA encaissés ce matin").
    *   Taux de présence Moyen (ex: "85% des étudiants sont là").
    *   Alertes Critiques (ex: "3 Profs absents non justifiés").

### Dashboard Chef de Département
*   **Vue Opérationnelle** :
    *   "Quels cours ont lieu maintenant ?" (Liste des salles actives).
    *   "Qui n'a pas encore soumis ses notes ?" (Wall of Shame des profs).

## 2. Le Moteur d'Exports (The Export Engine)

Skooly ne garde pas les données en otage. Tout est exportable.

### Exports Académiques
*   **PV de Délibération** (PDF/Excel) : Le document légal signé par le jury.
*   **Relevés de Notes en Masse** : Générer 500 PDF en un clic (ZIP).
*   **Statistiques LMD** : "Combien on validé l'UE INF304 ?" (Bar chart).

### Exports Présences (La Demande Précise)
*   **Rapport Hebdomadaire** : "Liste des étudiants absents > 10h cette semaine".
*   **Rapport Mensuel (Paie)** : "Liste des heures faites par M. le Prof X" -> Export vers Sage Paie.
*   **Rapport Journalier** : "Le point du jour" (Envoyé par email au DG à 18h00).

## 3. Architecture Technique des Rapports

On ne fait pas de `SELECT *` sur la base de prod en journée.

1.  **Read Replica** : Les rapports lourds tapent sur une réplication de la DB.
2.  **Job Queue (BullMQ)** : "Générer les 2000 bulletins" est une tâche de fond.
    *   User clique "Export".
    *   UI dit "On vous envoie un mail quand c'est prêt".
    *   Worker génère le ZIP -> Upload S3 -> Envoi Lien.
