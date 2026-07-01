# Aipex Client OS – Database Schema

## Purpose

Define the MVP database/storage structure for Aipex Client OS before implementation.

This schema supports the principles:

- Never lose project knowledge.
- Never ask twice.
- Everything should be inspectable.

## Storage Strategy

Use a hybrid WordPress storage model:

### Custom Post Types

Use CPTs for editorial-style objects that benefit from the WordPress admin UI:

- `aipex_project`
- `aipex_artifact`
- `aipex_agent`

### Custom Tables

Use custom tables for structured, append-heavy, audit-heavy and high-volume records:

- Decisions
- Risks
- Tasks
- Stage runs
- AI runs
- Reviews
- GitHub repositories
- GitHub events
- Protected assets
- Releases
- Audit log

## CPTs

### `aipex_project`

Purpose: Main project container.

Recommended meta fields:

| Meta Key | Type | Notes |
|---|---|---|
| `_aipex_project_slug` | string | Stable project slug |
| `_aipex_project_status` | string | active / paused / archived / completed |
| `_aipex_current_stage` | string | Current pipeline stage |
| `_aipex_owner_user_id` | int | Internal owner |
| `_aipex_primary_repo_id` | int | Linked GitHub repo table ID |
| `_aipex_project_scope` | string | internal / commercial / client |
| `_aipex_created_source` | string | manual / import / ai |

Indexes:

- WordPress post indexes already cover ID/status/date.
- Add meta queries sparingly; operational queries should use custom tables where possible.

---

### `aipex_artifact`

Purpose: Stores project artefact metadata. Markdown content may be stored in GitHub, locally, or both depending on implementation phase.

Recommended meta fields:

| Meta Key | Type | Notes |
|---|---|---|
| `_aipex_project_id` | int | Linked project post ID |
| `_aipex_stage_key` | string | Stage identifier |
| `_aipex_file_path` | string | GitHub or local path |
| `_aipex_artifact_type` | string | brief / architecture / review / legal etc |
| `_aipex_version` | string | Version number |
| `_aipex_status` | string | draft / approved / superseded |
| `_aipex_github_commit_sha` | string | Commit that last updated it |

---

### `aipex_agent`

Purpose: Stores specialist agent definitions.

Recommended meta fields:

| Meta Key | Type | Notes |
|---|---|---|
| `_aipex_agent_key` | string | Stable key |
| `_aipex_stage_owner` | string | Stage(s) owned |
| `_aipex_input_requirements` | longtext | Required inputs |
| `_aipex_output_requirements` | longtext | Required outputs |
| `_aipex_enabled` | bool | Enabled/disabled |

## Custom Tables

Table names should use the WordPress prefix:

```text
{$wpdb->prefix}aipex_decisions
{$wpdb->prefix}aipex_risks
{$wpdb->prefix}aipex_tasks
{$wpdb->prefix}aipex_stage_runs
{$wpdb->prefix}aipex_ai_runs
{$wpdb->prefix}aipex_reviews
{$wpdb->prefix}aipex_github_repositories
{$wpdb->prefix}aipex_github_events
{$wpdb->prefix}aipex_assets
{$wpdb->prefix}aipex_releases
{$wpdb->prefix}aipex_audit_log
```

---

## `aipex_decisions`

Append-only decision records.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | WP post ID for project |
| `stage_key` | VARCHAR(64) | Stage identifier |
| `decision_text` | LONGTEXT | Decision content |
| `reason` | LONGTEXT | Why decision was made |
| `status` | VARCHAR(32) | active / superseded / parked |
| `is_reversible` | TINYINT(1) | 0/1 |
| `supersedes_id` | BIGINT UNSIGNED NULL | Previous decision ID |
| `created_by` | BIGINT UNSIGNED NULL | WP user ID |
| `created_at` | DATETIME | UTC recommended |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY stage_key (stage_key),
KEY status (status),
KEY supersedes_id (supersedes_id),
KEY created_at (created_at)
```

---

## `aipex_risks`

Project and stage risks.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `stage_key` | VARCHAR(64) NULL | Related stage |
| `risk_text` | LONGTEXT | Risk description |
| `severity` | VARCHAR(16) | low / medium / high / critical |
| `probability` | VARCHAR(16) | low / medium / high |
| `mitigation` | LONGTEXT NULL | Mitigation plan |
| `owner_user_id` | BIGINT UNSIGNED NULL | Owner |
| `status` | VARCHAR(32) | open / mitigated / accepted / closed |
| `created_at` | DATETIME | Created date |
| `updated_at` | DATETIME | Updated date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY stage_key (stage_key),
KEY severity (severity),
KEY status (status)
```

