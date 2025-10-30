---
title: Parcours utilisateur – Dépôt → Validation
status: "draft"
---

1. **Drag‑drop** du fichier dans l’écran *Importer prévisions*.
2. **Validation technique** automatique : format, doublons `(Client, SKU, Période)`, valeurs numériques ≥ 0.
3. **Validation métier** : contrôle MOQ, markdown ≤ 30 %, lead‑time, etc.
4. **Snapshot “Validé”** validé et visible pour la DP.

## Workflow & statuts

| Statut            | Éditable par               | Transition                  |
| ----------------- | -------------------------- | --------------------------- |
| **Brouillon**     | Déposant                   | → Validé tech               |
| **Validé tech**   | Système                    | → Validé métier / Brouillon |
| **Validé métier** | Demand Planning ; Déposant | → Consolidé / Brouillon     |
| **Consolidé**     | Lecture seule              | — A étudier                 |

:buld: TODO - Une fois le workflow établi, proposer des alertes & notifications en fonction des erreurs ou validations.