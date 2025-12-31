# 🚑 Disaster Recovery Plan (DRP) : Survivre au Pire

> **Scénario du Pire** : Le Data Center d'Orange Cameroun brûle.
> Ou un admin efface la table `Users` en prod.
> Ou un Ransomware chiffre tout.

---

## 1. Stratégie de Backup (La Ceinture de Sécurité)

### A. Point-in-Time Recovery (PITR)
On utilise **PostgreSQL WAL (Write Ahead Logs)** archivés en continu.
*   **RPO (Recovery Point Objective)** : 5 minutes. (On accepte de perdre max 5 min de données).
*   **RTO (Recovery Time Objective)** : 1 heure. (Le temps de remonter l'infra).

### B. Le Dump Quotidien (Cold Storage)
Chaque nuit à 03h00 :
1.  `pg_dump` complet de la base.
2.  Chiffrement GPG (Clé publique Admin).
3.  Upload vers **AWS S3 (Paris)** ET **Orange Cloud (Douala)** (Redondance Géographique).
4.  Rétention : 30 jours glissants + 1 archivage mensuel (gardé 10 ans).

---

## 2. High Availability (HA) - Éviter la panne

Pour ne pas tomber si un serveur lâche.

*   **Database** : Cluster Postgres Primary + Standby Replica (Streaming Replication).
    *   Si Primary meurt, le Standby devient Primary auto (Failover).
*   **API** : Stateless. Scalée horizontalement (3 instances Docker derrière un Load Balancer Nginx).
    *   Si un conteneur crash, les 2 autres encaissent.

---

## 3. Procédure de Crise (Le "Red Button")

Si tout explose.

1.  **Déclaration d'Incident** : SMS à l'équipe Core.
2.  **Activation "Maintenance Mode"** : Page statique "Skooly est en maintenance".
3.  **Restauration** :
    *   Script Ansible : Provisionne un nouveau VPS vierge.
    *   Pull Docker Images.
    *   Restauration du dernier Dump S3.
    *   Rejeu des WAL logs (pour récupérer les 5 dernières minutes).
4.  **Vérification** : Smoke Test (Login ? Données là ?).
5.  **Réouverture**.

---

## 4. Protection des Données (Security First)

*   **Encryption at Rest** : Le disque dur du serveur est chiffré (LUKS). Si on vole le serveur physique, les données sont illisibles.
*   **Encryption in Transit** : TLS 1.3 forcé partout.
*   **WAF (Web App Firewall)** : Bloque les injections SQL et DDOS basiques (Cloudflare).

---

## 5. Le Cas "Internet Coupé au Cameroun" (Mode Dégradé)

Si le pays est coupé du monde (Câble sous-marin) mais que l'Intranet local marche.
*   Skooly doit pouvoir être déployé en **Local Mode** (Sur un serveur dans le campus) et se sync plus tard.
*   *(Note: C'est une feature Enterprise très complexe, hors scope V1, mais prévue dans l'architecture).*
