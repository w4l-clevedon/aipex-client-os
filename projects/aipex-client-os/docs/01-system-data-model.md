# Aipex Client OS – System Data Model

## Purpose

Define the core information model for Aipex Client OS before database, UI, REST API or plugin implementation begins.

The goal is to make every project, stage, decision, AI run, artefact and GitHub action traceable.

## Core Entities

### Project

The main container for all work.

Examples:
- Aipex Client OS
- Aipex Podcast System
- CaseFlow
- Securime
- Aipex CRM

Key fields:
- Project ID
- Name
- Slug
- Description
- Status
- Current stage
- Owner
- GitHub repository
- Created date
- Updated date

Relationships:
- Has many stages
- Has many artefacts
- Has many decisions
- Has many risks
- Has many assets
- Has many AI runs
- Links to one or more GitHub repositories

---

### Stage

A step in the Aipex pipeline.

Examples:
- Brief
- Viability
- Features
- Architecture
- Build
- QA
- Security Review
- Code Review
- Pricing
- Legal

Key fields:
- Stage ID
- Project ID
- Stage number
- Stage name
- Status
- Assigned agent
- Started date
- Completed date
- Gate result

Relationships:
- Belongs to project
- Has one or more artefacts
- Has many AI runs
- Has stage decisions
- Has stage risks

---

### Agent

A specialist role used by the pipeline.

Examples:
- Pipeline Director
- Product Manager
- UX Designer
- Database Specialist
- Senior WordPress Developer
- QA Engineer
- Security Engineer
- Code Reviewer

Key fields:
- Agent ID
- Name
- Role
- Description
- Stage ownership
- Input requirements
- Output requirements

Relationships:
- Can run many stages
- Can produce many AI runs
- Can produce artefacts

---

### Artefact

A permanent project output.

Examples:
- `00-brief.md`
- `03-architecture.md`
- `08-security-review.md`

Key fields:
- Artefact ID
- Project ID
- Stage ID
- Title
- File path
- Type
- Version
- Status
- Created by
- Created date
- Updated date

Relationships:
- Belongs to project
- Belongs to stage
- May be produced by AI run
- May create or update decisions, risks and tasks
- May link to GitHub commits

---

### Decision

A locked or important project choice.

Examples:
- Use custom tables instead of CPTs.
- Keep MVP internal-only.
- GitHub is the source of truth.

Key fields:
- Decision ID
- Project ID
- Stage ID
- Decision text
- Reason
- Reversible yes/no
- Status
- Date

Relationships:
- Belongs to project
- May belong to stage
- May come from artefact
- May affect future stages

---

### Risk

Something that could harm the project.

Examples:
- Scope expands too far before MVP.
- GitHub integration becomes too complex.
- AI loses context during development.

Key fields:
- Risk ID
- Project ID
- Stage ID
- Risk text
- Severity
- Probability
- Mitigation
- Owner
- Status

Relationships:
- Belongs to project
- May belong to stage
- May be created by review or AI run

---

### Task

A specific action required to progress the project.

Examples:
- Create UX wireframe.
- Build dashboard CPT.
- Add GitHub issue sync.

Key fields:
- Task ID
- Project ID
- Stage ID
- Title
- Description
- Status
- Priority
- Owner
- GitHub issue URL

Relationships:
- Belongs to project
- May belong to stage
- May link to GitHub issue
- May be created from artefact, decision or review finding

---

### Asset

A file or resource linked to the project.

Examples:
- Screenshots
- Logos
- Exports
- Mockups
- Client files
- Audio files
- PDFs

Key fields:
- Asset ID
- Project ID
- Title
- File path or external URL
- Asset type
- Source
- Notes
- Created date

Relationships:
- Belongs to project
- May be referenced by artefacts
- May be used by agents

---

### GitHub Repository

A repository linked to a project.

Key fields:
- Repository ID
- Project ID
- GitHub owner
- Repository name
- URL
- Default branch
- Visibility
- Status

Relationships:
- Belongs to project
- Has issues, branches, commits and pull requests

---

### GitHub Issue

A GitHub issue linked to project work.

Key fields:
- Issue ID
- Project ID
- Repository ID
- GitHub issue number
- Title
- Status
- Labels
- URL

Relationships:
- Belongs to repository
- May link to task
- May link to stage

---

### GitHub Commit

A recorded code or documentation change.

Key fields:
- Commit ID
- Repository ID
- SHA
- Message
- Author
- Date
- URL

