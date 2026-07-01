# Aipex Client OS – Project Rules

## Prime Directive

Aipex Client OS must be developed using Aipex Client OS.

Every meaningful change must follow the pipeline, produce or update the relevant artefact, and leave an auditable trail in GitHub.

## Core Principles

1. **Never lose project knowledge.**  
   Code can be regenerated. Context cannot.

2. **Never ask twice.**  
   Reuse known information from approved artefacts, GitHub, linked assets and prior decisions before asking for it again.

3. **Everything should be inspectable.**  
   Every AI decision, prompt, artefact, stage transition, code change, review, GitHub action and release decision should leave a trace.

## Development Rules

- One pipeline stage at a time unless explicitly approved.
- No coding before brief, product definition, architecture and MVP scope are agreed.
- No feature enters MVP unless it is required for the first useful internal release.
- Every feature must belong to one of: MVP, Phase 2, Paid/Commercial Future, Parked.
- Build and review responsibilities remain separate.
- Security is designed early, not bolted on later.
- GitHub is the source of truth, not chat history.
- Approved artefacts override memory and assumptions.
- If context is missing, retrieve the artefact before continuing.
- If a decision changes, update `DECISIONS.md`.
- If a risk appears, update `RISKS.md`.
- If a question blocks progress, update `OPEN-QUESTIONS.md`.

## Internal-Only Scope

Current audience: Aipex staff only.

External/commercial customer features are not MVP unless explicitly approved as future-facing architecture support.

## Definition of Done for Any Change

- Relevant artefact updated.
- Decision or risk recorded if applicable.
- Next action clear.
- GitHub commit created.
- Scope classification clear: MVP, Phase 2, Future Commercial, or Parked.
