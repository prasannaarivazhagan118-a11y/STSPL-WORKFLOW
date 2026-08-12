[README.md](https://github.com/user-attachments/files/30997644/README.md)
# Ledger — Project Ops

A single-file HTML task/ledger tracker with optional GitHub sync.

## Publish it (GitHub Pages)

1. Create a new **GitHub repo** (e.g. `ledger`), public or private — private also works with Pages on paid GitHub plans; on the free plan Pages requires a public repo.
2. Add this repo's contents (`index.html`) to it — either push with git, or use "Add file → Upload files" in the GitHub web UI.
3. Go to **Settings → Pages**. Under "Build and deployment", set Source = `Deploy from a branch`, Branch = `main` (or whichever branch you pushed to), folder = `/ (root)`. Save.
4. GitHub gives you a URL like `https://<your-username>.github.io/<repo-name>/`. Share that link with colleagues — that's the whole app, no server needed.

Any time you push a change to `index.html`, the page updates automatically within a minute or two.

## Important: how data storage works here

The original file was built for a sandbox that provides a special `window.storage` API. That API doesn't exist in normal browsers, so it's been swapped for a **localStorage-based shim** (see the top of the `<script>` block) — this is what makes it work standalone on GitHub Pages.

This means:
- **Each person's data lives only in their own browser**, on their own device. If your colleague opens the same link on their laptop, they start with an empty ledger — nothing is shared automatically.
- Clearing browser data/cache, or switching browsers/devices, wipes that person's local ledger.
- This is fine for a personal daily tracker, but it is **not** a shared multi-user database on its own.

## The built-in GitHub sync button

The app has a "Sync" feature (gear icon → enter a GitHub token, owner, repo) that pushes the current state (projects, tasks, team, activity log) to a JSON file in a GitHub repo, plus a dated file under `logs/`. Use it as:
- **A backup/export** of your local data, and
- **A shared activity feed** — colleagues can look at the JSON files in the repo to see what others logged, even though the live task list isn't automatically pulled back in.

If you want true shared/live task state across colleagues (everyone sees the same open tasks, edits merge), the app needs a further change: load the latest synced JSON from GitHub on startup instead of only pushing to it. That's a reasonable next step but isn't done here — happy to build it if useful.

### About the GitHub token
The sync feature asks for a **Personal Access Token (PAT)**, which it stores in that browser's localStorage and sends directly to the GitHub API from the browser. Keep this in mind:
- Anyone using a shared/public computer with the token saved could read it via browser dev tools.
- Create a **fine-grained PAT** scoped to only the one repo, with only "Contents: read and write" permission — not a classic token with broad access.
- Each colleague should generate and use **their own token**, rather than sharing one.

## Local testing before you push
Just open `index.html` directly in a browser, or run a tiny local server:
```
python3 -m http.server 8000
```
then visit `http://localhost:8000`.
