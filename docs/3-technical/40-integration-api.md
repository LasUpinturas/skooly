# 🔌 API d'Intégration & Webhooks : L'Ouverture

> Skooly ne doit pas être une prison de données.
> Il doit parler à Sage Compta, aux Tourniquets de la cantine, et au Moodle.

---

## 1. Developer API (REST)

Une API publique documentée (Swagger) pour les partenaires.
`https://api.skooly.io/v1`

### Authentification (API Keys)
Chaque intégration a sa clé.
*   `X-API-KEY`: `sk_live_...`
*   Scope restreint : Une clé pour "Cantine" n'a accès qu'à `GET /students/badge/{id}`.

### Endpoints Critiques
*   `GET /students/{matricule}` : Vérifier si un étudiant est inscrit (pour la Bibliothèque externe).
*   `POST /absences` : Pousser une absence depuis un système tiers.
*   `GET /financial/status/{matricule}` : Vérifier si l'étudiant est à jour (vert/rouge).

---

## 2. Webhooks (Push Notification)

Skooly notifie les systèmes externes en temps réel.

### Les Events (Topics)
*   `student.created` : Nouvel inscrit -> Créer son compte Active Directory.
*   `invoice.paid` : Paiement reçu -> Débloquer accès.
*   `student.graduated` : Diplômé -> Basculer en Alumni.

### Sécurité des Webhooks
1.  **Signature HMAC** : On signe chaque payload avec un secret partagé (`sha256`).
    *   Le destinataire vérifie que ça vient bien de Skooly.
2.  **Retry Policy** : Si le serveur en face répond `500` ou timeout, on réessaie (backoff exponentiel : 1min, 5min, 1h).

---

## 3. Cas d'Usage : Le Tourniquet de la Cantine

**Scénario** : L'étudiant scanne son badge à l'entrée du Resto U.

1.  **Tourniquet (IoT)** : Lit le NFC `12345`.
2.  **Tourniquet -> Skooly API** : `GET /v1/access-control/check?nfc=12345&zone=RESTAURANT`.
3.  **Skooly Logic** :
    *   Étudiant existe ? Oui.
    *   A payé sa scolarité ? Oui.
    *   A du solde resto ? Non.
4.  **Skooly API** : Répond `403 Forbidden` + Message "Solde insuffisant".
5.  **Tourniquet** : Bip Rouge 🔴.

---

## 4. Cas d'Usage : Sync Comptable (Sage Saari)

**Scénario** : Le comptable veut ses écritures dans Sage.

1.  **Sage Connector (Job de Nuit)** : Appelle `GET /v1/finance/journals?date=2024-12-31`.
2.  **Skooly** : Renvoie le JSON des écritures du jour.
3.  **Sage Connector** : Transforme en format propriétaire Sage et injecte.
