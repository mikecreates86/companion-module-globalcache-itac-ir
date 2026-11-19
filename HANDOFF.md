# HANDOFF.md

Running log for whichever tool works on this repo next. Newest entry first.
Each entry: date, tool, what changed, what is half-finished, what to do next, decisions made.

## 2026-11-19 · Claude Code (second entry)
- **What changed:** filled in AGENTS.md with Mike's answers: Companion 5.1.3, no problems in
  services since October, about 10 iTachs with mostly 1 port each. No code changes.
- **Half-finished:** nothing. The three unused branches are still there (harmless).
- **Next:** the project is done for now. Only act if (1) Bitfocus replies on PR #38: answer
  politely and make any requested changes on `reliability-overhaul`, then mirror them to
  `releases` with a version bump; or (2) Mike reports a problem. Optional, low priority:
  delete the unused branches; if Bitfocus asks, move to `@companion-module/base` 2.x.
- **Decisions:** no new features planned. 3.0.0 covers Mike's needs, and adding features now
  would only enlarge the open PR and make review harder.

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
