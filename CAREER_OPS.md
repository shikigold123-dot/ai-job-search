# CareerOps companion

CareerOps is installed in `career-ops/` as a Git submodule. The integration is intentionally isolated: this repository's `/setup`, `/scrape`, and `/apply` workflows remain unchanged, while CareerOps keeps its own profile, scanner, tracker, reports, and generated documents.

## Why a submodule

- Both projects can be updated independently.
- Their agent instructions, skills, data formats, and trackers do not overwrite each other.
- The parent repository pins a known CareerOps revision.
- Personal CareerOps files remain governed by CareerOps' own `.gitignore` and data contract.

## Clone this repository

For a fresh clone, include submodules:

```bash
git clone --recurse-submodules <this-repository-url>
```

If the parent repository is already cloned:

```bash
git submodule update --init --recursive
```

## Install and verify

```bash
cd career-ops
npm install
node doctor.mjs --json
```

`npm install` installs Playwright's Chromium runtime, which CareerOps uses for browser extraction and PDF generation.

## First-time onboarding with Codex

Run Codex from the CareerOps directory so its own `AGENTS.md` and skills are in scope:

```bash
cd career-ops
codex
```

Then ask:

```text
Set up career-ops for me.
```

Onboarding creates the required local user layer:

- `cv.md`
- `config/profile.yml`
- `portals.yml`
- `modes/_profile.md`, `modes/_custom.md`, and `modes/_brief.md`

After onboarding, useful Codex prompts include:

```text
Run the career-ops scan mode.
Evaluate this job with the career-ops auto-pipeline: <URL>
Run the career-ops tracker mode and summarize the current statuses.
```

## Privacy and backups

CareerOps ignores its personal user-layer files in its own repository, so they are not included when the public parent fork is committed or pushed. They are therefore local-only by default. Back up the user layer separately to an encrypted location or a private repository; do not force-add it to this public fork.

The two systems do not automatically synchronize profiles or application trackers. Keep one system as the source of truth for active applications; CareerOps is recommended if you decide to use its full pipeline.

## Updates

CareerOps has its own update checker:

```bash
cd career-ops
node update-system.mjs check
```

After accepting an upstream CareerOps update, review it and commit the updated submodule pointer in the parent repository. Do not edit system-layer files inside the submodule for personal customization; place personalization in the user-layer files listed above.
