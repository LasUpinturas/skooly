# ⚖️ Conformité & Souveraineté Numérique

## Le Contexte Camerounais (Loi de 2010 - Cybersécurité)
On ne fait pas n'importe quoi avec les données étudiantes au Cameroun.
Skooly est "Privacy-By-Design".

## 1. Hébergement des Données (Data Residency)

### Règle d'Or
Les données sensibles (Notes, Santé, Paiements) ne doivent idéalement pas quitter le territoire national (ou la zone CEMAC), ou être hébergées chez un provider conforme RGPD (qui couvre souvent les lois locales par extension).

### Options de Déploiement
*   **Mode Cloud (SaaS)** : Hébergé sur AWS Paris (eu-west-3) ou Azure South Africa. Conforme RGPD = OK pour le Cameroun (souvent toléré si sécurisé).
*   **Mode On-Premise (Souverain)** : Hébergé sur les serveurs de l'université ou chez Camtel/Orange DC à Douala. Skooly est livré via **Docker**.

## 2. Droit à l'Oubli & Rectification

### Workflow Légal
*   Un étudiant demande la suppression de ses données ?
    *   **Refus légal** : On ne peut pas supprimer un diplôme (c'est une archive d'état).
    *   **Anonymisation** : On supprime son email/téléphone marketing, mais on garde son matricule et ses notes (hashées).

## 3. Sécurité des Paiements

On ne stocke **JAMAIS** les numéros de carte bancaire (PCI-DSS).
Pour le Mobile Money :
*   On ne stocke que le `TransactionID` et le `PhoneNumber`.
*   Pas de code PIN stocké (évidemment).

## 4. Audit Log (La Boîte Noire)

La loi exige de pouvoir tracer "Qui a fait quoi".
Skooly enregistre **tout** :
*   `2024-12-30 14:00` : User `Admin` a changé la note de `Étudiant X` de 10 à 12.
*   `Motif` : "Erreur de saisie" (Champ obligatoire).
*   `IP` : `192.168.1.1`.

Sans ce log, le système est illégal dans une structure publique.
