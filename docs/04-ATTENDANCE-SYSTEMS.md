# 📍 Systèmes de Présences - Documentation Complète

> **Module critique pour le suivi de l'assiduité étudiants et enseignants**

---

## 📚 Table des Matières

1. [Présences Étudiants](#1-présences-étudiants)
2. [Méthodes de Pointage Étudiants](#2-méthodes-de-pointage-étudiants)
3. [Présences Enseignants](#3-présences-enseignants)
4. [Architecture Technique](#4-architecture-technique)
5. [Implémentation Recommandée](#5-implémentation-recommandée)

---

## 1. Présences Étudiants

### 1.1 Contexte & Enjeux

Au Cameroun et dans les IUT, la **présence** est **obligatoire** et souvent liée à :
- Validation des UE (règle: > X% absences = exclusion examen)
- Bourses (minimum présence requis)
- Discipline académique

**Problèmes actuels:**
- ❌ Listes d'émargement papier (fraude facile)
- ❌ Signature pour les absents ("copains")
- ❌ Saisie manuelle tardive
- ❌ Pas de statistiques en temps réel

**Solution digitale:**
- ✅ Pointage instantané
- ✅ Anti-fraude (géolocalisation, limite temporelle)
- ✅ Statistiques temps réel
- ✅ Alertes automatiques

---

## 2. Méthodes de Pointage Étudiants

### 🎯 Matrice de Comparaison

| Méthode | Coût | Fiabilité | Facilité | Anti-fraude | Recommandation |
|---------|------|-----------|----------|-------------|----------------|
| **QR Code Dynamique** | Gratuit | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ✅ **Top choix** |
| Code Numérique | Gratuit | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | Alternative simple |
| NFC/RFID | €€€ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Si budget |
| Reconnaissance Faciale | €€ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Futur |
| Liste manuelle | Gratuit | ⭐ | ⭐⭐ | ⭐ | Legacy fallback |

---

### 2.1 Méthode 1: QR Code Dynamique ⭐ RECOMMANDÉ

#### Principe

```
┌──────────────────────────────────────────────────────────┐
│                    WORKFLOW                               │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  1. Enseignant démarre le cours                         │
│     └─> Système génère QR code unique                   │
│                                                           │
│  2. QR affiché sur vidéoprojecteur/écran                │
│     └─> Valide pendant 5-10 minutes                     │
│                                                           │
│  3. Étudiants scannent avec app mobile                  │
│     ├─> Vérification géolocalisation (rayon campus)     │
│     ├─> Vérification timing (fenêtre valide)            │
│     └─> Vérification unique (1 scan/étudiant)           │
│                                                           │
│  4. Pointage enregistré instantanément                   │
│     └─> Notification à l'étudiant                       │
│                                                           │
│  5. QR rotation toutes les X minutes (anti-screenshot)   │
│                                                           │
└──────────────────────────────────────────────────────────┘
```

#### Avantages

✅ **Coût zéro** (juste smartphone étudiant)
✅ **Facile à mettre en œuvre**
✅ **Anti-fraude robuste** (géoloc + timing + rotation)
✅ **Pas d'infrastructure** (pas de lecteurs)
✅ **Instantané**

#### Architecture Technique

```typescript
// 1. Génération du QR Code (Backend)

interface AttendanceSession {
  id: string;                    // UUID
  courseId: string;
  teacherId: string;
  date: Date;
  startTime: Date;
  endTime: Date;
  qrToken: string;               // JWT signé
  qrValidUntil: Date;            // Expiration
  location: {
    latitude: number;
    longitude: number;
    radius: number;              // En mètres (ex: 500m)
  };
}

// Service de génération
class AttendanceService {
  async startSession(courseId: string, teacherId: string) {
    // 1. Créer session
    const session = await this.createSession(courseId, teacherId);
    
    // 2. Générer JWT avec payload spécial
    const qrToken = jwt.sign(
      {
        sessionId: session.id,
        courseId: courseId,
        exp: Math.floor(Date.now() / 1000) + (10 * 60), // 10 min
        iat: Math.floor(Date.now() / 1000),
        rotationId: generateRandomString(8) // Anti-screenshot
      },
      process.env.QR_SECRET,
      { algorithm: 'HS256' }
    );
    
    // 3. Mettre en cache Redis (vérification rapide)
    await redis.setex(
      `qr:${session.id}`,
      600, // 10 minutes
      JSON.stringify({ qrToken, location: session.location })
    );
    
    return { session, qrToken };
  }
  
  // Rotation automatique toutes les 2 minutes
  async rotateQR(sessionId: string) {
    const newToken = jwt.sign(
      {
        sessionId,
        exp: Math.floor(Date.now() / 1000) + (2 * 60),
        rotationId: generateRandomString(8)
      },
      process.env.QR_SECRET
    );
    
    await redis.setex(`qr:${sessionId}`, 120, newToken);
    
    // WebSocket pour push vers app enseignant
    this.websocket.emit(`session:${sessionId}`, { 
      event: 'qr_rotated',
      qrToken: newToken 
    });
    
    return newToken;
  }
}
```

```typescript
// 2. Scan QR Code (Mobile App)

class AttendanceScanner {
  async scanQR(qrToken: string) {
    try {
      // 1. Obtenir géolocalisation étudiant
      const position = await this.getCurrentPosition();
      
      // 2. Envoyer au backend
      const response = await api.post('/attendance/check-in', {
        qrToken,
        location: {
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
          accuracy: position.coords.accuracy
        }
      });
      
      if (response.success) {
        this.showSuccess('Présence enregistrée ✅');
      }
    } catch (error) {
      this.showError(error.message);
    }
  }
  
  private async getCurrentPosition(): Promise<GeolocationPosition> {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        resolve,
        reject,
        { 
          enableHighAccuracy: true, 
          timeout: 10000,
          maximumAge: 0 
        }
      );
    });
  }
}
```

```typescript
// 3. Vérification & Enregistrement (Backend)

class AttendanceCheckIn {
  async checkIn(studentId: string, qrToken: string, location: Location) {
    // 1. Vérifier JWT
    let payload;
    try {
      payload = jwt.verify(qrToken, process.env.QR_SECRET);
    } catch (error) {
      throw new Error('QR Code expiré ou invalide');
    }
    
    // 2. Récupérer session
    const session = await this.getSession(payload.sessionId);
    if (!session) {
      throw new Error('Session non trouvée');
    }
    
    // 3. Vérifier géolocalisation
    const distance = this.calculateDistance(
      location,
      session.location
    );
    
    if (distance > session.location.radius) {
      throw new Error('Vous êtes trop loin du campus');
    }
    
    // 4. Vérifier timing
    const now = new Date();
    if (now > session.endTime) {
      throw new Error('Session de pointage terminée');
    }
    
    // 5. Vérifier pas déjà pointé
    const existing = await this.prisma.attendance.findFirst({
      where: {
        sessionId: session.id,
        studentId: studentId
      }
    });
    
    if (existing) {
      throw new Error('Vous avez déjà pointé pour ce cours');
    }
    
    // 6. Vérifier étudiant inscrit à ce cours
    const enrollment = await this.isStudentEnrolled(
      studentId,
      session.courseId
    );
    
    if (!enrollment) {
      throw new Error('Vous n\'êtes pas inscrit à ce cours');
    }
    
    // ✅ 7. Enregistrer présence
    const attendance = await this.prisma.attendance.create({
      data: {
        sessionId: session.id,
        studentId: studentId,
        status: 'PRESENT',
        checkedInAt: now,
        location: location,
        method: 'QR_CODE'
      }
    });
    
    // 8. Notifier
    await this.notificationService.send(studentId, {
      title: 'Présence enregistrée',
      body: `${session.courseName} - ${formatDate(now)}`
    });
    
    return { success: true, attendance };
  }
  
  private calculateDistance(loc1: Location, loc2: Location): number {
    // Formule Haversine
    const R = 6371e3; // Rayon terre en mètres
    const φ1 = loc1.latitude * Math.PI/180;
    const φ2 = loc2.latitude * Math.PI/180;
    const Δφ = (loc2.latitude - loc1.latitude) * Math.PI/180;
    const Δλ = (loc2.longitude - loc1.longitude) * Math.PI/180;

    const a = Math.sin(Δφ/2) * Math.sin(Δφ/2) +
              Math.cos(φ1) * Math.cos(φ2) *
              Math.sin(Δλ/2) * Math.sin(Δλ/2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));

    return R * c; // Distance en mètres
  }
}
```

#### UI/UX Enseignant (Next.js)

```typescript
'use client';

import { QRCodeSVG } from 'qrcode.react';
import { useEffect, useState } from 'react';

export function AttendanceQRDisplay({ sessionId }: { sessionId: string }) {
  const [qrToken, setQrToken] = useState<string>('');
  const [stats, setStats] = useState({ present: 0, total: 0 });
  
  useEffect(() => {
    // WebSocket pour rotations automatiques
    const ws = new WebSocket(`wss://api.erp.iut.cm/ws`);
    
    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      
      if (data.event === 'qr_rotated') {
        setQrToken(data.qrToken);
      }
      
      if (data.event === 'student_checked_in') {
        setStats(prev => ({ ...prev, present: prev.present + 1 }));
      }
    };
    
    return () => ws.close();
  }, [sessionId]);
  
  return (
    <div className="flex flex-col items-center p-8">
      <h2 className="text-2xl font-bold mb-4">
        Scannez pour pointer votre présence
      </h2>
      
      <div className="bg-white p-8 rounded-lg shadow-xl">
        <QRCodeSVG 
          value={qrToken} 
          size={300}
          level="H"
          includeMargin
        />
      </div>
      
      <div className="mt-6 text-center">
        <p className="text-4xl font-bold text-green-600">
          {stats.present} / {stats.total}
        </p>
        <p className="text-gray-600">étudiants présents</p>
      </div>
      
      <div className="mt-4 flex items-center gap-2">
        <div className="w-3 h-3 bg-green-500 rounded-full animate-pulse" />
        <span className="text-sm text-gray-600">QR actif</span>
      </div>
    </div>
  );
}
```

---

### 2.2 Méthode 2: Code Numérique à 6 Chiffres

#### Principe

Alternative plus simple si problème avec camera/QR:

```
1. Enseignant génère code 6 chiffres aléatoire
   └─> Affiché au tableau

2. Valide pendant 10 minutes

3. Étudiants saisissent code sur app/portail

4. Même vérifications (géoloc, timing, unique)
```

#### Implémentation

```typescript
// Génération
function generateAttendanceCode(): string {
  return Math.floor(100000 + Math.random() * 900000).toString();
}

// Validation
async function validateCode(code: string, studentId: string) {
  const session = await redis.get(`code:${code}`);
  
  if (!session) {
    throw new Error('Code invalide ou expiré');
  }
  
  // Même vérifications que QR
  // ...
}
```

**Avantages:**
- Plus simple (pas besoin QR scanner)
- Fonctionne même sur vieux téléphones

**Inconvénients:**
- Risque partage du code (WhatsApp)
- Nécessite géolocalisation stricte

---

### 2.3 Méthode 3: NFC/RFID

#### Principe

```
1. Carte étudiant équipée puce NFC/RFID

2. Lecteur à l'entrée de chaque salle

3. Étudiant badge à l'entrée

4. Enregistrement automatique
```

#### Matériel Requis

```
Carte NFC NTAG216:
- Coût: ~0.50€ pièce × 3000 étudiants = 1 500€

Lecteurs NFC (USB):
- ACR122U: ~30€
- Besoin: 1 par salle (ex: 30 salles) = 900€

Total initial: ~2 400€
```

#### Implémentation Logique

```typescript
// Raspberry Pi ou PC avec lecteur NFC

import { NFC } from 'nfc-pcsc';

const nfc = new NFC();

nfc.on('reader', reader => {
  reader.on('card', async card => {
    // Lire UID de la carte
    const studentId = card.uid;
    
    // Envoyer au backend
    await api.post('/attendance/nfc-check-in', {
      studentId,
      readerId: reader.name,
      timestamp: new Date()
    });
  });
});
```

**Avantages:**
- ✅ Très rapide (< 1 sec)
- ✅ Pas besoin smartphone
- ✅ Difficilement falsifiable

**Inconvénients:**
- ❌ Coût matériel
- ❌ Maintenance lecteurs
- ❌ Perte/vol carte

---

### 2.4 Méthode 4: Reconnaissance Faciale

#### Principe

```
1. Photo prise lors de l'inscription

2. Caméra à l'entrée de la salle

3. Détection visages en temps réel

4. Matching avec base de données

5. Enregistrement automatique
```

#### Stack Technique

```typescript
// Backend: Python + Face Recognition

import face_recognition
import cv2

class FaceAttendance:
    def __init__(self):
        self.known_faces = self.load_known_faces()
    
    def load_known_faces(self):
        # Charger toutes les photos étudiants
        students = db.query("SELECT id, photo_path FROM students")
        
        encodings = {}
        for student in students:
            image = face_recognition.load_image_file(student.photo_path)
            encoding = face_recognition.face_encodings(image)[0]
            encodings[student.id] = encoding
        
        return encodings
    
    def identify_students(self, frame):
        # Trouver visages dans la frame
        face_locations = face_recognition.face_locations(frame)
        face_encodings = face_recognition.face_encodings(
            frame, 
            face_locations
        )
        
        identified = []
        
        for encoding in face_encodings:
            # Comparer avec base
            matches = face_recognition.compare_faces(
                list(self.known_faces.values()),
                encoding,
                tolerance=0.6
            )
            
            if True in matches:
                student_id = list(self.known_faces.keys())[
                    matches.index(True)
                ]
                identified.append(student_id)
        
        return identified
```

**Avantages:**
- Complètement automatique
- Pas d'interaction requise
- Détection multi-personnes

**Inconvénients:**
- Complexe techniquement
- Problèmes de vie privée
- Coût caméras
- Performance (éclairage, angle)

---

### 2.5 Comparaison Coûts

| Méthode | Coût Initial | Coût Récurrent | Maintenance |
|---------|--------------|----------------|-------------|
| QR Code | 0€ | 0€ | Faible |
| Code 6 chiffres | 0€ | 0€ | Faible |
| NFC/RFID | 2 400€ | Remplacement cartes | Moyenne |
| Reconnaissance Faciale | 5 000€+ | 0€ | Haute |

---

## 3. Présences Enseignants

### 3.1 Objectifs

- Suivi des heures effectuées vs heures prévues
- Base pour calcul indemnités
- Détection cours non assurés
- Statistiques ponctualité

### 3.2 Méthodes Recommandées

#### Méthode A: Ouverture/Fermeture de Cours

```
Workflow:
1. Enseignant ouvre le cours sur app
   ├─> Heure début enregistrée
   └─> Position GPS enregistrée

2. Système vérifie:
   ├─> Cours dans emploi du temps
   └─> Position cohérente (campus)

3. Enseignant ferme le cours
   └─> Heure fin enregistrée

4. Calcul durée effective
```

**Implémentation:**

```typescript
interface CourseSession {
  id: string;
  courseId: string;
  teacherId: string;
  scheduledStart: Date;
  scheduledEnd: Date;
  actualStart?: Date;
  actualEnd?: Date;
  status: 'SCHEDULED' | 'ONGOING' | 'COMPLETED' | 'CANCELLED';
  duration?: number; // minutes
}

class TeacherAttendanceService {
  async startCourse(teacherId: string, courseId: string) {
    const now = new Date();
    
    // Vérifier cours programmé
    const scheduled = await this.getScheduledCourse(
      teacherId,
      courseId,
      now
    );
    
    if (!scheduled) {
      throw new Error('Aucun cours programmé à cette heure');
    }
    
    // Enregistrer début
    const session = await this.prisma.courseSession.update({
      where: { id: scheduled.id },
      data: {
        actualStart: now,
        status: 'ONGOING'
      }
    });
    
    // Activer pointage étudiants
    await this.attendanceService.startSession(session.id, teacherId);
    
    return session;
  }
  
  async endCourse(sessionId: string) {
    const now = new Date();
    
    const session = await this.prisma.courseSession.update({
      where: { id: sessionId },
      data: {
        actualEnd: now,
        status: 'COMPLETED',
        duration: this.calculateDuration(session.actualStart, now)
      }
    });
    
    // Désactiver pointage étudiants
    await this.attendanceService.endSession(sessionId);
    
    return session;
  }
  
  private calculateDuration(start: Date, end: Date): number {
    return Math.floor((end.getTime() - start.getTime()) / 60000);
  }
}
```

#### Méthode B: Pointage Salle (QR/NFC)

Similaire aux étudiants mais avec QR fixe par salle:

```
1. QR code permanent dans chaque salle

2. Enseignant scanne à l'arrivée et au départ

3. Système associe avec emploi du temps
```

---

### 3.3 Reporting Enseignants

```typescript
// Fiche de service mensuelle

interface ServiceSheet {
  teacherId: string;
  month: string;
  courses: {
    name: string;
    scheduled: number;  // heures
    performed: number;  // heures
    difference: number;
  }[];
  totalScheduled: number;
  totalPerformed: number;
  overtimeHours: number;
}

async function generateServiceSheet(
  teacherId: string,
  month: string
): Promise<ServiceSheet> {
  const sessions = await prisma.courseSession.findMany({
    where: {
      teacherId,
      date: {
        gte: startOfMonth(month),
        lte: endOfMonth(month)
      }
    },
    include: { course: true }
  });
  
  // Agrégation par cours
  const courseStats = sessions.reduce((acc, session) => {
    const key = session.courseId;
    if (!acc[key]) {
      acc[key] = {
        name: session.course.name,
        scheduled: 0,
        performed: 0
      };
    }
    
    acc[key].scheduled += session.duration || 0;
    acc[key].performed += session.actualDuration || 0;
    
    return acc;
  }, {});
  
  // ...
}
```

---

## 4. Architecture Technique

### 4.1 Modèle de Données Prisma

```prisma
// schema.prisma

model AttendanceSession {
  id            String   @id @default(uuid())
  courseId      String
  course        Course   @relation(fields: [courseId], references: [id])
  teacherId     String
  teacher       Teacher  @relation(fields: [teacherId], references: [id])
  
  date          DateTime
  startTime     DateTime
  endTime       DateTime
  
  qrToken       String?
  qrValidUntil  DateTime?
  
  location      Json?    // { lat, lng, radius }
  
  method        AttendanceMethod
  status        SessionStatus @default(ACTIVE)
  
  attendances   Attendance[]
  
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  
  @@index([courseId, date])
  @@index([teacherId, date])
}

model Attendance {
  id          String   @id @default(uuid())
  sessionId   String
  session     AttendanceSession @relation(fields: [sessionId], references: [id])
  studentId   String
  student     Student  @relation(fields: [studentId], references: [id])
  
  status      AttendanceStatus
  checkedInAt DateTime?
  location    Json?
  method      AttendanceMethod
  
  justification String?
  justificationDoc String? // URL fichier
  justifiedAt   DateTime?
  justifiedBy   String?
  
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  
  @@unique([sessionId, studentId])
  @@index([studentId, status])
}

enum AttendanceStatus {
  PRESENT
  ABSENT
  LATE
  EXCUSED
  EXCLUDED
}

enum AttendanceMethod {
  QR_CODE
  NFC
  NUMERIC_CODE
  FACIAL_RECOGNITION
  MANUAL
}

enum SessionStatus {
  SCHEDULED
  ACTIVE
  CLOSED
  CANCELLED
}
```

### 4.2 APIs REST

```typescript
// Routes principales

POST   /api/attendance/sessions                 // Créer session
GET    /api/attendance/sessions/:id            // Détails session
PUT    /api/attendance/sessions/:id/close      // Fermer session

POST   /api/attendance/check-in                // Pointage étudiant
GET    /api/attendance/my-attendance           // Mes présences

GET    /api/attendance/stats/student/:id       // Stats étudiant
GET    /api/attendance/stats/course/:id        // Stats cours

POST   /api/attendance/justifications          // Submit justification
PUT    /api/attendance/justifications/:id      // Valider justification
```

---

## 5. Implémentation Recommandée

### Phase 1: MVP (QR Code)
```
✅ QR code dynamique avec géolocalisation
✅ App mobile basique (scan QR)
✅ Dashboard enseignant (génération QR, stats temps réel)
✅ Portail étudiant (historique présences)
```

### Phase 2: Amélioration
```
✅ Code numérique comme fallback
✅ Justifications en ligne
✅ Alertes automatiques (seuils dépassés)
✅ Reporting avancé
```

### Phase 3: Avancé
```
✅ NFC/RFID si budget
✅ Reconnaissance faciale (expérimental)
✅ Analytics prédictifs
✅ Intégration avec notes (corrélation)
```

---

**Prochaines étapes:** Voir [Finance](./06-FINANCE.md) ou [Emploi du Temps](./05-SCHEDULING.md)
