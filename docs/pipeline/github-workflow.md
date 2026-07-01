# Aipex Client OS GitHub Workflow

## Source of truth

GitHub is the canonical project memory. Chat conversations are working sessions; repository artefacts are the permanent record.

## Branch naming

Use clear, stage-specific branch names:

```text
pipeline/<project-slug>/stage-00-brief
feature/<project-slug>/<feature-name>
fix/<project-slug>/<issue-name>
review/<project-slug>/<review-name>
release/<project-slug>/v1.0.0
```

## Commit message style

```text
<stage|feature|fix|docs|review>: <short action>
```

Examples:

```text
docs: add podcast system viability artefact
stage: complete CaseFlow UX design
feature: add floating podcast player foundation
review: add security findings for AJAX endpoints
```

## Issue conventions

Issue titles should start with the project and stage or feature:

```text
[Podcast System] Stage 2 — Features & Packaging
[CaseFlow] Feature — Respondent magic login
[Project OS] Review — Security findings
```

Recommended labels:

- `pipeline`
- `stage`
- `brief`
- `viability`
- `features`
- `architecture`
- `tech-spec`
- `ux`
- `database`
- `build`
- `qa`
- `security`
- `review`
- `marketing`
- `pricing`
- `legal`
- `handoff`
- `blocked`
- `mvp`
- `phase-2`
- `add-on`

## Pull request checklist

Every implementation PR should include:

- Linked stage artefact
- Linked issue
- What changed
- Screenshots if UI changed
- Security notes
- Test notes
- Known gaps
- Reviewer focus areas

## Stage completion

A stage is complete only when:

- The artefact exists in the project folder
- Open questions are listed
- Locked decisions are recorded
- Definition of Done is complete
- Next action is clear

## Build/review separation

The reviewer never rewrites code directly. Review stages return findings. The build stage or developer fixes them in a later commit or PR.
