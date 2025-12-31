# Spécification Hiérarchie Multi-Campus (Holding)

## 1. Le Problème
Les grands groupes universitaires gèrent plusieurs écoles autonomes.
*   **Fragmentation** : L'IUT et l'ENSET utilisent des logiciels différents, rendant la consolidation des données impossible pour le Recteur.
*   **Coûts Doublés** : Payer plusieurs serveurs, plusieurs licences et plusieurs équipes de maintenance.
*   **Dossiers Incohérents** : Un étudiant qui change d'école au sein du même groupe doit tout recommencer (nouveau matricule, nouveaux documents).

## 2. La Solution : Architecture de Tenant Hiérarchique

### A. Relation Mère-Fille
Skooly permet de modéliser une "Université Mère" (Le Parent) ayant autorité sur plusieurs "Écoles Filles" (Les Children).
*   **Isolation des Opérations** : Chaque école a sa propre base opérationnelle, ses profs et ses règles de délibération.
*   **Partage des Référentiels** : L'Université peut imposer un format de matricule unique ou un catalogue de diplômes commun.

### B. Reporting Consolidé (Le Dashboard Groupe)
Accès réservé au top-management universaire pour voir les indicateurs clés agrégés de toutes les entités sans avoir à se connecter à chaque site individuellement.

### C. Mutualisation des Ressources
*   **Professeurs Itinérants** : Un prof peut avoir des charges horaires réparties sur plusieurs écoles avec un seul planning unifié.
*   **Gouvernance Centralisée** : L'administration centrale peut pousser des messages ou des règles de conformité à toutes les filiales en un clic.

## 3. Sécurité
L'architecture garantit par design qu'un administrateur de l'IUT ne peut jamais accéder aux données financières de l'ENSET, tout en permettant au Recteur de l'Université de voir les deux.
