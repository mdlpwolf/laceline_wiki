## 01‑Projet – Plateforme **Demand Planning & IA**

Cette page — destinée à tous les acteurs métier et IT — synthétise le **“pourquoi” et le “quoi”** du projet : contexte, valeur attendue, indicateurs de succès, parties prenantes, périmètre et positionnement face au marché. Elle sert de porte‑d’entrée fonctionnelle ; les détails techniques sont documentés dans les répertoires `03‑Technique` et `04‑API & Data`.

---

### 1. Contexte & problématique

La prévision des ventes s’appuie aujourd’hui sur des fichiers Excel volumineux ; intégration manuelle des prévisions de vente de chaque canal (Commerce, Web, Marketing) par la Demand Planning, ce qui génère **surcharge manuelle, erreurs de liaison et manque de traçabilité** [usmsupplychain.com](https://usmsupplychain.com/excel-functions-for-demand-planning/).\
Selon McKinsey, **80 % des entreprises industrielles fonctionnent encore avec un processus S&OP séquentiel et peu automatisé**, ralentissant la prise de décision [McKinsey & Company](https://www.mckinsey.com/capabilities/operations/our-insights/autonomous-supply-chain-planning-for-consumer-goods-companies).

> **Ambition** : une plateforme unique, collaborative, permettant d'automatiser la consolidation, simuler plusieurs scénarios S&OP et réduire de 70 % le temps global de génération d’un Purchase Order.

---

### 2. Vision & objectifs mesurables

Ces indicateurs, pour l'heure présentés à titre indicatifs, permettrons de confirmer que nous avons atteint la cible, tant d'un point de vue technique que fonctionnel. 

| Objectif | Cible 2026 | Comment on le mesure |
|----------|------------|----------------------|
| **Réduction temps retraitement de données**  | \-70% par rapport à l'historique | Chrono sur 3 campagnes |
| **Forecast Accuracy** | +8 pts (MAPE global) | KPI MAPE / canal [apics.org](https://www.apics.org/docs/default-source/industry-content/apics-sop-performance-report.pdf) |
| **Taux de complétude dépôt** | ≥ 95 % avant J‑15 | Rapport dashboards |
| **Scénarios S&OP simulés \< 2 min** | 3 scénarios / campagne | Temps d’exécution Celery |

---

### 3. Parties prenantes & rôles

| Groupe | Responsabilités clés |
|--------|----------------------|
| **Demand Planning** | Gouvernance des données, consolidation, lancement des scénarios |
| **Commerce / Web / Marketing** | Dépôt des prévisions, ajustements promo |
| **Achats / Supply** | Consultation capacité, PO, suivi ruptures |
| **IT (équipe 4 pers.)** | Dev, CI/CD, hébergement, support |
| **Sponsor – Direction** | Validation budgets, arbitrage priorités |

---

### 4. Référentiel marché (benchmark)

| Solution | Points saillants “best‑in‑class” | Référence |
|----------|----------------------------------|-----------|
| **Anaplan** | Scénarios _Three Whats_ : duplication instantanée + scorecards  | [Anaplan Inc](https://www.anaplan.com/content/dam/anaplan/assets/documents/white-paper/intelligent-forecasting-agile-scenario-planning-white-paper.pdf)[Bedford Consulting](https://bedfordconsulting.com/how-can-you-leverage-anaplan-for-effective-scenario-planning-and-analysis) |
| **SAP IBP** | Simulation “on‑the‑fly” dans Excel add‑in + operators en mode simulation  | [SAP Help Portal](https://help.sap.com/docs/SAP_INTEGRATED_BUSINESS_PLANNING/b28ffdd739bf45678ef36c44e64652d7/2362278d46c848bdb087cd89dc22cde7.html?locale=en-US&utm_source=chatgpt.com)[SAP Help Portal](https://help.sap.com/docs/SAP_INTEGRATED_BUSINESS_PLANNING/b28ffdd739bf45678ef36c44e64652d7/68b72b830d824e45aff5c9941a862be0.html) |
| **Kinaxis RapidResponse** | Moteur _Concurrent Planning_ in‑memory, classement KPI | [Kinaxis](https://www.kinaxis.com/en/what-concurrent-planning)[SupplyChainBrain](https://www.supplychainbrain.com/ext/resources/0-whitepapers/Kinaxis/supply-chain-planning-kinaxis.pdf) |
| **Centric Planning** | IA retail/fashion, ajustements assortiment & prix | [Centric Software](https://www.centricsoftware.com/blog/how-to-master-ai-demand-forecasting/) |
| **Tendance 2025** | Généralisation IA + automatisation S&OP | [McKinsey & Company](https://www.mckinsey.com/capabilities/operations/our-insights/autonomous-supply-chain-planning-for-consumer-goods-companies) |

Ces références guident nos exigences : import Excel drag‑drop, scénario “clone & compare”, calculs \< 2 min, dashboards rôle‑basés.

---

### 5. Périmètre & exclusions (Lot 1)

* **Inclus** : produits finis lingerie (France + E‑commerce), workflow 4 états, dashboards KPI, scénario S&OP v1.
* **Exclus** : échantillons, SMS, IA de pricing dynamique avancée (phase R5).

---

### 6. Roadmap & jalons macro

| Date cible | Release | Contenu majeur |
|------------|---------|----------------|
| **12 août 2025** | **0.2 POC** | Import drag‑drop + validation auto |
| **26 sept. 2025** | **R1** | Workflow complet + dashboards |
| **28 nov. 2025** | **R2** | Scénarios S&OP v1 (what‑if) |
| **T1 2026** | **R3** | Optimisation stock / capacity, API PLM |

_(Cycles Agile 3 semaines, démo publique à chaque sprint ; backlog détaillé dans `05‑Agile & Backlog`.)_

---

### 7. Glossaire express

| Terme | Définition |
|-------|------------|
| **Scenario** | Copie isolée d’une prévision pour test _what‑if_. |
| **Snapshot** | Capture horodatée des prévisions validées. |
| **MAPE** | Mean Absolute Percentage Error, mesure d’écart en %. |
| **S&OP** | Sales & Operations Planning, processus d’alignement demande‑capacité. |

---

### 8. Prochaines actions

1. **Valider ce contenu** en réunion Teams (Planner) – ajouter commentaires inline.
2. **Mettre à jour le Canvas CdC** si de nouveaux KPIs ou exclusions émergent.
3. **Créer issues GitLab** pour chaque jalon Roadmap.

---

> **Besoin d’approfondir ?** Consultez `02‑Fonctionnel/Parcours utilisateurs` pour le détail des écrans et `03‑Technique/Architecture` pour la cartographie FastAPI + Docker.

© 2025 – Équipe IT & Demand Planning