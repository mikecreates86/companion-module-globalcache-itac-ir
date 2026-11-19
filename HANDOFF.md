# HANDOFF.md

Running log for whichever tool works on this repo next. Newest entry first.
Each entry: date, tool, what changed, what is half-finished, what to do next, decisions made.

## 2026-11-19 · Claude Code
- **What changed:** added AGENTS.md (project notes), CLAUDE.md (points to these files) and this log, on `main` only.
- **Where things stand:** version 3.0.0 is released and is the front-page download. PR #38
  (the improvements submitted to Bitfocus on 2026-10-08) is open with no review yet, and still
  merges cleanly with Bitfocus's latest code. Mike closed the accidental PR #39.
- **Half-finished:** nothing in the code. Three unused branches remain (see AGENTS.md > Branches).
  Mike approved deleting them, but the tool's safety settings blocked it.
- **Next:** Mike answers the TODOs in AGENTS.md. Watch PR #38 for maintainer replies and respond
  politely. Any new fix or feature follows "Releasing a new version" in AGENTS.md.
- **Decisions:** these notes live on `main` only. Not on `reliability-overhaul`, because that
  branch is the public submission. Not yet on `releases`, because every push there publishes a
  release; add them there along with the next version bump.
