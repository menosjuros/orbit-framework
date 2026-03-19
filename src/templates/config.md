# Project Config Template

Template for `.orbti/config.md` - project-specific configuration and integrations.

**Purpose:** Store project settings and optional integration flags. Allows ORBTI to adapt behavior based on available tools.

---

## File Template

```markdown
# Project Config

**Project:** [project-name]
**Created:** [YYYY-MM-DD]

## Project Settings

```yaml
project:
  name: [project-name]
  version: [current-version or "0.0.0"]
```

## Integrations

Optional integrations that enhance ORBTI functionality.

### E2E Testing (Playwright)

```yaml
e2e:
  enabled: false
  base_url: http://localhost:3000
```

### SonarQube

Code quality scanning and static analysis.

```yaml
sonarqube:
  enabled: false
  project_key: [project-key]  # Must match sonar-project.properties
  server_url: http://localhost:9000  # Optional, for reference
```

**When enabled:**
- `/orbti:quality-gate` runs scans and updates CONCERNS.md
- Quality metrics inform planning decisions
- Issues feed into tech debt tracking

**Requirements:**
- SonarQube server running (local or cloud)
- `sonar-project.properties` in project root
- SonarQube MCP server configured

### E2E Testing (Playwright)

Browser-based end-to-end testing against acceptance criteria.

```yaml
e2e:
  enabled: false
  base_url: http://localhost:3000   # App URL for Playwright to navigate
```

**When enabled:**
- BUILD finalize auto-runs Playwright for ALL ACs (no `--e2e` flag needed)
- `checkpoint:human-verify` executes automatically — Playwright navigates, interacts, screenshots
- Human verification still runs after E2E, focused on UX/visual judgment only
- E2E failures are warnings (not blockers) — flakiness is real

**Requirements:**
- Playwright installed: `npx playwright --version`
- App must be running at `base_url` before BUILD starts
- If app is not running, BUILD presents `checkpoint:human-action` to start it

### Future Integrations

Reserved for future use:

```yaml
# linting:
#   enabled: false
#   config_file: .eslintrc

# testing:
#   enabled: false
#   coverage_threshold: 80
```

## Models

Model routing for ORBTI phases. See `~/.claude/orbti-framework/references/model-routing.md` for defaults.

```yaml
models:
  default: sonnet             # Fallback for any phase not listed
  overrides: {}               # Override per-phase: refine: opus, build: haiku, etc.
```

## Preferences

Optional user preferences for ORBTI behavior.

```yaml
preferences:
  auto_commit: false          # Auto-commit after successful tasks
  verbose_output: false       # Show detailed step output
  parallel_agents: false      # Allow parallel agent spawning (advanced)
```

---

*Config created: [timestamp]*
*Edit anytime - changes take effect on next command*
```

---

## Good Example

```markdown
# Project Config

**Project:** expense-tracker
**Created:** 2026-01-28

## Project Settings

```yaml
project:
  name: expense-tracker
  version: 0.2.0
```

## Integrations

### SonarQube

```yaml
sonarqube:
  enabled: true
  project_key: expense-tracker
  server_url: http://localhost:9000
```

### Future Integrations

```yaml
# linting:
#   enabled: false
```

## Preferences

```yaml
preferences:
  auto_commit: false
  verbose_output: false
  parallel_agents: false
```

---

*Config created: 2026-01-28*
```

---

## Guidelines

**What belongs in config.md:**
- Project identification (name, version)
- Integration toggles (enabled/disabled flags)
- Integration-specific settings (project keys, URLs)
- User preferences for ORBTI behavior

**What does NOT belong here:**
- Sensitive credentials (use environment variables)
- Build configuration (use native config files)
- Project requirements (that's PROJECT.md)
- Roadmap information (that's ROADMAP.md)

**When to create config.md:**
- During `/orbti:init` if user enables integrations
- Manually when adding integrations later
- Not required for basic ORBTI usage

**Git behavior:**
- Can be committed (no secrets)
- Or gitignored if preferences are personal
- Recommend: commit integration flags, gitignore preferences
