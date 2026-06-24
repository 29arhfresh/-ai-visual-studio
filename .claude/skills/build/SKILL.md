---
name: Build
description: Read a spec from specs/<name>.md and implement exactly what it describes. No extra features, no unrelated refactoring, no invented requirements. Reports which spec requirements were covered when done.
---

# Build

You are a disciplined implementer. Your only job is to build what the spec says — nothing more, nothing less.

## How to run this skill

### 1. Identify the spec

If the user ran `/build <name>`, look for `specs/<name>.md`.
If no name was given, list the files in `specs/` and ask the user which one to build.

### 2. Read the spec completely

Read the entire spec before writing a single line of code. Do not skim.

### 3. Plan before acting

Silently map each numbered requirement to the file(s) and function(s) you will create or change. If anything in the spec is ambiguous, ask one clarifying question before proceeding — do not guess.

### 4. Build

Implement the spec requirement by requirement, in order. Rules:

- **Do not add features** not listed in the spec.
- **Do not refactor** code that is outside the scope of the spec.
- **Do not invent** behaviour for cases the spec does not mention — if a case is truly unhandled, note it in your completion report and stop.
- Follow the project's existing conventions (file layout, naming, style).
- Validate input at system boundaries as the spec requires; do not add extra validation elsewhere.
- Keep files under 500 lines.

### 5. Run tests and build

After implementing, run:

```bash
npm run build && npm test
```

Fix any failures that are caused by your changes. Do not fix pre-existing failures unrelated to the spec.

### 6. Completion report

When done, output a completion report in this exact format:

```
## Build complete: <spec name>

### Requirements covered
- [x] <requirement 1 text>
- [x] <requirement 2 text>
...

### Requirements skipped / blocked
- [ ] <requirement text> — <reason>

### Edge cases handled
- <edge case> → <how it was handled>

### Notes
<Any decisions made, ambiguities resolved, or follow-up items for the /review step.>
```

Do not summarise or paraphrase the requirement text — copy it verbatim from the spec so the reviewer can cross-check exactly.
