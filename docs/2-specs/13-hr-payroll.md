# 👔 Module HR & Payroll : La Paie Complexe

## On ne gère pas les CDI classiques
Pour les CDI, il y a Sage Paie.
Skooly gère le **Cauchemar Administratif Universitaire** : Les Vacataires.

## Le Problème
Un enseignant vacataire est payé à l'heure, selon un taux qui dépend de son grade, du type de cours (CM/TD/TP), et s'il a dépassé son quota.

## Entités Principales

### 1. `TeachingContract`
*   `type`: `PERMANENT` | `VACATAIRE`
*   `hourly_rate_cm`: 5000 FCFA
*   `hourly_rate_td`: 3000 FCFA

### 2. `Timesheet` (Généré depuis Attendance)
C'est ici que la magie opère.
Les données du module **Présences** sont agrégées.
*   "M. Fofana a fait 10h de CM et 5h de TD en Octobre".

### 3. `PayrollRun` (Bulletin de Paie Simplifié)
*   Calcul brut : (10 * 5000) + (5 * 3000) = 65,000 FCFA.
*   Retenues fiscales (IRPP simplifié pour vacataires).
*   **Paiement** : Virement bancaire ou Mobile Money (Bulk Payment).

## Gestion des Quotas
Le système alerte si un vacataire dépasse le plafond légal d'heures (pour éviter la requalification en CDI).
