# 💸 Module SaaS Billing : La Pompe à Cash

> **Objectif** : Transformer Skooly en machine à rente.
> L'école doit payer pour continuer à utiliser le service. Pas de paiement = Pas d'accès.

---

## 1. Le Modèle de Licence (Pricing Engine)

On vend des **Licences Flottantes** ou des **Quotas Fixes**.

### A. Les Plans (Plans)
1.  **Starter (Free)** : < 100 Étudiants. (Pour les petites écoles pilotes).
2.  **Growth** : Payant au volume (ex: 500 FCFA / Étudiant / An).
3.  **Enterprise** : Licence Site (Illimité) + Modules Premium (IA, Anti-fraude).

### B. Gestion des Quotas (Hard vs Soft Limits)
*   **Storage** : "Vous avez utilisé 9.8Go / 10Go". (Alertes à 80%, 90%).
    *   *Action* : À 100%, l'upload est bloqué (mais on peut toujours télécharger).
*   **Étudiants** : "Licence 5000 étudiants".
    *   *Action* : Si on essaie d'inscrire le 5001ème, popup bloquante : "Upgradez votre plan".

---

## 2. Le Super-Admin Dashboard (God Mode)

Nous (WistantKode) avons besoin d'une interface pour gérer les clients.

*   **Tenant List** : Voir toutes les écoles inscrites.
*   **Health Score** : "L'IUT Douala n'a pas syncé depuis 3 jours".
*   **Impersonate (Login As)** : Se connecter en tant que "Admin IUT" pour débugger un problème (avec Audit Log strict).
*   **Kill Switch** : Désactiver un Tenant instantanément en cas de fraude ou impayé.

---

## 3. Workflow de Facturation (Invoicing)

Comment Skooly facture l'Université ?

1.  **Comptage Mensuel** : Le 1er du mois, un Job compte les "Étudiants Actifs".
2.  **Génération Facture** : PDF généré automatiquement.
3.  **Envoi** : Email au DAF de l'université.
4.  **Recouvrement Automatique (Dunning)** :
    *   J+5 : Rappel 1.
    *   J+15 : Rappel 2 + "Suspension imminente".
    *   J+30 : **Mode Read-Only**. L'école peut voir les données mais ne peut plus rien modifier/ajouter.

---

## 4. Architecture Technique (Isolation)

Comment être sûr que IUT Douala ne paie pas pour IUT Yaoundé ?

*   `TenantSubscription` Model : Lie un `Tenant` à un `Plan`.
*   Middleware `BillingGuard` :
    *   Avant chaque écriture (`POST /students`), vérifie si `CurrentCount < MaxQuota`.
    *   Si KO -> `403 Payment Required`.
    *   Utilise **Redis** pour ne pas compter en SQL à chaque requête.

---

## 5. Intégration Paiement SaaS (Comment ils nous paient ?)
*   **Virement Bancaire** (B2B classique). On valide manuellement dans le God Mode.
*   **Carte Bancaire / Mobile Money** (Stripe/CinetPay) : Pour les petites écoles, paiement self-service.
