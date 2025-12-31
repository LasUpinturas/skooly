# Stratégie des Modules : Core vs Extensions

## 1. Le Problème
Vouloir tout faire tout de suite conduit à un produit moyen partout et excellent nulle part.
De plus, toutes les écoles n'ont pas les mêmes besoins ni le même budget.

## 2. La Solution : Approche "Lego"

Nous divisons le système en trois couches distinctes. Cela clarifie le développement, le pricing et la maintenance.

---

### A. Le Core (Le Cœur du Réacteur)
*Ce sans quoi le système ne peut pas démarrer.*
**Licence** : Open Source (MIT).
**Disponibilité** : Immédiate.

1.  **Identity & Access** : Gestion des utilisateurs, rôles, permissions (RBAC).
2.  **Multi-Tenancy** : Isolation des données entre établissements.
3.  **Academic Structure** : Départements, Filières, Années, Salles.
4.  **Enrollment Base** : Inscription administrative simple des étudiants.

### B. Les Modules Métier (Les Organes)
*Fonctionnalités essentielles pour une gestion quotidienne.*
**Licence** : Enterprise (Commercial).

1.  **Finance** : Facturation, Paiements (Cash/Banque), Mobile Money.
2.  **Attendance** : Suivi des présences (QR Code), Justificatifs.
3.  **Grades (LMD)** : Saisie des notes, Calcul des moyennes, PV de délibération.
4.  **Scheduling** : Emploi du temps simple, contraintes salles/profs.

### C. Les Extensions Premium (Le Luxe)
*Fonctionnalités à haute valeur ajoutée ou techniques.*
**Licence** : Enterprise + Add-on.

1.  **Library** : Gestion des prêts de livres.
2.  **Housing** : Gestion des cités universitaires.
3.  **Alumni** : Suivi post-graduation.
4.  **AI Services** : Détection de fraude, Prédiction de décrochage.

---

## 3. Matrice de Priorité (Roadmap par Valeur)

Nous développons les modules dans l'ordre de leur criticité pour la survie de l'établissement :
1.  **Cashflow** (Finance) : Si l'école ne peut pas encaisser, elle ferme.
2.  **Légalité** (Academic/Grades) : Si l'école ne peut pas diplômer, elle perd son accréditation.
3.  **Optimisation** (Attendance/Scheduling) : Gagner du temps et des ressources.
