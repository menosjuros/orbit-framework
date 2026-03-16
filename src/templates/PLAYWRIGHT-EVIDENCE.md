# Test Evidence: {Plan Name}

**Date:** {timestamp}
**Runner:** Playwright {version}
**Spec:** `.orbit/phases/{phase-dir}/{plan}-e2e.spec.ts`
**Base URL:** {base_url}
**Source:** `.orbit/phases/{phase-dir}/{plan}-SUMMARY.md`

---

## Results

| Test | AC | Status | Video | Screenshot |
|------|----|--------|-------|------------|
| {test title} | AC-1 | ✅ PASS | [video](evidence/AC-1/video.webm) | [screenshot](evidence/{plan}-AC-1-pass.png) |
| {test title} | AC-2 | ❌ FAIL | [video](evidence/AC-2/video.webm) | [screenshot](evidence/{plan}-AC-2-fail.png) |

**Passed:** N  |  **Failed:** N  |  **Total:** N

---

## Failures

### {test name}

**AC:** AC-{N}
**Error:**
```
{playwright error message and stack trace}
```

**Screenshot:** `evidence/{plan}-AC-N-fail.png`
**Trace:** `evidence/{trace-file}.zip`
*(Open trace: `npx playwright show-trace evidence/{trace-file}.zip`)*

---

## Verdict

> {ALL PASS ✅ — Feature validated by automated tests.}
> {FAILURES FOUND ❌ — N issue(s) require attention. See UAT file.}

---

## Files Generated

- Spec: `.orbit/phases/{phase-dir}/{plan}-e2e.spec.ts`
- Evidence: `.orbit/phases/{phase-dir}/evidence/`
- HTML Report: `playwright-report/index.html`
- UAT Issues: `.orbit/phases/{phase-dir}/{plan}-UAT.md` *(if failures)*
