# ICP & WebDev101 — Practice Platform

A single-file practice platform for first-year programming.

- **ICP** — 129 C++ questions across 17 modules, graded against **740 tiered test cases**, compiled by a real `g++`.
- **WebDev101** — 25 HTML/CSS/JS tasks with live preview and 71 automated checks that run entirely in the browser.

No build step, no server, no API keys. One HTML file.

---

## Publishing on GitHub Pages

1. Create a new repository on GitHub (public).
2. Upload the contents of this folder to the repository root.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**.
5. Pick branch **`main`** and folder **`/ (root)`**, then **Save**.
6. Wait about a minute. Your site is live at:

```
https://<your-username>.github.io/<repo-name>/
```

### Via command line

```bash
git init
git add .
git commit -m "Add practice platform"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Then set Settings → Pages → Deploy from branch → `main` / root.

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire application. GitHub Pages serves this automatically at the root URL. |
| `.nojekyll` | Tells GitHub to serve files as-is instead of running them through Jekyll. Keep it. |
| `README.md` | This file. Shown on the repository page, not on the site. |
| `DEPLOY.md` | Hosting notes, self-hosting the compiler, known limits. |
| `LICENSE` | MIT. |

**`index.html` must keep that exact name** — GitHub Pages looks for it by default. Renaming it means the site loads a directory listing or a 404 instead.

---

## After it goes live

Open the site and check the chip beside the code editor:

- Green **`g++ 10.2`** — a real compiler is grading. This is what you want.
- Amber **`simulated`** — the compiler service was unreachable and execution is being approximated. Still usable, but not authoritative.

Then solve one C++ question end to end and complete one WebDev task to confirm both halves work.

---

## Progress and backups

Progress is stored in the browser via `localStorage`, so it is tied to **one browser on one device**. Clearing browser data erases it, and it does not follow you to another machine.

The home screen has **Export progress** and **Import progress** for moving between devices, and **Reset everything** to start over. Export a backup before switching machines or clearing browser data.

---

## Updating the site

Edit or replace `index.html`, commit, and push. GitHub Pages redeploys in about a minute.

```bash
git add index.html
git commit -m "Update platform"
git push
```

If you do not see the change, do a hard refresh (**Ctrl+Shift+R**, or **Cmd+Shift+R** on Mac) — the browser caches the old file.

---

## Notes

- The site must be served over **https** (GitHub Pages does this automatically) for the compiler service to be reachable.
- Everything on the WebDev101 side works offline. The C++ compiler needs a connection.
- See `DEPLOY.md` for self-hosting the compiler if a whole class will use this at once.
