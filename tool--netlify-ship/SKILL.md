---
name: tool--netlify-ship
description: >
  Deploy an update to an existing Netlify project the safe way — preview first, then promote to
  production on confirmation — and log the version to the project's local CHANGELOG.md. Trigger it to
  DEPLOY changes: it finds the linked project folder, pushes a draft/preview deploy, shows the preview
  URL, promotes to prod once confirmed, verifies it went live, and appends a version entry. Handles
  first-time linking and reports failures verbatim. Built for the Netlify Launch folder convention.
  Activates on "ship it", "deploy this", "deploy the update", "push to netlify", "go live",
  "netlify ship", "/tool--netlify-ship".
classification: trusted-input
last_reviewed: 2026-07-28
---

# Netlify Ship

Deploys an update as **preview → confirm → promote → log.** No dragging, no surprises, a written
history in the project. Pairs with [[tool--netlify-new]] (which scaffolds the project first).

## When to use

- The operator changed a project under `$LAUNCH_DIR/<project>/` and says "ship it" / "go live".
- For a brand-new project (no site yet), use [[tool--netlify-new]] instead.

## Prerequisites (verify, fix if missing)

1. `netlify -v` — CLI installed. If missing, point to `Netlify Launch/_setup/Setup-Netlify.command`.
2. `netlify status` — logged in. If not, operator runs `netlify login` (their auth, never do it for them).
3. Read `LAUNCH_DIR` from `~/.netlify-launch.conf` (fallback: `~/_Archive - AI Projects/Netlify Launch`).

## The procedure

### 1. Determine the target folder

- If the operator names a project, use `$LAUNCH_DIR/<name>/`.
- Else use the project in current context. If ambiguous, list `$LAUNCH_DIR` subfolders and ask.
- Confirm it holds `index.html` (or ask the entry file).

### 2. Verify linked

- Check `<folder>/.netlify/state.json`. If present → linked.
- If absent → `netlify link` (operator searches by site name) OR hand off to [[tool--netlify-new]] if
  the site doesn't exist yet.

### 3. Preview deploy (draft)

```bash
cd "<folder>" && netlify deploy --dir .
```
Capture the **Draft/preview URL**. Show it to the operator: "Preview is up — check it, then I'll promote."

### 4. Promote (only on confirmation)

After the operator confirms the preview looks right:
```bash
netlify deploy --prod --dir .
```
Output MUST contain `Production deploy is live` + `Deployed to production URL:`. If not → FAILED;
report the exact error, do not claim success. Optional: `curl -s -o /dev/null -w "%{http_code}" <url>` == 200.

### 5. Log the version

Prepend a new entry to `<folder>/CHANGELOG.md` (create it if missing):
```
## v<n> — <YYYY-MM-DD HH:MM>
- <one line: what changed this deploy>
- Live: <production URL>
```
Increment `<n>` from the previous top entry. Ask the operator for the "what changed" line if not obvious.

### 6. Report

One line on success: production URL + "deployed in Ns, logged as v<n>". On failure: what broke + likely fix.

## Rollback (when a deploy breaks the site)

The CHANGELOG.md records history but does NOT store old code. The rollback mechanism is Netlify's
retained deploys:
- Dashboard: project → Deploys → pick the last good deploy → **Publish deploy**.
- Or CLI: `netlify api listSiteDeploys --data '{"site_id":"<id>"}'` then restore, or just re-run a
  previous known-good source. Tell the operator the dashboard route first — it is one click.

## Rules

- **Preview before prod, always.** Never skip step 3 straight to promote.
- **Verify after act.** Not done until step 4 confirms live.
- **Log every promote.** A production deploy that isn't in CHANGELOG.md is incomplete.
- **One site per folder.** If the linked site name looks wrong, stop and confirm before pushing.
- **Never log in for the operator.** `netlify login` is their action.
- **Idempotent.** Re-deploying unchanged files just re-publishes the same content — safe.

## Example

Operator (in a project folder, after editing `index.html`): **"ship it"**
→ Reads `LAUNCH_DIR`, confirms `.netlify/state.json` exists, runs `netlify deploy --dir .`, replies:
  "Preview up: `https://a1b2--my-site.netlify.app` — check it, then I'll promote."
Operator: **"looks good"**
→ `netlify deploy --prod --dir .` → confirms `Production deploy is live` → prepends to `CHANGELOG.md`:
  ```
  ## v2 — <YYYY-MM-DD HH:MM>
  - Tightened button spacing on mobile.
  - Live: https://my-site.netlify.app
  ```
  → reports: "Live at my-site.netlify.app — deployed in 3s, logged as v2."

## Boundaries

| Situation | Use instead |
|---|---|
| Scaffolding a new project / creating a site | [[tool--netlify-new]] |
| Installing the environment (Node, CLI, login) | `Netlify Launch/_setup/Setup-Netlify.command` |
| Custom domains, DNS, env vars, team settings | Netlify dashboard |

Does not cover non-static apps that need a real build step beyond `netlify deploy`.
