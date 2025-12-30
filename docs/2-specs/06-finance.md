# 💰 Module Finance : La Comptabilité à Double Entrée

## La Philosophie Odoo appliquée aux Frais Scolaires

Oubliez la table `payments` avec une colonne `status`. C'est amateur.
Dans Skooly (comme dans Odoo), **TOUT est une écriture comptable (`Journal Entry`)**.

## Entités Principales ("Models")

### 1. `Invoice` (Facture - `account.move`)
Une inscription en L1 génère une FACTURE.
*   **Débit** : Compte Client (Étudiant X) -> 50,000 FCFA
*   **Crédit** : Compte Produit (Scolarité) -> 50,000 FCFA
*   **Status** : `DRAFT` -> `POSTED` (Validé) -> `PAID` (Soldé).

### 2. `Payment` (Paiement - `account.payment`)
Quand MTN Mobile Money nous envoie de l'argent.
*   **Débit** : Compte Banque (MTN MoMo) -> 50,000 FCFA
*   **Crédit** : Compte Client (Étudiant X) -> 50,000 FCFA

### 3. La Réconciliation (Le Magie)
Au départ, la facture est "Impayée".
Le paiement est "Non lettré".
Le système lie les deux : `Invoice.amount_residual` devient 0. La facture passe à **PAID**.

## Intégration Mobile Money (MTN / Orange)

### Le Problème de la Réalité
L'API MTN peut dire "Succès", mais l'argent n'est pas là. Ou l'inverse.
Skooly utilise un **Journal de Transition**.

1.  **RequestToPay** : On crée un paiement en statut `PENDING`.
2.  **Webhook** : MTN appelle Skooly -> "Transaction X Réussie".
3.  **Validation** : Skooly passe le paiement à `POSTED` et réconcilie la facture.

### Gestion de l'Échec
Si MTN échoue, le paiement passe à `REJECTED`. La facture reste `OPEN`.
L'étudiant voit toujours "Impayé".

## Pourquoi c'est mieux ?
*   **Audit** : On sait exactement combien d'argent est "En cours chez MTN" vs "En banque".
*   **Tranches** : Si l'étudiant paie 20,000 sur 50,000, la facture reste `OPEN` avec un résiduel de 30,000. C'est natif.

## Code Snippet (Structure)

```typescript
interface JournalEntry {
  id: string;
  type: 'invoice' | 'payment';
  date: Date;
  lines: JournalItem[]; // Débit/Crédit
  state: 'draft' | 'posted';
}
```
