# Stage Definition of Done Template

Every stage must end with a Definition of Done section.

```markdown
## Definition of Done

- [ ] Required artefact created or updated
- [ ] Open questions listed
- [ ] Locked decisions recorded
- [ ] Risks added or updated
- [ ] GitHub issue/branch/commit guidance included where relevant
- [ ] Next action stated clearly
- [ ] Stage gate passed or failure stated plainly
```

## Stage gate wording

Use one of:

- `PASS`
- `PASS WITH RISKS`
- `RESHAPE`
- `BLOCKED`
- `NO-GO`

## Rules

- Do not mark a stage complete if any blocking question remains unanswered.
- Do not run ahead unless explicitly told `run all`.
- If viability is `NO-GO`, stop the pipeline.
- If legal input is needed, mark `[SOLICITOR]`.
- If information is not verified, mark `[ASSUMPTION]`.
