# Use Case: Enterprise SLA Reporting

**YouTube walkthrough:** [Enterprise Sla Reporting — System Design #Shorts](https://youtu.be/pplbZInzQqk)

**Design doc:** [docs/DESIGN.md](./docs/DESIGN.md) — architecture, patterns, and why.


**Parent system design:** [05 — Model Monitoring & Behavior Observability](../05-model-monitoring-observability.md)

## Users & problem

Enterprise customers require monthly evidence of latency, availability, and error budgets—per region and sometimes per model pin.

## Requirements & SLOs

| Requirement | Target |
|-------------|--------|
| Metrics | Availability, TTFT/ITL, error rate |
| Grain | Tenant + region + model revision |
| Export | PDF/CSV + API |
| Trust | Tamper-evident aggregates |

## Design (from parent)

```
Per-tenant metric pipelines → immutable monthly rollups
  → SLA calculator (error budget burn)
  → customer-facing report + status
```

Reuse infra metrics taxonomy from **05**; isolate tenant labels carefully (cardinality!).

## Specializations

| Concern | SLA reporting choice |
|---------|----------------------|
| Cardinality | Pre-aggregate tenant metrics |
| Exclusions | Documented maintenance windows |
| Disputes | Raw sample retention policy |
| Residency | Reports stored in-region |

## Failure modes

- Missing tenant labels → treat as pipeline bug; don’t invent data.
- Canary traffic skews SLA → exclude internal canaries from customer burn.
- Over-promise metrics you don’t measure → contract only to instrumented SLOs.




## Design walkthrough (opens on GitHub)

> **Watch on YouTube:** [Enterprise Sla Reporting — System Design #Shorts](https://youtu.be/pplbZInzQqk)


![Design overview](docs/video/design-overview.gif)

Full narrated video (download): [docs/video/design-overview.mp4](docs/video/design-overview.mp4)

## Run (self-contained POC)

This folder is a **standalone** project (safe to split into its own GitHub repo).

```bash
cd enterprise-sla-reporting
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
PYTHONPATH=. python -m uvicorn app.main:app --reload --port 8000
```

```bash
curl -s http://127.0.0.1:8000/health | jq
```

curl -s "http://127.0.0.1:8000/sla/acme?availability=0.999" | jq

---

**Copyright (c) 2026 Debashis Bhattacharjee. All Rights Reserved.**  
Unauthorized copying or redistribution of this material is prohibited.  
GitHub: [Debashis2007](https://github.com/Debashis2007)

