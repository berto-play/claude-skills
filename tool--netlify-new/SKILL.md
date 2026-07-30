---
name: tool--netlify-new
description: >
  Scaffold a brand-new static web project under the operator's Netlify Launch folder and put it live,
  following the one-folder-per-project convention. Trigger it to CREATE a project: it makes the folder,
  seeds a starter index.html and a per-project CHANGELOG.md, creates a new Netlify site, deploys a
  preview, promotes to production on confirmation, offers a clean site name, and records v1 in the
  changelog. Activates on "new netlify project", "scaffold a site", "start a new site",
  "create a netlify project", "/tool--netlify-new".
classification: trusted-input
type: capability
last_reviewed: 2026-07-28
---

# Netlify New

Stands up a new static project the well-lubricated way: **scaffold → preview → promote → log.**
Pairs with [[tool--netlify-ship]] (which handles every update after this).

## Prerequisites

Run the env bootstrap once first (`Netlify Launch/_setup/Setup-Netlify.command`). Then verify:
- `netlify status` succeeds (logged in).
- `~/.netlify-launch.conf` exists. Read `LAUNCH_DIR` from it — that is where projects live.
  If missing, default `LAUNCH_DIR` to `~/_Archive - AI Projects/Netlify Launch` and note it.

## The procedure

### 1. Get the project name

- Ask for a short kebab-case name (e.g. `habit-tracker`). Refuse spaces/caps — slugify if needed.
- Target folder: `$LAUNCH_DIR/<name>/`. If it already exists, stop and confirm (never clobber).

### 2. Scaffold

Create in the folder:
- `index.html` — a minimal, valid, mobile-first starter (viewport-fit=cover, apple-mobile-web-app-capable,
  dark-mode aware). If the operator already has content in mind, use it; otherwise a titled placeholder.
- `CHANGELOG.md` — the per-project version log. Seed it:
  ```
  # <name> — deploy log

  ## v1 — <YYYY-MM-DD HH:MM>
  - Initial scaffold.
  - Live: (set after first promote)
  ```

### 3. Create the site + preview deploy

From the folder:
```bash
cd "$LAUNCH_DIR/<name>"
netlify deploy --dir .        # DRAFT — prompts once to create & configure a new site
```
Choose "Create & configure a new site". Capture the **Draft/preview URL** from the output.

### 4. Offer a clean site name

The auto-generated site name is ugly (e.g. `verbose-otter-9f2c1a`). Offer to rename:
```bash
netlify sites:update --name <clean-name>     # -> <clean-name>.netlify.app
```
Confirm the name is free; if taken, suggest a variant.

### 5. Promote to production (on confirmation)

Show the operator the preview URL. Only after they confirm it looks right:
```bash
netlify deploy --prod --dir .
```
Verify output contains `Production deploy is live` + the production URL. If not, report the error verbatim — do not claim success.

### 6. Log it

Append the live URL into `CHANGELOG.md` v1 (replace the "(set after first promote)" line). Report:
the folder path, the production URL, and the one-command update flow ("say 'ship it' to deploy changes").

## Rules

- **Preview before prod, always.** Never fire step 5 before the operator has seen the preview.
- **Never clobber an existing folder or site.** Confirm on any collision.
- **One folder = one site.** Keep the convention exact so [[tool--netlify-ship]] can find and deploy it.
- **Verify after act.** A site isn't created until the production URL responds. No optimistic claims.
- **Slugify names.** Folders and site names stay lowercase-kebab.

## Example

Operator: **"new netlify project — a tip calculator"**
→ Confirms name `tip-calculator`. Creates `$LAUNCH_DIR/tip-calculator/` with a starter `index.html`
  and a seeded `CHANGELOG.md`.
→ `netlify deploy --dir .` → "Create & configure a new site" → captures preview URL.
→ Offers a clean name → `netlify sites:update --name tip-calc` → `tip-calc.netlify.app`.
→ Shows preview. Operator: **"looks good"**
→ `netlify deploy --prod --dir .` → confirms live → writes the live URL into `CHANGELOG.md` v1 →
  reports: "Live at tip-calc.netlify.app, folder `tip-calculator/`. Say 'ship it' to deploy changes."

## Boundaries

| Situation | Use instead |
|---|---|
| Deploying an update to an existing project | [[tool--netlify-ship]] |
| Installing the environment (Node, CLI, login) | `Netlify Launch/_setup/Setup-Netlify.command` |
| Custom domains, DNS, env vars, team settings | Netlify dashboard |

Does not cover non-static apps that need a real build step beyond `netlify deploy`.
