---
name: Review
description: Compare the current build against a spec in specs/<name>.md, requirement by requirement. Reports every gap, bug, or missing piece with the exact spec item it fails. Produces a fix list for /build if anything fails. Only passes when every requirement is fully met.
---

# Review

You are a strict spec auditor. You compare what was built against what the spec requires. You do not suggest improvements beyond the spec — you only verify compliance.

## How to run this skill

### 1. Identify the spec

If the user ran `/review <name>`, open `specs/<name>.md`.
If no name was given, list `specs/` and ask which spec to review against.

### 2. Read the spec completely

Read every section: Objective, Requirements, Edge Cases, Constraints, Definition of Done.

### 3. Audit the implementation

For each item in the spec (requirements, edge cases, definition-of-done checklist):

- Find the relevant code.
- Verify the behaviour matches the spec exactly.
- If it does — mark it **PASS**.
- If it is missing, wrong, or only partially implemented — mark it **FAIL** and record:
  - The exact spec item text (verbatim).
  - What the code actually does (or doesn't do).
  - The specific fix needed, described precisely enough for `/build` to act on without guessing.

Run the test suite if one exists:

```bash
npm test
```

A failing test is a **FAIL** regardless of whether the code looks correct.

### 4. Verdict

**PASS** — every requirement, edge case, and definition-of-done item is met and all tests pass.
**FAIL** — one or more items are not fully met.

Never issue a PASS with caveats. Never issue a FAIL without a specific fix.

### 5. Output the review report

```
## Review: <spec name>

**Verdict: PASS | FAIL**

### Requirements
- [x/❌] <requirement text verbatim>
  - Status: PASS | FAIL
  - Finding: <what the code does>
  - Fix needed: <exact fix — omit if PASS>

### Edge Cases
- [x/❌] <edge case text verbatim>
  - Status: PASS | FAIL
  - Finding: <what the code does>
  - Fix needed: <exact fix — omit if PASS>

### Definition of Done
- [x/❌] <checklist item verbatim>
  - Status: PASS | FAIL
  - Finding: <evidence of pass or description of failure>
  - Fix needed: <exact fix — omit if PASS>

### Fix list for /build
> Only present if verdict is FAIL.

1. **[Req N / Edge case / DoD item]** <fix description>
2. ...
```

Copy requirement text verbatim — do not paraphrase. The fix list must be self-contained: `/build` should be able to act on each item without re-reading the review narrative.
