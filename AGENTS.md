# QA workspace

This folder runs [qa-feature-flow](https://github.com/ko-andreiev/qa-feature-flow), a QA method for one feature in six
phases (1 Discovery · 2 Business rules · 3 Test cases · 4 Pre-flight · 5 Live verification · 6 Triage), each ending at
a gate the human answers. The method lives in the skill; this file only routes. Product facts: `feature-flow.toml`.

## At session start: check the state, tell the human in one or two lines

1. No `feature-flow.toml` at the root → "This workspace is not set up yet" and offer the setup command (below).
2. Unfinished runs → list each and offer to continue it. Run journals are `{date}-{module}-{platform}.md` in `qa.runs`
   of `feature-flow.toml` (default `qa-work/feature-flow/runs/`); a run is unfinished when the journal's frontmatter
   `status:` is not `closed`. Per run: feature (`modules:`), platform (`platform:`), last phase reached (highest in
   `phases:`, checked against "Where we stopped" in §1 of the feature note under `qa.feature_notes`), and whether a gate
   waits for the human (the journal's last `<!-- GATE-OPEN … -->` has no `<!-- GATE-CLOSED … -->` after it).
   Read only the frontmatter, the gate markers and note §1.
3. Otherwise one line: ready; start a feature with the run command.

## Commands: the human types them, the skill never starts from a plain phrase

`{feature}` is a ticket key or a feature name; `{platform}` is `web`, `mobile` or `both`.

- Claude Code: `/qa-feature-flow:qa-feature-flow {feature} {platform}` · setup: `/qa-feature-flow:qa-feature-flow setup`
- Codex: `$qa-feature-flow {feature} {platform}`; Cursor: `/qa-feature-flow {feature} {platform}`; setup the same way
  with `setup`. Both need the skill installed first: `adapters/codex/README.md` or `adapters/cursor/README.md` in the
  package. Other agents: "Use in other agents" in the package README.

## What the human says → what you do

- "Test / analyse / QA feature X" → give the run command for X; no platform named → ask web, mobile or both first.
- "Continue", "next phase" → the run command of the unfinished run (several → ask which). The skill finds the phase
  from the files; do not work it out yourself.
- "Status", "where are we" → the summary from the session-start check.
- "Where are the bugs / the report" → Phase 6 writes its findings, bugs first, into §6 of the feature note and shows the
  report and each bug draft in chat at its gate; a ticket is filed only after the human's OK. Filed tickets: `bugs:` in
  the run journal's frontmatter. Test cases: `qa.test_cases`.
- "Set up", "change the config" → `setup`; "check the config", or after updating the method → `setup check`.

## Rules of this folder

- One phase per session. After a phase's closing line the next phase starts in a fresh session, not here.
- Only the human answers a gate. While a gate is open, stop and wait; never answer it or pick an option for them.
- `repos/` is read-only: read code, history and docs; never edit, commit, pull or push there.
- Run nothing (the product, builds, containers, package managers) unless the human asks; the skill's scripts are part
  of the method.
- Talk to the human in `project.language` of `feature-flow.toml`; before setup, in the language they write in.
