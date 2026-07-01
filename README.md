# Aipex Client OS

Aipex Client OS is the GitHub-first operating system for developing WordPress and WooCommerce products through a structured specialist-agent pipeline.

This repository is the source of truth for project briefs, viability decisions, feature packaging, architecture, technical specifications, UX plans, database reviews, build notes, QA reports, security reviews, code reviews, marketing strategy, pricing, legal groundwork and handoff artefacts.

## Core principle

Quality beats speed, but the pipeline must prevent overbuilding. Every feature is challenged against one question:

> Is this needed for the first paying customer?

If not, it is phase 2, an add-on, or parked.

## Pipeline mode

This repository uses the Option 4 specialist-agent model. Each stage is handled by a focused role and produces a named Markdown artefact.

## Repository layout

```text
docs/pipeline/
  master-prompt.md
  agent-roster.md
  stage-map.md
  artifact-index.md
  github-workflow.md

docs/templates/
  project-artifact-set.md
  stage-definition-of-done.md
  issue-template.md

projects/_template/docs/
  00-brief.md
  01-viability.md
  02-features.md
  03-architecture.md
  03b-tech-spec.md
  04-ux-design.md
  05-database-review.md
  06-build.md
  07-qa-testing.md
  08-security-review.md
  09-code-review.md
  10-marketing-strategy.md
  11-pricing-strategy.md
  12-legal-framework.md
  13-roadmap-handoff.md
```

## How to use with ChatGPT

Start a new project with:

```text
Use the Aipex Client OS pipeline in w4l-clevedon/aipex-client-os.
Create a new project called <project-name>.
Run Stage -1 only.
```

Continue a project with:

```text
Continue <project-name> using the Aipex Client OS pipeline.
Show status first, then run the next stage.
```

## Status

Initial framework seeded.
