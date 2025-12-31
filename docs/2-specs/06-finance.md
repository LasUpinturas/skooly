# Spécification Module Finance : Comptabilité & Paiements

## 1. Le Problème
La gestion financière est le point critique.
*   **Fraude** : Reçus falsifiés, argent liquide "perdu", collusions.
*   **Complexité** : Réconciliation entre les paiements bancaires (UBA), le Mobile Money, et la comptabilité.
*   **Suivi** : Impossible de savoir en temps réel "Qui doit quoi".

## 2. La Solution : Système Hybride & Comptabilité Double Entrée

### A. Modèle de Paiement Hybride
Skooly s'adapte à la réalité locale du "Guichet Unique".
Voir le détail dans `06-finance.md` (qui est déjà bien structuré, je vais juste le nettoyer).

1.  **Gros Montants (Scolarité)** : Via Banque (UBA).
    *   L'étudiant paie à la banque.
    *   Skooly importe le relevé bancaire.
    *   Réconciliation automatique par Matricule ou ID Transaction.
2.  **Petits Montants (Frais)** : Via Mobile Money (API native).
    *   Confirmation instantanée.

### B. Comptabilité à Partie Double (Ledger)
Nous ne stockons pas juste "Payé = Oui". Nous générons des écritures comptables réelles.
Chaque transaction impacte deux comptes :
*   *Facturation* : Crédit "Produit Scolarité" / Débit "Compte Recevable Étudiant".
*   *Paiement* : Crédit "Compte Recevable Étudiant" / Débit "Banque".

*Avantage* : Auditabilité totale et export facile vers les logiciels comptables (Sage).

### C. Recouvrement Automatisé (Dunning)
Le système gère la relance des impayés.
*   **J-7** : SMS de rappel avant échéance.
*   **J+1** : SMS de retard + Pénalité (configurable).
*   **J+30** : Blocage automatique des services (Accès notes, Réinscriptions).

## 3. Modèle de Données
*   `Invoice`, `InvoiceLine`.
*   `Payment` (Method: CASH, BANK, MOMO).
*   `LedgerEntry` (Debit, Credit, Account).
