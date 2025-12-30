# 📍 Module Présences : La Vérité Terrain

## Pourquoi les systèmes de présences échouent ?
Parce qu'ils sont trop rigides ou trop permissifs.
Un étudiant ne "signe" pas juste. Il participe à une **Session**.

## Entités Principales ("Models")

### 1. `AttendanceSession` (Odoo: `pos.session` like)
Un cours n'est pas juste un horaire. C'est une session ouverte par l'enseignant.

*   `status`: `OPEN` (Le QR code tourne), `CLOSED` (Fini), `VALIDATED` (Signé par le prof).
*   `teacher_location`: Lat/Long du prof au moment de l'ouverture.

### 2. `AttendanceRecord` (Odoo: `hr.attendance`)
La preuve atomique de présence.

*   `student_id`
*   `session_id`
*   `check_in_time`
*   `method`: `QR_SCAN`, `NFC`, `MANUAL_OVERRIDE`
*   `trust_score`: 0-100 (Calculé par l'IA anti-fraude).

## Workflow Anti-Fraude (Le "Secret Sauce")

Comment empêcher un étudiant d'envoyer le QR code par WhatsApp à son pote qui dort au quartier ?

1.  **QR Code Rotatif (TOTP)** : Le QR affiché au projecteur change toutes les 10 secondes. Une photo prise à T0 est invalide à T+11s.
2.  **Double Géolocalisation** :
    *   Le téléphone de l'étudiant envoie sa GPS coord.
    *   Le serveur compare avec la GPS coord du prof (ou de la salle).
    *   Distance > 50m ? 🚩 **FLAGGED**.
3.  **Fingerprinting** : Même Device ID pour 2 étudiants différents ? 🚩 **FRAUDE**.

## Implémentation Odoo-Style

*   **Pas de suppression** : Si un prof se trompe, il crée une "Correction" (contre-écriture).
*   **Validation par lot** : Le prof valide toute la séance à la fin. Cela "poste" les présences (état irréversible).

## Code Snippet (Logique de Scan)

```typescript
async scanQr(userId: string, qrPayload: string, geo: GeoPoint) {
  const session = await decodeQr(qrPayload); // Contient ID + Timestamp
  
  if (isExpired(session.timestamp)) throw new Error("QR périmé");
  if (distance(geo, session.teacherGeo) > 50) throw new Error("Trop loin !");
  
  return this.prisma.attendanceRecord.create({
    data: { userId, sessionId: session.id, method: 'QR_SCAN' }
  });
}
```
