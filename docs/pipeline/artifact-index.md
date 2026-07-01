# Aipex Client OS Artifact Index

Each project should store its artefacts under:

```text
projects/<project-slug>/docs/
```

## Required artefacts

| Stage | File | Owner |
|---|---|---|
| -1 | `00-idea-qualification.md` | Strategic Advisor |
| 0 | `00-brief.md` | Project Intake Specialist |
| 1 | `01-viability.md` | Market & Viability Analyst |
| 2 | `02-features.md` | Product Manager |
| 3 | `03-architecture.md` | WordPress Plugin Architect |
| 3b | `03b-tech-spec.md` | Senior Technical Writer |
| 4 | `04-ux-design.md` | UX/Product Designer |
| 5 | `05-database-review.md` | Database Specialist |
| 6 | `06-build.md` | Senior WordPress Developer |
| 7 | `07-qa-testing.md` | QA Engineer |
| 8 | `08-security-review.md` | Security Engineer |
| 9 | `09-code-review.md` | Code Reviewer |
| 10 | `10-marketing-strategy.md` | Marketing Strategist |
| 11 | `11-pricing-strategy.md` | Pricing Strategist |
| 12 | `12-legal-framework.md` | Legal & Licensing Groundwork |
| 13 | `13-roadmap-handoff.md` | Project Director / Handoff |

## Supporting files

Recommended supporting files:

```text
projects/<project-slug>/STATUS.md
projects/<project-slug>/DECISIONS.md
projects/<project-slug>/OPEN-QUESTIONS.md
projects/<project-slug>/RISKS.md
projects/<project-slug>/ROADMAP.md
projects/<project-slug>/CHANGELOG.md
```

## Rules

- Artefacts are project memory.
- Never overwrite a completed artefact without noting the revision reason.
- If a stage is revised, update the affected downstream artefacts.
- If context is missing, ask for the relevant artefact rather than guessing.
- Legal items needing review must be marked `[SOLICITOR]`.
- Unverified claims must be marked `[ASSUMPTION]`.
