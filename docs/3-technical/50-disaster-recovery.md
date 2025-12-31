# Stratégie de Reprise d'Activité (Disaster Recovery)

## 1. Analyse des Risques
L'indisponibilité de Skooly peut avoir des conséquences graves (arrêt des examens, impossibilité de paiement). Nous identifions trois niveaux de menace :
1.  **Panne mineure** : Crash d'un conteneur API.
2.  **Panne majeure** : Corruption de la base de données ou suppression accidentelle.
3.  **Catastrophe totale** : Destruction physique du data center.

## 2. Stratégie de Sauvegarde (Backup)

### A. Point-in-Time Recovery (PITR)
Utilisation des journaux de transaction (WAL) de PostgreSQL pour permettre une restauration à n'importe quelle seconde du passé (RPO < 5 minutes).

### B. Redondance Géographique
Les sauvegardes chiffrées sont exportées hors du data center de production :
*   Destination 1 : Stockage objet local (Cameroun).
*   Destination 2 : Cloud Azure/AWS (Europe) pour une isolation totale en cas de crise régionale.

---

## 3. Plan de Continuité (Protocole de Crise)

En cas d'incident, l'équipe technique suit une procédure standardisée :
1.  **Détection & Alerte** : Monitoring automatique (UptimeRobot/Sentry) déclenchant une alerte aux astreintes.
2.  **Isolation** : Mise en mode "Maintenance" pour protéger l'intégrité des données restantes.
3.  **Restauration** : Provisioning d'une nouvelle infrastructure via Infrastructure as Code (Terraform/Ansible) et réinjection du dernier backup sain.
4.  **Vérification (Smoke Test)** : Batterie de tests automatisés validant les fonctions critiques (Login, Finance) avant réouverture au public.

## 4. Règle d'Or : "Never Trust the Server"
Aucune donnée critique n'existe en un seul exemplaire. Le système est conçu pour être reconstruit de zéro en moins d'une heure à partir des seuls backups externes.