---

## `aipex_tasks`

Project tasks and pipeline actions.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `stage_key` | VARCHAR(64) NULL | Related stage |
| `title` | TEXT | Task title |
| `description` | LONGTEXT NULL | Details |
| `priority` | VARCHAR(16) | low / normal / high / urgent |
| `status` | VARCHAR(32) | open / in_progress / blocked / done / parked |
| `owner_user_id` | BIGINT UNSIGNED NULL | WP user ID |
| `github_issue_url` | TEXT NULL | Linked issue |
| `created_at` | DATETIME | Created date |
| `updated_at` | DATETIME | Updated date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY stage_key (stage_key),
KEY status (status),
KEY priority (priority)
```

---

## `aipex_stage_runs`

Records each execution of a pipeline stage.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `stage_key` | VARCHAR(64) | Stage identifier |
| `agent_key` | VARCHAR(64) NULL | Agent used |
| `status` | VARCHAR(32) | pending / running / complete / blocked / failed |
| `gate_result` | VARCHAR(32) NULL | pass / pass_with_risks / blocked / no_go |
| `started_at` | DATETIME NULL | Start date |
| `completed_at` | DATETIME NULL | Completion date |
| `artifact_id` | BIGINT UNSIGNED NULL | Linked artefact post ID |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_stage (project_id, stage_key),
KEY status (status),
KEY agent_key (agent_key)
```

---

## `aipex_ai_runs`

Records specialist-agent AI activity.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `stage_run_id` | BIGINT UNSIGNED NULL | Related stage run |
| `agent_key` | VARCHAR(64) | Agent used |
| `provider` | VARCHAR(64) NULL | AI provider |
| `model` | VARCHAR(128) NULL | Model used |
| `summary` | LONGTEXT | Human-readable run summary |
| `full_record_path` | TEXT NULL | Protected stored full record path |
| `input_artifacts` | LONGTEXT NULL | JSON list |
| `output_artifacts` | LONGTEXT NULL | JSON list |
| `status` | VARCHAR(32) | complete / failed / partial |
| `created_at` | DATETIME | Created date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY stage_run_id (stage_run_id),
KEY agent_key (agent_key),
KEY provider_model (provider, model),
KEY created_at (created_at)
```

Security note: full interaction records should be stored in the protected asset store or another protected path, not as public media.

---

## `aipex_reviews`

QA, security and code review records.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `stage_key` | VARCHAR(64) NULL | Related stage |
| `review_type` | VARCHAR(32) | qa / security / code / product |
| `verdict` | VARCHAR(32) | ship / fix_then_ship / rework / blocked |
| `findings` | LONGTEXT | Findings JSON or markdown |
| `highest_severity` | VARCHAR(16) | low / medium / high / critical |
| `status` | VARCHAR(32) | open / resolved / accepted |
| `created_at` | DATETIME | Created date |
| `updated_at` | DATETIME | Updated date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY review_type (review_type),
KEY verdict (verdict),
KEY highest_severity (highest_severity),
KEY status (status)
```

---

## `aipex_github_repositories`

Linked GitHub repositories.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `owner` | VARCHAR(128) | GitHub owner/org |
| `repo` | VARCHAR(128) | Repo name |
| `url` | TEXT | Repo URL |
| `default_branch` | VARCHAR(128) | Usually main |
| `visibility` | VARCHAR(32) | public / private |
| `status` | VARCHAR(32) | active / archived |
| `created_at` | DATETIME | Created date |
| `updated_at` | DATETIME | Updated date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
UNIQUE KEY owner_repo (owner, repo),
KEY status (status)
```

---

## `aipex_github_events`

Optional local GitHub event cache.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `repository_id` | BIGINT UNSIGNED | Linked repository ID |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `event_type` | VARCHAR(64) | issue / commit / pr / release |
| `external_id` | VARCHAR(191) | GitHub ID/SHA/number |
| `title` | TEXT NULL | Event title |
| `url` | TEXT NULL | GitHub URL |
| `payload` | LONGTEXT NULL | JSON payload |
| `created_at` | DATETIME | Created date |

Indexes:

```sql
PRIMARY KEY (id),
KEY repository_id (repository_id),
KEY project_id (project_id),
KEY event_type (event_type),
KEY external_id (external_id),
KEY created_at (created_at)
```

MVP note: GitHub events can start as manual links and later become synced events.

---

## `aipex_assets`

Protected project asset records.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `title` | TEXT | Asset title |
| `asset_type` | VARCHAR(64) | image / pdf / export / prompt / record / other |
| `storage_driver` | VARCHAR(64) | local / github / dropbox / drive |
| `storage_path` | TEXT | Protected path or external reference |
| `mime_type` | VARCHAR(191) NULL | MIME type |
| `file_size` | BIGINT UNSIGNED NULL | Bytes |
| `access_level` | VARCHAR(32) | private / project / staff |
| `created_by` | BIGINT UNSIGNED NULL | WP user ID |
| `created_at` | DATETIME | Created date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY asset_type (asset_type),
KEY storage_driver (storage_driver),
KEY access_level (access_level),
KEY created_at (created_at)
```

