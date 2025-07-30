# 1. Architecture

```text
/
├── frontend/                      # Code HTML + CSS + logique front Python
│   ├── app/                       # Application Jinja2
│   │   ├── __init__.py
│   │   ├── main.py                # Lanceur uvicorn ou serveur HTTP simple
│   │   ├── routes.py              # Gestion des routes
│   │   ├── templates/             # Fichiers Jinja2
│   │   │   ├── base.html
│   │   │   ├── home.html
│   │   │   └── ...
│   │   ├── static/                # Fichiers CSS / JS / images
│   │   │   ├── styles.css
│   │   │   └── ...
│   │   └── utils.py               # Fonctions auxiliaires front
│   ├── requirements.txt
│   └── gunicorn.conf.py          # Si production via gunicorn
│
├── backend/                      # API Python (FastAPI, Flask, etc.)
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py               # API launcher
│   │   ├── models/               # Pydantic / ORM
│   │   ├── services/             # Appels IA, traitement, etc.
│   │   ├── api/                  # Routes REST
│   │   └── utils.py
│   ├── requirements.txt
│   └── Dockerfile                # Image GPU-ready pour Serveur IA
│
├── shared/                       # Code réutilisé (schémas, validations, constantes)
│   ├── __init__.py
│   ├── config.py
│   └── schemas.py
│
├── .gitlab-ci.yml                # Pipeline CI/CD
├── .env.example                  # Variables d'environnement (modèle)
└── README.md
```

---

:warning: a partir de ce point, ce qui est présenté dépasse l'état d'avancement du développement. Tout ce qui suit est donc à titre indicatif, et est largement sujet à variations.

---

# 2. Modèle de données (idée)

| Table | Champs | Particularités |
|-------|--------|----------------|
| `forecast_snapshot` | id, created_at | Version pour scénarios |
| `forecast_line` | snapshot_id FK, sku, qty, period | Clé composite |
| `scenario` | id, base_snapshot_id, status, assumptions JSONB | Index GIN |

---

# 3. Endpoints REST principaux

| Méthode | URL | Action |
|---------|-----|--------|
| POST | `/imports/` | Upload Excel/CSV |
| GET | `/snapshots/{id}` | Détails snapshot |
| POST | `/scenarios/{id}/simulate` | Simulation scénario |
| GET | `/dashboards/kpi` | KPIs agrégés |

Swagger : `/docs`.

---

# 4. Templates Jinja2 – principes

* Héritage `base.html` ; blocs `content` spécifiques.
* Variables filtrées avant rendu.
* Macros pour composants (bouton, alert).

---

# 5. Tests & qualité

| Outil | Usage |
|-------|-------|
| `pytest` + `httpx` | Tests API |
| `factoryboy` | Fixtures |
| `coverage.py` | ≥ 85 % |
| `pre‑commit` | black, isort, flake8 |

---

# 6. CI/CD GitLab

Pipeline : **lint → test → build‑docker → deploy‑staging** Secrets via _CI/CD variables_ ; staging Docker Compose ; option K8s (R3).

---

# 7. Definition of Done (tech)

1. CI vert
2. Migrations appliquées
3. Wiki à jour
4. Changelog bump

---

# 8. Logging & Observability

| Sujet | Outil / Pratique | Notes |
|-------|------------------|-------|
| **Logging structuré** | [`loguru`](https://github.com/Delgan/loguru) | JSON logs, corrélation `request_id` injectée via middleware |
| **Metrics** | `prometheus-fastapi-instrumentator` | Exposition `/metrics`; scrape par Prometheus + Grafana |
| **Tracing** | OpenTelemetry (OTLP) → Jaeger | Trace ID inclus dans logs & headers |
| **Alerting** | Grafana alert rules | Seuils : latence P95 \> 500 ms, erreurs \> 2 % |

---

# 9. Configuration & Secrets

* **Pattern : `pydantic.BaseSettings`** → toutes les variables env typées.
* 
* **Fichiers `.env`** pour le local uniquement ; CI/CD injecte via _Variables GitLab_.
* 
* **Secrets sensibles** (tokens, passwords) stockés dans variables protégées.

```python
from pydantic import BaseSettings 

class Settings(BaseSettings):
    db_url: str
    redis_url: str
    secret_key: str
    class Config:
        env_file = ".env"
```

---

# 10. Sécurité

| Zone | Mesure | Implémentation |
|------|--------|----------------|
| Authentification | SSO Azure AD (OIDC) | JWT access token dans cookies HttpOnly |
| Rate limiting | 100 req/min/IP | Middleware `slowapi` |
| Input validation | Pydantic schemas | 422 sur payload invalide |
| Vulnérabilité | OWASP scan CI | ZAP docker in pipeline |
| Stockage | Secrets chiffrés (AES‑256) | Vault / KMS |

---

# 11. Performance cibles

* **Latence API (P95)** : \< 300 ms pour `/dashboards/kpi`.
* **Simulation S&OP** : \< 2 min pour 50 k lignes.
* **Tests charge** : `locust` 500 utilisateurs concurrents, tolérance erreur \< 1 %.