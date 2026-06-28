# Activity Search

A self-contained web app for searching, filtering, and editing operational
activities — with a workflow for pushing updates back to this repository
**through Claude**.

- **Live artifact:** https://claude.ai/code/artifact/9ad8ed29-61d3-42e7-99a6-419553d84b84
- **App source:** [`index.html`](./index.html) (single file, no build step, no dependencies)
- **Canonical data:** [`activities.json`](./activities.json)

## What it does

- Full-text search across activity name, description, owner, ID, and category.
- Filter by **category** and **status** (planned / active / blocked / done).
- Sort by due date, name, or status; live summary counts at the top.
- Add, edit, and delete activities. Edits persist locally in the browser
  (`localStorage`), so your working changes survive a page reload.
- **Export for Claude** — produces clean JSON matching `activities.json`.

## Updating the repository through Claude

An artifact runs under a strict content-security policy and cannot write to
GitHub directly. So the repository is updated *through Claude*, which acts as
the bridge:

1. Open the app and make your changes (add / edit / delete activities).
2. Click **Export for Claude** and **Copy JSON**.
3. Paste the JSON to Claude and ask it to update `activities.json`.
4. Claude commits the change and pushes it to the repository.

You can also just describe the change in plain language — e.g. *"mark the
payroll audit as blocked and add an onboarding task due next Friday"* — and
Claude will edit `activities.json` and `index.html`'s seed data, commit, and
push, no copy-paste required.

## Data shape

Each activity is one object in `activities.json`:

```json
{
  "id": "ACT-1042",
  "name": "Screen candidates for Warehouse Associate",
  "description": "First-pass review of 38 applicants…",
  "category": "Recruitment",
  "owner": "A. Rossi",
  "status": "active",
  "due": "2026-07-03"
}
```

`status` is one of `planned`, `active`, `blocked`, `done`. `due` is an
ISO `YYYY-MM-DD` date.

> Note: `index.html` embeds a copy of this data as its initial seed so the app
> works when opened directly from disk. When you change `activities.json`, keep
> the seed array in `index.html` in sync — Claude does this for you when it
> applies an update.

## Running locally

Open `index.html` in any browser, or serve the folder:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```
