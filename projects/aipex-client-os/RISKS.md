# Aipex Client OS – Risk Register

| Risk | Severity | Probability | Mitigation | Owner |
|---|---|---|---|---|
| Scope expands into a full commercial SaaS before internal MVP is usable. | High | Medium | Keep current scope internal-only and classify future commercial features separately. | Pipeline Director |
| AI-generated code repeats previous mistakes or loses context. | High | High | Force artefact-first workflow, decision log and review gates. | Pipeline Director |
| GitHub integration becomes too complex for MVP. | Medium | Medium | Start with structured artefacts and manual/assisted GitHub workflows before deeper automation. | Technical Architect |
| WordPress alone may not be suitable for long-running AI orchestration. | Medium | Medium | Consider external worker services during architecture stage without forcing them into MVP. | Technical Architect |
