# 1. Personas & rôles

**Déposant** (Commerce / Web / Marketing)
• Charge un fichier Excel/CSV ou saisit en ligne les prévisions.
• Suit les erreurs retournées et corrige jusqu’à validation

**Validateur métier** (Demand Planning)
• Vérifie la cohérence business (MOQ, prix, promotions).
• Ajuste les hypothèses (markdown, allocation) avant publication.
• Consulte la demande consolidée pour préparer les PO.
• Compare capacité usine vs besoin, suggère ajustements.

**Administrateur IT**
• Gère les comptes et les intégrations (ERP, PLM, SSO).

:warning: **Contrôle de Gestion**
• Suite aux échanges du 30/07/2025, il devient apparent que l'application peut aussi comprendre un espace dédié au Contrôle de Gestion. En effet, de nombreux suivis réalisés par cette équipe reprend les mêmes données que celles que nous traiteront pour la DP.
• Horizon Septembre [Release 1], faire une récolte du besoin en regard de ce qui est déjà réalisé pour fournir au CG une interface de consultation (suite réflexion, prepack, kit & tout le toutim)

---

# 2. Parcours utilisateur – Dépôt → Validation

1. **Drag‑drop** du fichier dans l’écran *Importer prévisions*.
2. **Validation technique** automatique : format, doublons `(Client, SKU, Période)`, valeurs numériques ≥ 0.
3. **Validation métier** : contrôle MOQ, markdown ≤ 30 %, lead‑time, etc.
4. **Snapshot “Validé”** archivé et visible pour la Supply.

---

# 3. Workflow & statuts

| Statut            | Éditable par               | Transition                  |
| ----------------- | -------------------------- | --------------------------- |
| **Brouillon**     | Déposant                   | → Validé tech               |
| **Validé tech**   | Système                    | → Validé métier / Brouillon |
| **Validé métier** | Demand Planning ; Déposant | → Consolidé / Brouillon     |
| **Consolidé**     | Lecture seule              | — A étudier                 |

:buld: TODO - Une fois le workflow établi, proposer des alertes & notifications en fonction des erreurs ou validations.

---

# 4. Règles de gestion essentielles

* **Unicité** : `(Client, SKU, Période)` unique par snapshot.
* **Quantité minimale** : `qty ≥ 0`.
* **Lead‑time fournisseur ≥ MOQ** (appliqué en v2 scenarios).
* **Seuils d’alerte** : à définir

---

# 5. Scénarios & simulation S\&OP

* **Créer** un scénario : clone du snapshot validé (< 10 s).
* **Appliquer** des drivers (variables - exemples : ∆ vente ; revue ↗ | ↘ ) via formulaire ou import.
* **Simuler** : recalcul demande, stock, P\&L simplifié.
* **Comparer** : tableau Baseline / Scénario A / Scénario B + KPIs.
* **Publier** le scénario choisi

---

# 6. KPI & Dashboards

| KPI                         | Description                  | Objectif 2026 |
| --------------------------- | ---------------------------- | ------------- |
| **MAPE global**             | Écart moyen absolu           | +8 pts        |
| **OTIF**                    | On‑Time In‑Full              | ≥ 97 %        |
| **Complétude dépôt**        | % lignes déposées avant J‑15 | ≥ 95 %        |
| **Cycle dépôt → consolidé** | Durée moyenne (h)            | ≤ 24 h        |

---

# 7. Glossaire

*Driver* · *Snapshot* · *Scenario* · *PO* · *MAPE*

---

# 8. Definition of Ready / Definition of Done

**Ready** : règles définies, maquette écran validée
**Done** : tests verts, wiki mis à jour, KPI visible