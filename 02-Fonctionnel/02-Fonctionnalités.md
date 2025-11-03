---
title: Principales fonctionnalités
status: "draft"
---
> Cette page liste les fonctionnalités livrées / en cours / prévues, avec leur périmètre et leurs règles de gestion.
> Les domaines couverts côté application sont exposés par les routeurs **`forecasts`**, **`collections`** (+ **`collections_wizard`**), **`clients`** (+ **`client_wizard`**) et **`scenarios`**, en plus des routes **core** (compte, organisation, rôles, SSO, invitations, tableau de bord).

**Légende statut** : ✅ livré  |  🧪 POC / partiel  |  🚧 en cours  |  🔜 backlog

---

## 1) Prévisions (Forecasts)

**Objectif.** Centraliser et fiabiliser la prévision par canal (Commerce, Web, Marketing), sur un référentiel commun (SKU, période, client).
**Fonctions.**

* **Import de fichiers** (Excel/CSV) avec validation (unicité `(client, sku, period)`, format, encodage). **Endpoints** : `POST /imports` ; erreurs standardisées `VALIDATION_ERROR` (ex. « Duplicate (client, sku, period) »). 🧪/🚧
* **Consultation / édition de lignes de prévision** (par filtres période, canal…). **Permissions** : `VIEW_FORECASTS`, `EDIT_FORECASTS`. 🚧
* **Indicateurs** (ex. MAPE agrégé par canal via `/kpi`). 🔜

**Règles de gestion clés.**

* **Clé fonctionnelle** : `(client, sku, period)` unique.
* **Workflow 3 états** : `Brouillon → Active → Archived`.
    > à réévaluer en fonction du besoin.  

---

### 1.1) Snapshots (versions de prévision)

**Objectif.** Geler une **photo** des prévisions validées pour servir de base aux scénarios S&OP.
**Fonctions.**

* **Création / gestion des snapshots** (`MANAGE_SNAPSHOTS`). 🧪
* **Lecture d’un snapshot** : `GET /snapshots/{id}`. 🧪

---

## 3) Scénarios S&OP (what‑if)

**Objectif.** Simuler l’impact d’hypothèses (lead time, démarque, boost volume…) à partir d’un snapshot, comparer et publier.
**Fonctions.**

* **Créer / éditer un scénario** (`CREATE_SCENARIO`, `EDIT_SCENARIO`). 🧪
* **Lancer une simulation** : `POST /scenarios/{id}/simulate` (temps cible < 2 min au niveau produit/canal). 🔜 (objectif produit)  
* **Comparer base vs scénario** (delta KPI, MAPE). 🔜

---

## 4) Collections (assortiments)

**Objectif.** Structurer les lignes/assortiments (familles, capsules…), préparer les dépôts par collection.
**Fonctions.**

* **Consultation des collections** (`VIEW_COLLECTIONS`). 🧪
* **Assistant de création** (`collections_wizard`) pour guider la saisie. 🧪 (routeur présent)

---

## 5) Clients / Canaux

**Objectif.** Gérer les comptes clients et canaux de vente pour piloter la prévision.
**Fonctions.**

* **Consultation des clients** (`VIEW_CLIENTS`). 🧪
* **Assistant de création** (`client_wizard`). 🧪 (routeur présent) 

---

## 6) Tableaux de bord & KPI

**Objectif.** Offrir une vue synthétique (MAPE, complétude dépôt, progression workflow).
**Fonctions.**

* **Dashboard** applicatif (route `dashboard`). 🧪
* **Endpoint KPI** : `GET /kpi` (filtres période, canal). 🧪

---

## 7) Import / Export & Intégrations

* **Import** : Excel `*.xlsx` (onglet `Forecast`) ou CSV UTF‑8 ; colonnes requises : `sku, client, period, qty, price`. 🧪/🚧
* **Export** : CSV, Excel, NDJSON (streaming grande vol.). 🧪/🚧
* **Webhooks sortants** : `forecast.validated`, `scenario.published`, `import.failed` (signés HMAC). 🔜

---

## 8) Sécurité & Administration

**Périmètre.** Authentification, SSO, invitations, organisations, rôles/permissions.

* **SSO OIDC / Azure AD** (JWT) pour utilisateurs, **PAT** pour intégrations. 🧪/🚧 (spec API)
* **Invitations** utilisateurs, **gestion des rôles**, **organisation** : routeurs `invitation`, `role`, `organization`, `user` inclus. 🚧
* **RBAC** (mappage par rôle) — extrait du seed :

  | Fonctionnalité                                                                        | Web | Commerce | Demand Planning | IT |
  | ------------------------------------------------------------------------------------- | --: | -------: | --------------: | -: |
  | Voir les prévisions (`VIEW_FORECASTS`)                                                |   ✓ |        ✓ |               ✓ |  ✓ |
  | Éditer les prévisions (`EDIT_FORECASTS`)                                              |   ✓ |        ✓ |               ✓ |  ✓ |
  | Importer des prévisions (`IMPORT_FORECASTS`)                                          |   ✓ |        ✓ |               ✓ |  ✓ |
  | Gérer les snapshots (`MANAGE_SNAPSHOTS`)                                              |     |          |               ✓ |  ✓ |
  | Voir clients (`VIEW_CLIENTS`)                                                         |   ✓ |        ✓ |               ✓ |  ✓ |
  | Voir collections (`VIEW_COLLECTIONS`)                                                 |   ✓ |        ✓ |               ✓ |  ✓ |
  | Créer/éditer scénarios (`CREATE_SCENARIO`/`EDIT_SCENARIO`)                            |     |          |               ✓ |  ✓ |
  | Admin core (créer/éditer/supprimer rôle, inviter/supprimer user, éditer organisation) |     |          |                 |  ✓ |

  *Source* : `ROLE_PERMISSIONS` (seed DB) et énum `ValidPermissions`.  

---

## 9) Parcours utilisateurs (synthèse)

* **Dépôt de prévision** : import fichier → validation technique → corrections → validation métier → consolidation (snapshot). 🧪/🚧  
* **Simulation S&OP** : dupliquer snapshot → paramétrer hypothèses → simuler → comparer KPI → publier. 🔜 (objectif < 2 min)  

---

## 10) Endpoints de référence (extraits)

| Verbe                            | Endpoint                   | Usage fonctionnel                |
| -------------------------------- | -------------------------- | -------------------------------- |
| **POST**                         | `/imports`                 | Dépôt de fichier de prévisions   |
| **GET**                          | `/snapshots/{id}`          | Consulter une version consolidée |
| **POST**                         | `/scenarios/{id}/simulate` | Lancer une simulation what‑if    |
| **GET**                          | `/kpi`                     | Suivre les indicateurs agrégés   |
| **GET**                          | `/reference/sku`           | Référentiel produit              |
| Swagger disponible sur `/docs`.  |                            |                                  |

