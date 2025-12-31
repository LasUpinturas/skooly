# Philosophie d'Architecture : Event-Driven Modular Monolith

## 1. Le Problème
Les ERPs monolithiques traditionnels deviennent rapidement des "plats de spaghettis" :
*   Le module Finance importe le module Étudiant, qui importe le module Note...
*   Modifier une règle de calcul de moyenne casse la facturation.
*   L'ajout d'une fonctionnalité devient exponentiellement coûteux et risqué.

À l'inverse, les Microservices sont trop complexes à déployer et maintenir pour une équipe réduite.

## 2. La Solution : Le Monolithe Modulaire (Inspiré d'Odoo)

Nous adoptons l'architecture qui a fait le succès d'Odoo, mais avec une stack moderne (NestJS/TypeScript).

### A. Modularité Stricte
Le code est divisé en modules isolés (`Core`, `Academic`, `Finance`).
*   **Règle d'or** : Un module ne doit JAMAIS importer le Service d'un autre module.
*   **Communication** : Les modules communiquent exclusivement via des **Événements**.

### B. Architecture Event-Driven (Réactive)
Au lieu d'appels de fonctions directs (Couplage Fort), nous utilisons des événements (Couplage Faible).

*   *Mauvais (Couplage)* :
    ```typescript
    // Dans StudentService
    financeService.createInvoice(studentId); // Student dépend de Finance
    ```

*   *Bon (Event)* :
    ```typescript
    // Dans StudentService
    eventBus.emit('student.registered', { studentId });
    // FinanceModule écoute 'student.registered' et crée la facture.
    // Student ne sait même pas que Finance existe.
    ```

### C. La Vérité est dans les Faits (Event Sourcing Light)
Nous ne stockons pas uniquement l'état actuel (`Status: PAID`), mais la séquence de faits qui y a conduit (`InvoiceCreated` -> `PaymentReceived`). Cela garantit une traçabilité totale (Audit Trail) indispensable pour la finance et la scolarité.
