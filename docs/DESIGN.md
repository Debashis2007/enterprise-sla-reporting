# Design: Enterprise SLA Reporting

**Project:** `enterprise-sla-reporting`  
**Parent system design:** `05-model-monitoring-observability.md`

## 1. What this POC demonstrates

Compute tenant SLA compliance and error-budget burn from availability/latency inputs.

## 2. Architecture (POC)

```text
GET /sla/{tenant}?availability=&ttft_p99_ms= → burn + meets_sla
```

## 3. Patterns used (and why)

| Pattern | Why used | Where in code |
|---------|----------|---------------|
| Error budget burn | SLOs need quantitative burn, not vibes. | `error_budget_burn`. |
| Multi-metric SLA | Availability alone ignores latency pain. | Availability + TTFT. |
| Tenant grain | Enterprise reports are per-customer. | Path param `tenant`. |

## 4. Key endpoints

`GET /health`, `GET /sla/{tenant}`

## 5. Tradeoffs / POC limits

Inputs are query params — production pulls from metric store.

## 6. How to run

See the **Run (self-contained POC)** section in [`../README.md`](../README.md).

This folder is self-contained and can be published as its own GitHub repository.

## 7. Design walkthrough video

Narrated with **ElevenLabs Debpro voice** and Debpro still image (via [GitaProject](/Users/deb/Development/GenAI/GitaProject)):

- Video: [`video/design-overview.mp4`](./video/design-overview.mp4)
- Script: [`video/narration.txt`](./video/narration.txt)

