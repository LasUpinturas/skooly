# Conformité & Souveraineté Numérique

## 1. Le Problème
Le Cameroun (et la zone CEMAC) dispose d'un cadre légal strict concernant la cybersécurité et les données à caractère personnel (Loi de 2010).
Stocker des notes, des données médicales ou financières d'étudiants camerounais sur des serveurs inconnus expose l'université à des sanctions pénales et à une perte de souveraineté.

## 2. La Solution : "Privacy-By-Design" & Localisation

Skooly intègre la conformité au cœur de son architecture, et non comme une option.

### A. Hébergement et Résidence des Données
Nous offrons deux modèles de déploiement pour garantir la souveraineté :
1.  **Mode On-Premise (Souverain)** : Skooly est déployé directement sur les serveurs de l'université ou dans un Data Center national (Camtel/Orange Cameroun). Les données ne quittent jamais le territoire.
2.  **Mode Cloud (Conforme)** : Hébergement sur des zones conformes RGPD (ex: France), avec des garanties contractuelles de non-transfert hors juridiction autorisée.

### B. Droit des Personnes (Rétention et Oubli)
Le système gère nativement les cycles de vie légaux :
*   **Droit de Rectification** : Tout étudiant peut demander formellement la correction d'une donnée erronée.
*   **Archivage Légal** : Les preuves de diplômes et PV de notes sont conservés indéfiniment (caractère d'archive publique).
*   **Purge Automatique** : Les données non essentielles (logs de connexion, brouillons) sont purgées après 12 mois glissants.

### C. Traçabilité et Audit (Loi de 2010)
Toute action modification de donnée critique (Note, Paiement) est enregistrée dans un journal d'audit inaltérable.
**Chaque entrée contient** :
*   L'auteur (User ID).
*   L'horodatage précis (NTP synchronisé).
*   L'adresse IP source.
*   L'ancienne et la nouvelle valeur.
*   Le motif de la modification (Champ obligatoire).

Cela permet de répondre aux réquisitions judiciaires en cas de fraude ou de litige.
