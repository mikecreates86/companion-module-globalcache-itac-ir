# AGENTS.md: Global Caché iTach IR module

## Working rules (read these first)
- Read HANDOFF.md first, update it last.
- Make small, focused commits with clear messages, and make sure the project builds before each commit.
- Do not change architecture, rename things, or add dependencies without asking Mike first.
- Mike is the project director, not the programmer. Explain decisions in plain language.

## What this is and who it's for
- A Bitfocus Companion module that controls IR devices (projectors, TVs, displays) through
  Global Caché iTach IR interfaces (IP2IR including PoE, and WF2IR Wi-Fi).
- It is Mike's improved version of the community module `bitfocus/companion-module-globalcache-itac-ir`
  (this repo is a fork of it). Used in live church production, so reliability comes first.
- Two goals, in order: (1) work reliably for Mike's setup; (2) share general improvements back to Bitfocus.
  Church-specific details (addresses, names, passwords) must never go into anything shared.

## Branches (important: the module code is NOT on main)
- `main`: the repo front page (README with the download link), these notes, and the README updater.
  Its module code is the old upstream 2.0.3. Do not develop the module here.
- `reliability-overhaul`: the version submitted to Bitfocus as PR #38. Module code only, no
  fork-only extras. Pushing here updates the public PR.
- `releases`: same code as `reliability-overhaul`, plus the release workflow and `release-notes/`.
  Every push here builds and publishes a release, so only push with a version bump.
- Leftover, unused: `test-builds` (closed PR #39), `cleanup-old-test-download`,
  `mikecreates86-latest-release-readme-template`. Mike approved deleting them; the tool's safety settings blocked it.

## Build, test, run
- No Xcode: this is a Node.js project. Needs Node 22 and Yarn 1 (`packageManager: yarn@1.22.22`).
- On `releases` or `reliability-overhaul`: `yarn install --frozen-lockfile`, then `yarn lint`,
  `yarn test` (34 tests, node:test) and `yarn package`, which writes
  `globalcache-itac-ir-<version>.tgz`, the file Companion imports.
- `yarn package` also leaves a `pkg/` folder. Delete it and never commit it.
- `main` still has upstream's 2.0.3 code, which expects Node 18. To check it builds on Node 22:
  `yarn install --frozen-lockfile --ignore-engines`, then `npx companion-module-build`.
- Run in Companion: Modules > Import module package > choose the .tgz, then pick that version
  in the Global Cache connection. TODO (Mike): which Companion version(s) do you run?

## Releasing a new version (what Mike downloads)
1. Make the change on `reliability-overhaul` if it's for everyone, then bring it into `releases`.
2. On `releases`, raise `version` in package.json (three-part number, higher than the last
   release) and add `release-notes/<version>.md` in plain language.
3. Push `releases`. `.github/workflows/release.yaml` lints, tests, refuses any version that is
   not higher than every published release, builds and checks the .tgz, and publishes
   `v<version>` marked Latest. Then `main`'s `update-latest-release.yml` updates the README version line.
- Never reuse or lower a version, never overwrite a release, never publish a "beta" number.

## Architecture in plain language
- `index.js`: the Companion connection. Starts the client and turns its events into status,
  variables, feedbacks and log messages.
- `src/itach.js`: the engine, with no Companion code in it. It keeps the network connection
  alive (reconnects with back-off, idle keepalive check) and sends IR codes one at a time per
  port, waiting for the device's confirmation (`completeir`) before sending the next.
- `src/protocol.js`: reads the device's replies (completeir, busyIR, ERR_xx, device list).
- `src/ircode.js`: cleans up and checks IR codes (Global Caché format, iLearn `sendir` lines,
  Pronto hex) before anything is sent.
- `src/actions.js`, `feedbacks.js`, `variables.js`, `presets.js`, `config.js`: what users see in Companion.
- `src/upgrades.js`: converts settings saved by older versions. Never remove or reorder entries.
- `test/fake-itach.js`: a pretend iTach the tests use to simulate drops, hangs and errors.

## Naming and code conventions
- CommonJS (`require`), tabs, no semicolons, single quotes, 120 columns (Prettier config from
  `@companion-module/tools`). Run `yarn format` before committing; `yarn lint` must pass.
- Methods are mixed into the instance class from the `src/*.js` files (pattern kept from upstream).
- Action and option ids are permanent: `portSet`, `portNum`, `ir`, `repeat`, `stopIR`,
  `clearQueue`, `reconnect`, `sendRaw`. Renaming them breaks users' saved buttons.

## Decisions already made, and why
- Stay on `@companion-module/base` 1.14 (not 2.x) so it still runs on older Companion installs.
- A code whose delivery is uncertain is NOT resent by default, because resending a power
  toggle would undo it. Users can turn resending on (`retryUncertain`).
- Codes not started within 3 s (`maxCommandAge`) are dropped, never fired late after an outage.
- Busy ports are retried (3 times by default). Device errors are reported, not retried.
- Defaults: keepalive check every 10 s while idle, 2 s response timeout, TCP port 4998, module address 1.
- Version 3.0.0, because saved option values changed (the upgrade script turns old port ids like `'1,'` into `'1'`).
- Releases are plain numbered versions marked Latest. The earlier "beta" test builds were retired.
- Test-build tooling is kept off `reliability-overhaul` so nothing beta-related reaches Bitfocus.
- Maintainer listed in the manifest: Mike Moore, with the email Mike chose.

## Do not change without asking Mike
- Architecture, file layout, action/option ids, default settings, dependencies or their versions.
- Anything on `reliability-overhaul` (it is the public PR), or anything else sent to Bitfocus.
- The release workflow's version check, or any published release.

## Current state (as of 2026-11-19)
- Working: version 3.0.0 is released and is the front-page download. Mike ran the 3.0.0 test
  builds on iTach IP2IR (PoE) and WF2IR units and reported it working well.
- Shared: PR #38 to Bitfocus, opened 2026-10-08, still open with no review. Bitfocus's later
  update (a dependency bump on 2026-10-30) still merges cleanly with it.
- Not tested on real hardware: GC-100 and iTach Flex (they should work by setting the module address).
- Half-built: nothing.
- Known bugs: none reported. TODO (Mike): anything odd seen in services since October?
- TODO (Mike): roughly how many iTach units and ports you run and what they control
  (kept here for context only, never in shared code).
- TODO (Mike): which features or fixes you want next.
