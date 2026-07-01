# Aipex Client OS – Decision Log

| Date | Decision | Reason | Stage | Reversible? |
|---|---|---|---|---|
| 2026-07-01 | Aipex Client OS will be developed using its own pipeline. | Validates the methodology while building the platform. | Foundation | No |
| 2026-07-01 | Current scope is internal Aipex staff only. | Avoids premature commercial complexity and keeps MVP focused. | Brief | Yes |
| 2026-07-01 | GitHub is the source of truth. | Prevents context loss and makes work auditable. | Foundation | No |
| 2026-07-01 | Core principles are: never lose project knowledge, never ask twice, everything should be inspectable. | These principles directly address the problems of context loss, repeated questioning and opaque AI actions. | Brief | No |
| 2026-07-01 | AI run records will store both summary and full interaction record. | Summary enables quick inspection; full record preserves auditability and reproducibility. | Data Model | Yes |
| 2026-07-01 | Project assets will use a protected project asset store. | Project files may contain sensitive client or development information and need project-aware access control. | Data Model | Yes |
| 2026-07-01 | Decisions will be append-only. | Preserves audit integrity; changed decisions should supersede previous decisions rather than overwrite them. | Data Model | No |
