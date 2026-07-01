# Aipex Client OS – Product Architecture

## Purpose

Define the high-level architecture that governs all future development. This document is the architectural source of truth before implementation begins.

## Core Modules

1. Dashboard
2. Project Manager
3. Pipeline Engine
4. AI Orchestrator
5. Agent Registry
6. Artifact Manager
7. Knowledge Store
8. Decision Log
9. Risk Register
10. Asset Library
11. GitHub Integration
12. Documentation Generator
13. QA Centre
14. Security Centre
15. Release Manager
16. Settings & Integrations

## Information Flow

Idea → Project → Pipeline Stage → Specialist Agent → Artifact → Review → GitHub → Knowledge Store → Next Stage

Every stage produces or updates artefacts. Artefacts become permanent project memory and are linked to decisions, risks and GitHub history.

## Source of Truth

1. Approved GitHub artefacts.
2. Decision Log.
3. Risk Register.
4. Linked project assets.
5. Chat session (temporary working memory only).

## Integrations

### MVP
- GitHub
- WordPress
- Local file uploads

### Phase 2
- Dropbox
- Google Drive
- OpenAI
- Anthropic
- Google Gemini
- Microsoft 365
- Jira / Linear / Trello

## AI Orchestration

The Pipeline Director coordinates specialist agents.

Each agent:
- receives only the artefacts relevant to its stage;
- cannot overwrite approved decisions without recording a revision;
- returns structured outputs;
- hands off to the next agent.

## Architectural Principles

- GitHub is the permanent source of truth.
- Never lose project knowledge.
- Never ask twice.
- Everything should be inspectable.
- One stage at a time.
- Human approval at stage gates.
- Build and review are separate responsibilities.

## Long-term Vision

Evolve from an internal development platform into a commercial AI software engineering operating system while preserving compatibility with the internal workflow.
