# Spécification Système de Facturation SaaS (Billing)

## 1. Le Problème
Pour durer, le projet doit s'autofinancer et être géré comme un service professionnel.
*   **Contrôle des Coûts** : Sans limite de quota, une école pourrait saturer les serveurs avec des téraoctets de données, rendant le business non viable.
*   **Gestion des Revenus** : Le suivi manuel des abonnements de 50 établissements est impossible.
*   **Rigueur Commerciale** : Nécessité de couper l'accès en cas d'impayé prolongé pour protéger la propriété intellectuelle et les ressources serveurs.

## 2. La Solution : Moteur d'Abonnement et Quotas Dynamiques

### A. Gestion Energique des Quotas
Le système impose des limites matérielles (Hard Limits) basées sur le contrat :
*   **Nombre d'étudiants actifs** : Bloque les nouvelles inscriptions si le seuil est atteint.
*   **Stockage Documentaire** : Alerte à 80% de remplissage, blocage de l'upload à 100%.

### B. Niveaux de Service (SLA)
1.  **Starter** : Pour les tests, limité et sans support garanti.
2.  **Growth** : Pour les écoles moyennes, support par ticket, backups quotidiens.
3.  **Enterprise** : Pour les universités, support 24/7 par téléphone, isolation de base de données dédiée, backups temps-réel.

### C. Automatisation du Recouvrement (Lifecycle)
Le système gère le cycle de vie du client sans intervention humaine :
*   Génération automatique de la facture SaaS chaque mois/an.
*   **Mode "Lecture Seule"** : En cas d'impayé à J+15, l'université peut encore consulter ses données mais ne peut plus rien modifier (Garantit la continuité du service mais incite au paiement).

## 3. Super-Admin (Le God Mode)
Interface réservée aux ingénieurs de Skooly pour superviser la santé globale de tous les Tenants, monitorer l'usage des ressources et provisionner de nouvelles écoles en quelques secondes.
