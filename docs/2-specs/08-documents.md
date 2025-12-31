# Spécification Module Documents & Diplômes Sécurisés

## 1. Le Problème
La falsification de documents académiques (diplômes, relevés de notes) est un risque majeur pour la réputation d'une institution.
*   **Vérification lente** : Les entreprises doivent appeler l'université pour vérifier l'authenticité d'un diplôme.
*   **Perte/Détérioration** : Les étudiants perdent leurs originaux papiers et le processus de duplicata est long.
*   **Coût d'impression** : Impression sécurisée coûteuse (papier filigrané, tampons).

## 2. La Solution : Certification Digitale et QR Codes de Vérification

### A. Signature Numérique des Documents
Chaque document généré par Skooly (Attestation, Relevé, Diplôme) est signé cryptographiquement.
*   **Hash Unique** : Un identifiant unique est généré à partir du contenu du document.
*   **QR Code Public** : Un QR code est imprimé en bas du document. En le scannant, un tiers (employeur, ambassade) accède à une page de vérification officielle hébergée par l'université confirmant l'authenticité de la copie.

### B. Portefeuille de Documents Étudiant
L'étudiant dispose d'un espace sécurisé dans son portail.
*   **Consultation 24/7** : Accès à ses documents dès qu'ils sont générés et validés par l'administration.
*   **Partage Sécurisé** : Envoi direct du document certifié à un tiers via un lien temporaire.

### C. Archivage Légal
Le système assure la conservation à long terme (indéfinie) des documents originaux, garantissant que même 20 ans plus tard, l'université peut réémettre une attestation certifiée conforme.

## 3. Sécurité
*   Le module de génération utilise des templates verrouillés.
*   L'accès à la modification des données sources (les notes) avant génération est logué de manière paranoïaque (Audit Trail).