Relationships:
- Belongs to repository
- May update artefact
- May complete task
- May relate to AI run

---

### AI Run

A specific AI action or specialist-agent execution.

Examples:
- Security Engineer reviewed AJAX endpoints.
- Product Manager created feature matrix.
- Pipeline Director updated project status.

Key fields:
- AI Run ID
- Project ID
- Stage ID
- Agent ID
- Prompt summary
- Input artefacts
- Output artefacts
- Result status
- Model/provider
- Started date
- Completed date

Relationships:
- Belongs to project
- Belongs to stage
- Belongs to agent
- May produce artefact
- May create decisions, risks or tasks
- May link to GitHub commit

---

### Review

A QA, security or code review result.

Key fields:
- Review ID
- Project ID
- Stage ID
- Review type
- Verdict
- Findings
- Severity
- Status
- Created date

Relationships:
- Belongs to project
- May create tasks
- May create risks
- May block release

---

### Release

A packaged version or milestone.

Examples:
- v0.1 internal prototype
- v1.0 internal MVP
- v2.0 commercial-ready release

Key fields:
- Release ID
- Project ID
- Version
- Title
- Status
- Release notes
- GitHub tag
- Date

Relationships:
- Belongs to project
- Has many tasks
- Has release artefacts
- Links to GitHub tag or release

## Entity Relationship Summary

```text
Project
 ├── Stages
 │    ├── Agents
 │    ├── AI Runs
 │    └── Artefacts
 ├── Decisions
 ├── Risks
 ├── Tasks
 ├── Assets
 ├── Reviews
 ├── Releases
 └── GitHub Repository
      ├── Issues
      ├── Commits
      └── Pull Requests
```

## Core Traceability Flow

```text
AI Run → produces Artefact
Artefact → creates or updates Decisions, Risks and Tasks
Task → links to GitHub Issue
GitHub Issue → links to Commit or Pull Request
Commit → updates Project
Project → updates Status
```

## Source of Truth

Priority order:

1. Approved GitHub artefacts
2. Decision Log
3. Risk Register
4. Project Status
5. Linked assets
6. Chat session context

Chat is temporary working memory. GitHub is permanent project memory.

## Minimum MVP Data

The MVP should support:

- Projects
- Stages
- Agents
- Artefacts
- Decisions
- Risks
- Tasks
- Assets
- GitHub repository link
- AI run log
- Project status

## Phase 2 Data

Later versions may add:

- Multiple repositories per project
- Pull request review mapping
- External storage providers
- Cost tracking per AI run
- Client-facing views
- Team permissions
- Billing/licensing data
- Multi-tenant commercial accounts

## Audit Trail

Every meaningful action should be inspectable:

- Who or what performed it
- When it happened
- What input was used
- What changed
- Which artefact was affected
- Which GitHub commit recorded it
- Whether it changed scope, risk, cost or release readiness

## WordPress Storage Recommendation

### Recommended CPTs

Use custom post types for objects that benefit from WordPress admin screens and editorial-style management:

- Project
- Artefact
- Agent
- Asset

### Recommended Custom Tables

Use custom tables for operational records, logs and high-volume data:

- Decisions
- Risks
- Tasks
- AI Runs
- Stage Runs
- Reviews
- GitHub Events
- Release Records

## Reasoning

Projects, artefacts, agents and assets are manageable content objects, so CPTs make early admin development faster.

Decisions, risks, AI runs, stage runs and GitHub events will grow quickly and require structured querying, filtering, indexing and audit integrity, so custom tables are safer and more scalable.

## Open Questions

| Question | Blocking? | Notes |
|---|---|---|
| Should AI run prompts be stored in full, summarised, or both? | Yes | Full prompts improve auditability but may expose sensitive data. |
| Should project assets live in the WordPress Media Library or a protected project asset store? | Yes | Sensitive client assets may need restricted storage. |
| Should GitHub events be pulled on demand or synchronised into local tables? | No | MVP can begin with repository links and manual references. |
| Should decisions be editable or append-only? | Yes | Append-only is better for audit integrity. |

## Definition of Done

- [x] Core entities defined
- [x] Entity relationships defined
- [x] MVP data requirements identified
- [x] Phase 2 data separated
- [x] WordPress storage approach proposed
- [x] Audit trail requirements documented
- [x] Open questions listed

## Next Action

Resolve the blocking open questions before moving into database schema and UX structure.