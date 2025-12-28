# Gestion Financière - Documentation Complète

**Module critique pour la gestion des frais de scolarité et paiements au Cameroun**

---

## Table des Matières

1. [Contexte Financier Camerounais](#1-contexte-financier-camerounais)
2. [Structure des Frais](#2-structure-des-frais)
3. [Modes de Paiement](#3-modes-de-paiement)
4. [Intégration Mobile Money](#4-intégration-mobile-money)
5. [Gestion des Échéanciers](#5-gestion-des-échéanciers)
6. [Bourses & Réductions](#6-bourses--réductions)
7. [Architecture Technique](#7-architecture-technique)

---

## 1. Contexte Financier Camerounais

### 1.1 Réalités du Terrain

**Faits:**
- **< 10%** de la population a une carte bancaire
- **> 70%** utilisent Mobile Money (MTN MoMo dominant)
- Paiements en **espèces** encore très courants
- Files d'attente énormes aux guichets

**Problèmes actuels:**
- Réconciliation manuelle (erreurs, lenteur)
- Reçus papier (perte, falsification)
- Pas de traçabilité
- Difficile de suivre les impayés
- Rapports financiers laborieux

**Solutions digitales:**
- Paiement 24/7 via Mobile Money
- Reçus automatiques par SMS/Email
- Traçabilité complète
- Suivi temps réel des impayés
- Rapports automatiques

---

## 2. Structure des Frais

### 2.1 Composition Typique des Frais (IUT)

```
┌──────────────────────────────────────────────────────────┐
│           FRAIS DE SCOLARITÉ ANNUELS                     │
├───────────────────────────────┬──────────────────────────┤
│ COMPOSANTE                    │ MONTANT (FCFA)           │
├───────────────────────────────┼──────────────────────────┤
│ Droits universitaires (État)  │ 50 000                   │
│ Frais de formation            │ Variable (100k - 400k)   │
│ Assurance étudiant            │ 5 000                    │
│ Carte étudiant                │ 2 000                    │
│ Visite médicale               │ 5 000                    │
│ Frais d'examen                │ 10 000                   │
│ Frais de TP/Labo              │ 20 000                   │
│ Sport & activités             │ 5 000                    │
├───────────────────────────────┼──────────────────────────┤
│ TOTAL EXEMPLE (Licence 1)     │ 197 000 - 497 000        │
└───────────────────────────────┴──────────────────────────┘

FRAIS ADDITIONNELS:
├── Stage (Licence 3)          │ 15 000
├── Mémoire                     │ 20 000
├── Rattrapage (par UE)         │ 5 000
├── Duplicata carte             │ 2 000
├── Duplicata relevé            │ 1 000
└── Pénalités retard            │ 5-10% (optionnel)
```

### 2.2 Modèle de Données

```prisma
// schema.prisma

model FeeStructure {
  id            String   @id @default(uuid())
  
  academicYear  String   // "2024-2025"
  program       String   // "Licence", "Master"
  level         Int      // 1, 2, 3
  field         String?  // "Génie Informatique"
  
  components    FeeComponent[]
  
  totalAmount   Int      // En FCFA
  currency      String   @default("XAF")
  
  validFrom     DateTime
  validUntil    DateTime
  
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  
  @@index([academicYear, program, level])
}

model FeeComponent {
  id              String   @id @default(uuid())
  
  feeStructureId  String
  feeStructure    FeeStructure @relation(fields: [feeStructureId], references: [id])
  
  name            String   // "Droits universitaires"
  description     String?
  amount          Int      // Montant en FCFA
  
  type            FeeType
  required        Boolean  @default(true)
  
  createdAt       DateTime @default(now())
}

enum FeeType {
  TUITION              // Frais de scolarité
  REGISTRATION         // Droits universitaires
  INSURANCE            // Assurance
  MEDICAL              // Visite médicale
  EXAM                 // Frais d'examen
  LAB                  // TP/Labo
  SPORTS               // Sport & activités
  LIBRARY              // Bibliothèque
  INTERNSHIP           // Stage
  THESIS               // Mémoire/Thèse
  CARD                 // Carte étudiant
  OTHER                // Autre
}
```

---

## 3. Modes de Paiement

### 3.1 Vue d'Ensemble

| Mode | Usage | Avantages | Inconvénients |
|------|-------|-----------|---------------|
| **MTN Mobile Money** | 70% | Instantané, partout | Commission 2-3% |
| **Orange Money** | 20% | Idem MTN | Commission 2-3% |
| **Banque** | 5% | Pas de commission | Lent, reçu manuel |
| **Espèces** | 5% | Pas de commission | File d'attente, erreurs |

### 3.2 Workflow de Paiement

```
┌──────────────────────────────────────────────────────────┐
│                  WORKFLOW GÉNÉRAL                         │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  1. Étudiant consulte sa situation financière           │
│     └─> Montant dû, échéances                           │
│                                                           │
│  2. Choisit mode de paiement                             │
│     ├─> Mobile Money (recommandé)                       │
│     ├─> Banque (génération références)                  │
│     └─> Espèces (sur place)                             │
│                                                           │
│  3. Effectue le paiement                                 │
│     ├─> Mobile Money: Paiement via téléphone            │
│     ├─> Banque: Virement avec référence unique          │
│     └─> Espèces: Caisse sur campus                      │
│                                                           │
│  4. Confirmation automatique                             │
│     └─> Webhook (Mobile Money)                          │
│     └─> Import relevé (Banque)                          │
│     └─> Saisie caissier (Espèces)                       │
│                                                           │
│  5. Génération reçu                                      │
│     ├─> Numéro unique                                    │
│     ├─> QR code vérification                            │
│     ├─> PDF téléchargeable                              │
│     └─> Envoi par SMS/Email                             │
│                                                           │
│  6. Mise à jour compte étudiant                          │
│     └─> Solde mis à jour                                │
│     └─> Déblocage accès (si applicable)                 │
│                                                           │
└──────────────────────────────────────────────────────────┘
```

---

## 4. Intégration Mobile Money

### 4.1 MTN Mobile Money (Cameroun)

#### API Collection (Paiement)

```typescript
// Service MTN MoMo

import axios from 'axios';
import { v4 as uuidv4 } from 'uuid';

interface MoMoConfig {
  environment: 'sandbox' | 'production';
  subscriptionKey: string;
  userId: string;
  apiKey: string;
  callbackUrl: string;
}

class MTNMoMoService {
  private config: MoMoConfig;
  private baseUrl: string;
  private accessToken?: string;
  
  constructor(config: MoMoConfig) {
    this.config = config;
    this.baseUrl = config.environment === 'production'
      ? 'https://proxy.momoapi.mtn.cm'
      : 'https://sandbox.momodeveloper.mtn.com';
  }
  
  // 1. Créer accès token
  async createAccessToken(): Promise<string> {
    const auth = Buffer.from(
      `${this.config.userId}:${this.config.apiKey}`
    ).toString('base64');
    
    const response = await axios.post(
      `${this.baseUrl}/collection/token/`,
      {},
      {
        headers: {
          'Authorization': `Basic ${auth}`,
          'Ocp-Apim-Subscription-Key': this.config.subscriptionKey
        }
      }
    );
    
    this.accessToken = response.data.access_token;
    return this.accessToken;
  }
  
  // 2. Request to Pay (Demander paiement)
  async requestToPay(params: {
    amount: number;
    phoneNumber: string;
    externalId: string;  // Votre référence (ex: paymentId)
    message: string;
  }) {
    if (!this.accessToken) {
      await this.createAccessToken();
    }
    
    const referenceId = uuidv4();
    
    try {
      await axios.post(
        `${this.baseUrl}/collection/v1_0/requesttopay`,
        {
          amount: params.amount.toString(),
          currency: 'XAF',
          externalId: params.externalId,
          payer: {
            partyIdType: 'MSISDN',
            partyId: params.phoneNumber.replace(/\+/g, '')  // Ex: 237670000000
          },
          payerMessage: params.message,
          payeeNote: 'Frais de scolarité IUT Douala'
        },
        {
          headers: {
            'Authorization': `Bearer ${this.accessToken}`,
            'X-Reference-Id': referenceId,
            'X-Target-Environment': this.config.environment,
            'Ocp-Apim-Subscription-Key': this.config.subscriptionKey,
            'Content-Type': 'application/json',
            'X-Callback-Url': this.config.callbackUrl
          }
        }
      );
      
      return { success: true, referenceId };
      
    } catch (error) {
      console.error('MTN MoMo error:', error.response?.data);
      throw new Error('Échec de la demande de paiement');
    }
  }
  
  // 3. Vérifier statut du paiement
  async getTransactionStatus(referenceId: string) {
    if (!this.accessToken) {
      await this.createAccessToken();
    }
    
    const response = await axios.get(
      `${this.baseUrl}/collection/v1_0/requesttopay/${referenceId}`,
      {
        headers: {
          'Authorization': `Bearer ${this.accessToken}`,
          'X-Target-Environment': this.config.environment,
          'Ocp-Apim-Subscription-Key': this.config.subscriptionKey
        }
      }
    );
    
    return response.data;
    /*
    Response example:
    {
      "amount": "100000",
      "currency": "XAF",
      "externalId": "payment_123",
      "payer": {
        "partyIdType": "MSISDN",
        "partyId": "237670000000"
      },
      "status": "SUCCESSFUL" | "PENDING" | "FAILED",
      "reason": "..."
    }
    */
  }
}

// Utilisation dans votre application

class PaymentService {
  private momo: MTNMoMoService;
  
  constructor() {
    this.momo = new MTNMoMoService({
      environment: process.env.MOMO_ENV as 'sandbox' | 'production',
      subscriptionKey: process.env.MOMO_SUBSCRIPTION_KEY!,
      userId: process.env.MOMO_USER_ID!,
      apiKey: process.env.MOMO_API_KEY!,
      callbackUrl: `${process.env.APP_URL}/api/webhooks/momo`
    });
  }
  
  async initiatePayment(
    studentId: string,
    amount: number,
    phoneNumber: string
  ) {
    // 1. Créer enregistrement paiement en DB
    const payment = await this.prisma.payment.create({
      data: {
        studentId,
        amount,
        currency: 'XAF',
        method: 'MTN_MOMO',
        status: 'PENDING',
        phoneNumber
      }
    });
    
    // 2. Demander paiement à MTN
    const result = await this.momo.requestToPay({
      amount,
      phoneNumber,
      externalId: payment.id,
      message: `Frais scolarité - ${payment.id.substring(0, 8)}`
    });
    
    // 3. Sauvegarder référence MTN
    await this.prisma.payment.update({
      where: { id: payment.id },
      data: {
        providerReference: result.referenceId,
        metadata: { momoReferenceId: result.referenceId }
      }
    });
    
    return payment;
  }
}
```

#### Webhook (Callback MTN)

```typescript
// Route: POST /api/webhooks/momo

import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    
    // Body example:
    // {
    //   "externalId": "payment_123",
    //   "status": "SUCCESSFUL",
    //   "amount": "100000",
    //   "currency": "XAF",
    //   ...
    // }
    
    const paymentId = body.externalId;
    const status = body.status;
    
    // Mettre à jour le paiement
    const payment = await prisma.payment.update({
      where: { id: paymentId },
      data: {
        status: status === 'SUCCESSFUL' ? 'COMPLETED' : 'FAILED',
        completedAt: status === 'SUCCESSFUL' ? new Date() : null,
        failureReason: status === 'FAILED' ? body.reason : null,
        metadata: body
      },
      include: { student: true }
    });
    
    if (status === 'SUCCESSFUL') {
      // Actions post-paiement
      await this.onPaymentSuccess(payment);
    }
    
    return NextResponse.json({ success: true });
    
  } catch (error) {
    console.error('Webhook error:', error);
    return NextResponse.json(
      { error: 'Webhook processing failed' },
      { status: 500 }
    );
  }
}

async function onPaymentSuccess(payment: Payment) {
  // 1. Générer reçu
  const receipt = await generateReceipt(payment);
  
  // 2. Envoyer par SMS
  await smsService.send(payment.phoneNumber, {
    message: `Paiement reçu: ${payment.amount} FCFA. Reçu: ${receipt.number}. Télécharger: ${receipt.url}`
  });
  
  // 3. Envoyer par email
  await emailService.send(payment.student.email, {
    subject: 'Confirmation de paiement',
    template: 'payment-receipt',
    data: { payment, receipt }
  });
  
  // 4. Mettre à jour compte étudiant
  await updateStudentBalance(payment.studentId, payment.amount);
  
  // 5. Débloquer accès si nécessaire
  await checkAndUnblockStudent(payment.studentId);
}
```

### 4.2 Orange Money

```typescript
// Service Orange Money (API similaire)

class OrangeMoneyService {
  async requestPayment(params: {
    amount: number;
    phoneNumber: string;
    orderId: string;
  }) {
    // 1. Obtenir token OAuth
    const token = await this.getAccessToken();
    
    // 2. Initier paiement
    const response = await axios.post(
      'https://api.orange.com/orange-money-webpay/cm/v1/webpayment',
      {
        merchant_key: process.env.OM_MERCHANT_KEY,
        currency: 'XAF',
        order_id: params.orderId,
        amount: params.amount,
        return_url: `${process.env.APP_URL}/payment/callback`,
        cancel_url: `${process.env.APP_URL}/payment/cancel`,
        notif_url: `${process.env.APP_URL}/api/webhooks/orange`,
        lang: 'fr',
        reference: params.orderId
      },
      {
        headers: {
          'Authorization': `Bearer ${token}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    return {
      paymentUrl: response.data.payment_url,
      payToken: response.data.pay_token
    };
  }
}
```

---

## 5. Gestion des Échéanciers

### 5.1 Paiement en Tranches

```typescript
model PaymentPlan {
  id            String   @id @default(uuid())
  studentId     String
  student       Student  @relation(fields: [studentId], references: [id])
  
  academicYear  String
  totalAmount   Int
  
  installments  Installment[]
  
  status        PaymentPlanStatus @default(ACTIVE)
  
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
}

model Installment {
  id              String   @id @default(uuid())
  paymentPlanId   String
  paymentPlan     PaymentPlan @relation(fields: [paymentPlanId], references: [id])
  
  number          Int      // 1, 2, 3, 4
  amount          Int
  dueDate         DateTime
  
  status          InstallmentStatus @default(PENDING)
  paidAt          DateTime?
  paidAmount      Int?
  
  paymentId       String?
  payment         Payment? @relation(fields: [paymentId], references: [id])
  
  @@index([paymentPlanId, dueDate])
}

enum InstallmentStatus {
  PENDING
  PAID
  LATE
  CANCELLED
}
```

### 5.2 Rappels Automatiques

```typescript
// Job planifié (cron)

import { CronJob } from 'cron';

// Chaque jour à 9h
new CronJob('0 9 * * *', async () => {
  
  // 1. Trouver échéances dues dans 7 jours
  const upcomingInstallments = await prisma.installment.findMany({
    where: {
      status: 'PENDING',
      dueDate: {
        gte: new Date(),
        lte: addDays(new Date(), 7)
      }
    },
    include: {
      paymentPlan: {
        include: { student: true }
      }
    }
  });
  
  // 2. Envoyer rappels
  for (const installment of upcomingInstallments) {
    const student = installment.paymentPlan.student;
    const daysRemaining = differenceInDays(
      installment.dueDate,
      new Date()
    );
    
    await smsService.send(student.phone, {
      message: `Rappel: Échéance de ${installment.amount} FCFA due dans ${daysRemaining} jours. Pay via Mobile Money sur erp.iut.cm`
    });
  }
  
  // 3. Trouver échéances en retard
  const lateInstallments = await prisma.installment.findMany({
    where: {
      status: 'PENDING',
      dueDate: {
        lt: new Date()
      }
    }
  });
  
  // 4. Marquer comme en retard + notifier
  for (const installment of lateInstallments) {
    await prisma.installment.update({
      where: { id: installment.id },
      data: { status: 'LATE' }
    });
    
    // Alert
    // ...
  }
  
}).start();
```

---

## 6. Bourses & Réductions

### 6.1 Types de Bourses

```prisma
model Scholarship {
  id            String   @id @default(uuid())
  
  name          String   // "Bourse d'Excellence"
  type          ScholarshipType
  
  amount        Int?     // Montant fixe
  percentage    Int?     // Ou pourcentage (ex: 50%)
  
  criteria      Json     // Critères d'éligibilité
  
  academicYear  String
  
  applications  ScholarshipApplication[]
  
  createdAt     DateTime @default(now())
}

enum ScholarshipType {
  EXCELLENCE       // Mérite académique
  SOCIAL           // Situation sociale
  MINESUP          // Bourse d'État
  ORPHAN           // Orphelin
  DISABILITY       // Handicap
  EMPLOYEE_CHILD   // Enfant d'agent
  SIBLING          // Réduction fratrie
  OTHER
}

model ScholarshipApplication {
  id              String   @id @default(uuid())
  
  scholarshipId   String
  scholarship     Scholarship @relation(fields: [scholarshipId], references: [id])
  
  studentId       String
  student         Student  @relation(fields: [studentId], references: [id])
  
  status          ApplicationStatus @default(PENDING)
  appliedAt       DateTime @default(now())
  reviewedAt      DateTime?
  reviewedBy      String?
  
  documents       Json?    // URLs des justificatifs
  
  @@unique([scholarshipId, studentId])
}

enum ApplicationStatus {
  PENDING
  APPROVED
  REJECTED
}
```

### 6.2 Application de Réductions

```typescript
async function calculateFinalAmount(
  studentId: string,
  baseAmount: number
): Promise<number> {
  // 1. Chercher bourses actives
  const scholarships = await prisma.scholarshipApplication.findMany({
    where: {
      studentId,
      status: 'APPROVED'
    },
    include: { scholarship: true }
  });
  
  let finalAmount = baseAmount;
  
  // 2. Appliquer réductions
  for (const app of scholarships) {
    const scholarship = app.scholarship;
    
    if (scholarship.amount) {
      // Montant fixe
      finalAmount -= scholarship.amount;
    } else if (scholarship.percentage) {
      // Pourcentage
      finalAmount -= (baseAmount * scholarship.percentage / 100);
    }
  }
  
  // 3. Minimum 0
  return Math.max(0, finalAmount);
}
```

---

## 7. Architecture Technique

### 7.1 Modèle Complet

````prisma
model Payment {
  id                  String   @id @default(uuid())
  
  studentId           String
  student             Student  @relation(fields: [studentId], references: [id])
  
  amount              Int      // Montant en FCFA
  currency            String   @default("XAF")
  
  method              PaymentMethod
  status              PaymentStatus @default(PENDING)
  
  // Références externes
  providerReference   String?  // Référence MTN/Orange
  bankReference       String?  // Référence bancaire
  receiptNumber       String?  @unique
  
  // Metadata
  phoneNumber         String?
  metadata            Json?
  
  // Dates
  initiatedAt         DateTime @default(now())
  completedAt         DateTime?
  
  failureReason       String?
  
  // Relations
  installmentId       String?
  
  createdAt           DateTime @default(now())
  updatedAt           DateTime @updatedAt
  
  @@index([studentId, status])
  @@index([receiptNumber])
}

enum PaymentMethod {
  MTN_MOMO
  ORANGE_MONEY
  BANK_TRANSFER
  CASH
  CARD
  OTHER
}

enum PaymentStatus {
  PENDING
  PROCESSING
  COMPLETED
  FAILED
  CANCELLED
  REFUNDED
}
````

### 7.2 Dashboard Financier

```typescript
// API: GET /api/finance/dashboard

export async function getFinanceDashboard(academicYear: string) {
  const stats = await prisma.$transaction([
    // Total attendu
    prisma.paymentPlan.aggregate({
      where: { academicYear },
      _sum: { totalAmount: true }
    }),
    
    // Total reçu
    prisma.payment.aggregate({
      where: {
        status: 'COMPLETED',
        createdAt: {
          gte: startOfAcademicYear(academicYear)
        }
      },
      _sum: { amount: true }
    }),
    
    // Par méthode
    prisma.payment.groupBy({
      by: ['method'],
      where: { status: 'COMPLETED' },
      _sum: { amount: true },
      _count: true
    }),
    
    // Impayés
    prisma.installment.count({
      where: {
        status: 'LATE',
        paymentPlan: { academicYear }
      }
    })
  ]);
  
  return {
    expected: stats[0]._sum.totalAmount || 0,
    collected: stats[1]._sum.totalAmount || 0,
    collectionRate: (stats[1]._sum.totalAmount / stats[0]._sum.totalAmount) * 100,
    byMethod: stats[2],
    latePayments: stats[3]
  };
}
```

---

**Prochaine étape:** Voir [Emploi du Temps](./05-SCHEDULING.md) ou [Documents](./08-DOCUMENTS-DIPLOMAS.md)
