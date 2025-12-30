# 📡 Module Communication : Le Megaphone Intelligent

## On ne refait pas Gmail
Grosse erreur : essayer de build une inbox.
Skooly est un **Routeur de Messages**.

## Entités Principales

### 1. `NotificationTemplate`
On ne hardcode pas les messages.
*   `code`: `TUITION_REMINDER`
*   `channel`: `SMS` | `EMAIL` | `WHATSAPP`
*   `content`: "Hello {{student_name}}, n'oublie pas tes {{amount}} FCFA."

### 2. `MessageLog` (Preuve Légale)
Si un étudiant dit "Je ne savais pas", on sort les logs.
*   `status`: `SENT` -> `DELIVERED` -> `READ` (via Pixel tracking).
*   `provider_id`: ID technique chez Twilio/Infobip.

## Canaux Supportés

### 1. WhatsApp Business API (Le Roi en Afrique)
C'est là que les étudiants sont.
*   Envoi de bulletins PDF direct sur WhatsApp.
*   Chatbot basique : "Mon emploi du temps ?" -> Réponse auto image.

### 2. SMS (Pour l'urgence critique)
*   "Cours annulé ce matin".
*   "Alerte sécurité".
*   Coût par message, donc on l'utilise peu.

### 3. Email (Pour l'officiel)
*   Reçus de paiement, convocations.
*   Intégration SendGrid / Amazon SES.
