# Aipex Client OS – Data Model Decisions

## Purpose

Record the resolved decisions from `01-system-data-model.md` before moving into database schema and UX structure.

## Resolved Decisions

| Area | Decision | Reason | Status |
|---|---|---|---|
| AI run record | Store both a summary and full interaction record. | Summary enables quick inspection; full record preserves auditability and reproducibility. | Approved |
| Project assets | Use a protected project asset store. | Project files may contain client data, screenshots, exports, credentials, plans or sensitive development information. | Approved |
| Decisions | Use append-only decision records. | Preserves audit integrity. If a decision changes, create a new superseding decision instead of editing history. | Approved |

## Implementation Notes

### AI Run Records

Each AI run should include:

- Project ID
- Stage ID
- Agent ID
- Summary
- Full interaction record
- Input artefacts
- Output artefacts
- Result status
- Model/provider
- Started date
- Completed date

Sensitive content handling will be defined during the security architecture stage.

### Protected Project Asset Store

Assets should not rely on the standard WordPress Media Library for MVP because access control and audit trails need to be project-aware.

Required asset fields:

- Asset ID
- Project ID
- Title
- File path or external URL
- Asset type
- Source
- Access level
- Notes
- Created date

### Append-only Decisions

Decision records should not be overwritten. If a decision changes:

1. Add a new decision record.
2. Mark it as superseding the previous decision.
3. Preserve the original decision for audit history.

Required decision fields:

- Decision ID
- Project ID
- Stage ID
- Decision text
- Reason
- Reversible yes/no
- Status
- Supersedes decision ID
- Date

## Remaining Non-Blocking Question

| Question | Blocking? | Current Position |
|---|---|---|
| Should GitHub events be pulled on demand or synchronised into local tables? | No | MVP can begin with repository links and manual references. |

## Definition of Done

- [x] AI run storage decision recorded
- [x] Asset storage decision recorded
- [x] Decision history model recorded
- [x] Remaining non-blocking question identified

## Next Action

Proceed to database schema and UX structure.