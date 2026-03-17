---
name: orbit:enable-e2e
description: Install Playwright CLI and enable E2E testing as default for /orbit:test
allowed-tools: [Read, Write, Bash, Edit, AskUserQuestion]
---

<objective>
Install @playwright/cli (the agent-optimized Playwright CLI) and configure ORBIT to use E2E testing by default in /orbit:test.

After running this command, /orbit:test will automatically run Playwright tests
against the project's acceptance criteria — no --e2e flag needed.
</objective>

<process>

<step name="check_existing">
Check if Playwright CLI is already installed:

```bash
playwright-cli --version 2>/dev/null || echo "NOT_INSTALLED"
```

If installed, show current version and ask if user wants to reinstall or just enable in config.
</step>

<step name="install">
If not installed:

```bash
npm install -g @playwright/cli@latest
```

Then install skills for agent integration (required for Claude Code workflows):

```bash
playwright-cli install --skills
```

Verify install succeeded:
```bash
playwright-cli --version
```

If install fails, show the error and stop — do not partially configure.
</step>

<step name="install_browsers">
Ask user which browser to use for E2E tests:

```
Playwright CLI installed. Which browser for E2E tests?

[1] Chromium (default, recommended)
[2] Firefox
[3] WebKit (Safari)
[4] All three
```

Install selected browser(s):
```bash
playwright-cli install chromium   # or firefox / webkit / --all
```
</step>

<step name="update_config">
Check if .orbit/config.md exists:
```bash
ls .orbit/config.md 2>/dev/null
```

If exists: add/update the e2e section.
If not: create config.md with e2e section.

Add to `.orbit/config.md`:

```yaml
## E2E Testing

```yaml
e2e:
  enabled: true
  runner: playwright-cli
  browser: chromium        # or firefox / webkit
  default: true            # /orbit:test uses e2e by default
  base_url: ""             # set your app URL here
```
```

If `default: true` is set, `/orbit:test` will run Playwright tests automatically
without requiring the --e2e flag.
</step>

<step name="confirm">
```
════════════════════════════════════════
E2E TESTING ENABLED
════════════════════════════════════════

Playwright CLI: [version]
Browser: [selected]
Default: yes — /orbit:test will run E2E automatically

Config saved to: .orbit/config.md

────────────────────────────────────────
▶ NEXT STEPS:

1. Set your app base URL in .orbit/config.md:
   base_url: "http://localhost:3000"

2. Run your first E2E test:
   /orbit:test

3. To disable E2E default and return to integration tests:
   Set e2e.default: false in .orbit/config.md
────────────────────────────────────────
```
</step>

</process>

<success_criteria>
- [ ] @playwright/cli installed globally
- [ ] playwright-cli install --skills completed
- [ ] Browser(s) installed
- [ ] .orbit/config.md updated with e2e section
- [ ] e2e.default set to true
- [ ] User knows how to set base_url
</success_criteria>

<anti_patterns>
- Don't install @playwright/test — this uses @playwright/cli (agent-optimized)
- Don't skip `playwright-cli install --skills` — required for agent integration
- Don't set default: true if install failed
</anti_patterns>
