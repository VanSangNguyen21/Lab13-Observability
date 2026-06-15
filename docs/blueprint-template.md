# Day 13 Observability Lab Report

> **Instruction**: Fill in all sections below. This report is designed to be parsed by an automated grading assistant. Ensure all tags (e.g., `[GROUP_NAME]`) are preserved.

## 1. Team Metadata
- [GROUP_NAME]: Nguyen Van Sang
- [REPO_URL]: https://github.com/VanSangNguyen21/Lab13-Observability.git
- [MEMBERS]:
  - Member A: Nguyen Van Sang | Role: Individual (Logging, PII, Tracing, Enrichment, SLOs, Alerts, Dashboards, Incident Response)

---

## 2. Group Performance (Auto-Verified)
- [VALIDATE_LOGS_FINAL_SCORE]: 100/100
- [TOTAL_TRACES_COUNT]: 21
- [PII_LEAKS_FOUND]: 0

---

## 3. Technical Evidence (Group)

### 3.1 Logging & Tracing
- [EVIDENCE_CORRELATION_ID_SCREENSHOT]: docs/images/correlation_id_evidence.jpg
- [EVIDENCE_PII_REDACTION_SCREENSHOT]: docs/images/pii_redaction_evidence.jpg
- [EVIDENCE_TRACE_WATERFALL_SCREENSHOT]: docs/images/trace_waterfall_evidence.jpg
- [TRACE_WATERFALL_EXPLANATION]: The trace waterfall shows the parent span `/chat` calling the agent's `run` span, which in turn spawns a retrieval (`mock_rag.retrieve`) span and an LLM generation (`mock_llm.generate`) span. When the `rag_slow` incident is enabled, the retrieve span latency spikes to 2500ms, representing 94% of the overall request latency.

### 3.2 Dashboard & SLOs
- [DASHBOARD_6_PANELS_SCREENSHOT]: docs/images/dashboard_6_panels.jpg
- [SLO_TABLE]:
| SLI | Target | Window | Current Value |
|---|---:|---|---:|
| Latency P95 | < 3000ms | 28d | 827.1ms |
| Error Rate | < 2% | 28d | 0.0% |
| Cost Budget | < $2.5/day | 1d | $0.0021 |

### 3.3 Alerts & Runbook
- [ALERT_RULES_SCREENSHOT]: docs/images/alert_rules.jpg
- [SAMPLE_RUNBOOK_LINK]: [alerts.md](file:///C:/ai_vinuni/code_vinuni/Lab13-Observability/docs/alerts.md#L3-L15)

---

## 4. Incident Response (Group)
- [SCENARIO_NAME]: rag_slow
- [SYMPTOMS_OBSERVED]: API response times spike from ~650ms to over 3100ms, causing significant latency for client requests and breaching the 3000ms SLO target.
- [ROOT_CAUSE_PROVED_BY]: The log trace shows a retrieval operation latency of 2500ms under `mock_rag.retrieve` which accounts for almost the entire request overhead.
- [FIX_ACTION]: Disable the `rag_slow` toggle by calling `/incidents/rag_slow/disable`.
- [PREVENTIVE_MEASURE]: Implement a strict 1.0-second timeout configuration on retrieval requests. If the retrieval service exceeds this duration, it must fail fast, log a warning, and fall back to a baseline search or a cached answer.

---

## 5. Individual Contributions & Evidence

### Nguyen Van Sang
- [TASKS_COMPLETED]: Completed all tasks individually, including implementing Correlation ID middleware, PII scrubbing (emails, VN phones, CCCDs, passports, credit cards, and addresses), log enrichment (user_id_hash, session, feature, model, env), SLO setup, alerting rules, and simulated dashboard evidence.
- [EVIDENCE_LINK]: [middleware.py](file:///C:/ai_vinuni/code_vinuni/Lab13-Observability/app/middleware.py), [logging_config.py](file:///C:/ai_vinuni/code_vinuni/Lab13-Observability/app/logging_config.py), [main.py](file:///C:/ai_vinuni/code_vinuni/Lab13-Observability/app/main.py), [pii.py](file:///C:/ai_vinuni/code_vinuni/Lab13-Observability/app/pii.py)

---

## 6. Bonus Items (Optional)
- [BONUS_COST_OPTIMIZATION]: (Description + Evidence)
- [BONUS_AUDIT_LOGS]: (Description + Evidence)
- [BONUS_CUSTOM_METRIC]: (Description + Evidence)