Security note: protected assets should not be directly web-accessible by default.

---

## `aipex_releases`

Project releases and milestones.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED | Project post ID |
| `version` | VARCHAR(64) | Version number |
| `title` | TEXT | Release title |
| `status` | VARCHAR(32) | planned / building / testing / released / parked |
| `release_notes` | LONGTEXT NULL | Notes |
| `github_tag` | VARCHAR(191) NULL | GitHub tag |
| `released_at` | DATETIME NULL | Release date |
| `created_at` | DATETIME | Created date |
| `updated_at` | DATETIME | Updated date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY version (version),
KEY status (status),
KEY released_at (released_at)
```

---

## `aipex_audit_log`

General audit trail.

| Column | Type | Notes |
|---|---|---|
| `id` | BIGINT UNSIGNED AUTO_INCREMENT | Primary key |
| `project_id` | BIGINT UNSIGNED NULL | Related project |
| `object_type` | VARCHAR(64) | project / stage / artifact / decision etc |
| `object_id` | BIGINT UNSIGNED NULL | Related object ID |
| `action` | VARCHAR(64) | created / updated / superseded / deleted / generated |
| `actor_type` | VARCHAR(32) | user / ai / system / github |
| `actor_id` | VARCHAR(191) NULL | User ID, agent key or external ID |
| `summary` | TEXT | Human-readable summary |
| `meta` | LONGTEXT NULL | JSON data |
| `created_at` | DATETIME | Created date |

Indexes:

```sql
PRIMARY KEY (id),
KEY project_id (project_id),
KEY object_lookup (object_type, object_id),
KEY action (action),
KEY actor_type (actor_type),
KEY created_at (created_at)
```

## Protected Asset Store

Recommended MVP local structure:

```text
wp-content/uploads/aipex-protected/
  .htaccess
  index.php
  projects/
    <project-id>/
      assets/
      ai-runs/
      exports/
      private/
```

Protection requirements:

- Block direct public access.
- Serve files through permission-checked controller endpoints only.
- Log access to sensitive assets.
- Keep asset metadata in `aipex_assets`.

## Migration Strategy

Use a stored schema version option:

```text
aipex_client_os_db_version
```

Rules:

- Run migrations on plugin activation and version upgrade.
- Use `dbDelta()` for table creation/changes where appropriate.
- Keep migrations incremental.
- Never drop data automatically during upgrade.
- Use uninstall only for explicit full removal.

Suggested versions:

| Version | Change |
|---|---|
| 0.1.0 | Create MVP tables |
| 0.2.0 | Add GitHub event sync fields |
| 0.3.0 | Add release and review refinements |

## MVP Build Order

1. Register CPTs: Project, Artefact, Agent.
2. Create core custom tables.
3. Add project status storage.
4. Add decision log table and append-only write methods.
5. Add risk and task tables.
6. Add protected asset table and storage folder protection.
7. Add AI run table.
8. Add audit log helper.
9. Add GitHub repository link table.
10. Add admin list screens.

## Open Questions

| Question | Blocking? | Recommendation |
|---|---|---|
| Should GitHub events sync automatically in MVP? | No | Start with repository links and manual references. Add sync later. |
| Should full AI interaction records be encrypted at rest? | Yes | Decide during security architecture. |
| Should protected assets support external storage in MVP? | No | Start local; abstract driver names for later. |

## Definition of Done

- [x] CPTs defined
- [x] Custom tables defined
- [x] Columns and indexes defined
- [x] Protected asset store defined
- [x] Migration strategy defined
- [x] MVP build order defined
- [x] Open questions listed

## Next Action

Create `03-ux-structure.md` defining the dashboard, project screens, pipeline view, artefact viewer, decision log, asset library and agent console.