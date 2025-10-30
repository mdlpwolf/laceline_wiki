---
title: LaceLine Wiki
status: "draft"
tags: ["architecture", "howto"]
---

---

:warning: DEPRECATED - Nécessite une mise à jour

---

🎀 Bienvenue !\
Ce wiki centralise **la documentation fonctionnelle et technique** du projet afin que chacun puisse comprendre, contribuer et suivre l’avancement.

---

## :rocket: Vision rapide

> Réduire de **70 %** le temps nécessaire à la création d’un Purchase Order et fiabiliser la prévision grâce à une plateforme collaborative, intégrée et évolutive (IA ready).

---

## :map: Plan du wiki

| Section | À quoi ça sert ? | Etat |
|---------|------------------|------|
| [01‑Projet](01-Projet) | Contexte, objectifs, KPIs clés, référentiel marché | :white_check_mark: |
| [02‑Fonctionnel](02-Fonctionnel) | Parcours utilisateurs, règles de gestion, scénarios S&OP | :white_check_mark: |
| [03‑Technique](03-Technique) | Architecture FastAPI & Docker, normes de dev, CI/CD | :white_check_mark: |
| [04‑API & Data](04-API-Data) | Spécifications API, schémas JSON, mapping Data | :white_check_mark: |
| [05‑Agile & Backlog](05-Agile-Backlog) | Liens vers Boards & épics GitLab | :soon: |
| [06‑How‑to / FAQ](06-How-to-FAQ) | Guides pas‑à‑pas et questions fréquentes | :soon: |
| [07‑Connaissance métier](07-Connaissance-metier) | Processus Demand Planning, glossaire Supply‑Chain | :soon: |
| [Changelog](Changelog) | Historique des versions livrées | :soon: |

---

## :key: Termes essentiels

### :bikini: Métier

* **SKU**
* **Pièce**
* **UVC**
* **Kit**
* **Prepack**
* **Vrac**
* **Fond de rayon**
* **Lead-time**
* **GMS**
* **FDV**

### :gear: Technique

* **Forecast Snapshot** : capture datée des prévisions validées.
* **Scenario** : copie isolée d’un snapshot pour simulation _what‑if_ S&OP (voir 02‑Fonctionnel).
* **Workflow 4 états** : `Brouillon → Validé tech → Validé métier → Consolidé`.
* **MAPE** : indicateur d’exactitude des prévisions.\
  (:soon:[Glossaire complet](07-Connaissance-metier/Glossaire) )

---

## :checkered_flag: Démarrage rapide pour les nouveaux

1. **Cloner le repo** : `git clone git@gitlab.com:org/dmp.git`
2. **Lancer l’environnement local** : ( :grey_exclamation: pour l'instant, seul le frontend est développé :grey_exclamation:)
   1. `cd frontend/app`
   2. `uvicorn main:app --reload`
3. **Ouvrir l’app** : [http://localhost:8000](http://localhost:8000)
4. **Consulter la doc API** : [http://localhost:8000/docs](http://localhost:8000/docs)\
   _(Tout est expliqué pas‑à‑pas dans _[_03‑Technique/Setup local_](03-Technique/Setup-local)_)_.

---

© 2025 — IT Wolf