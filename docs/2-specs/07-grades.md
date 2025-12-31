# Spécification Module Notes & Délibérations

## 1. Le Problème
Le calcul des moyennes dans le système LMD est arithmétiquement complexe (Crédits, Coefficients, Unités d'Enseignement, Compensation Intra-UE, Compensation Inter-UE, Dettes, Capitalisation).
Faire cela sur Excel est suicidaire et source d'erreurs gravissimes pour l'avenir des étudiants.

## 2. La Solution : Moteur de Calcul LMD

### A. Saisie des Notes
*   **Double Saisie (Aveugle)** : Pour les examens critiques, deux opérateurs saisissent les notes. Le système alerte en cas de divergence.
*   **Anonymat** : Saisie par numéro d'anonymat, décodé uniquement après validation.
*   **Verrouillage** : Une fois validée par le jury, une note devient immuable (sauf procédure exceptionnelle loguée).

### B. Algorithme de Délibération (Le Moteur)
Le système implémente les règles LMD CEMAC :
1.  **Moyenne EC** : Note CC (30%) + Note Exam (70%).
2.  **Moyenne UE** : Moyenne pondérée des ECs.
    *   Si Moyenne UE >= 10/20 : UE Validée (Crédits acquis).
    *   Si Moyenne UE < 10 : UE Non Validée.
3.  **Compensation Semestrielle** :
    *   Si Moyenne Semestre >= 10 : Le semestre est validé par compensation (toutes les UEs sont réputées acquises, sauf note éliminatoire).

### C. Le Procès Verbal (PV)
Le système génère le PV officiel de délibération (PDF) prêt à imprimer et signer.
Il contient les statistiques de réussite, la liste des admis, ajournés et exclus.

## 3. Intégrité des Données
Toute modification de note post-délibération déclenche une alerte "Suspicious Activity" aux administrateurs systèmes et au Doyen.
