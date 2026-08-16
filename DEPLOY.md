# Deploying the ICP & WebDev101 practice platform

One file, no build step, no server, no API keys: `learning-platform.html`.

---

## Fastest route (2 minutes)

**Netlify Drop** — go to `app.netlify.com/drop` and drag the file in. You get a live URL immediately. Rename the file to `index.html` first so the URL is clean.

**GitHub Pages** — create a repo, add the file as `index.html`, then Settings → Pages → deploy from `main` branch. Live at `username.github.io/reponame`.

**Vercel** — `vercel deploy` in a folder containing the file as `index.html`.

**No hosting at all** — the file works opened directly from disk (`file://`). Everything runs except the compiler, which needs a network connection.

---

## The compiler

The app runs C++ through **Piston**, a free public execution service. Real `g++`, no API key, no signup. It asks Piston at runtime which C++ version is available rather than assuming one.

The chip beside the editor tells you which engine graded the work:

| Chip | Meaning |
|---|---|
| green **g++ 10.2** | A real compiler. Authoritative. |
| amber **simulated** | Piston was unreachable; execution was traced instead. Close, but not authoritative. |

If Piston fails, the app retries it after five minutes rather than giving up for the session.

### Rate limits

Piston's public endpoint allows roughly **5 requests per second**. One request is sent per test case, spaced 260ms apart. That is comfortable for one student and fine for a small group.

**For a whole class, self-host Piston:**

```bash
docker run -d --name piston -p 2000:2000 --privileged ghcr.io/engineer-man/piston
docker exec piston cli/index.js ppman install c++
```

Then change the two constants near the top of the execution section:

```js
const PISTON = "http://your-server:2000/api/v2/execute";
const PISTON_RUNTIMES = "http://your-server:2000/api/v2/runtimes";
```

Serve the page over **http** if Piston is on http, or put both behind https — a browser will block an http request from an https page.

---

## Where progress is stored

Inside Claude, progress goes to the account. **Deployed, it uses `localStorage`** — meaning it is tied to that browser on that device.

Consequences worth telling students:

- Clearing browser data erases progress.
- Progress does not follow them to another device.
- Private/incognito windows lose it on close.

The home screen has **Export progress** (downloads a JSON backup) and **Import progress** (restores it). Tell students to export before switching machines. **Reset everything** wipes local data behind two confirmations.

To make progress follow accounts across devices you would need a backend — out of scope for a single file, but the storage layer is isolated in one adapter object, so swapping `localStorage` for an API is a contained change.

---

## What's inside

- **ICP** — 129 C++ questions across 17 modules, 740 test cases tiered Sample → Basic → Boundary → Edge → Tricky → Stress. Every expected output was generated from a reference solution, and all 129 solvers ship in the app and are verified against all 740 cases.
- **WebDev101** — 25 HTML/CSS/JS tasks with live preview and 71 real DOM assertions. Runs entirely in the browser; no network needed.

---

## Known limits

1. **No syntax highlighting.** The editor does indentation, bracket matching and auto-close, but the text is not coloured. Adding CodeMirror would mean a CDN dependency and a larger file.
2. **No accounts or teacher view.** Single user, single browser.
3. **No worked solutions**, only on-demand hints.
4. **Piston is a third-party free service.** If it disappears, the traced fallback still works, or self-host per above.

---

## Before you go live

- [ ] Rename to `index.html`
- [ ] Open it and solve one C++ question end to end — confirm the chip is **green**
- [ ] Complete one WebDev task and confirm the checks tick
- [ ] Test on a phone; the panes become Question / Code / Result tabs
- [ ] Export a backup and re-import it
- [ ] Tell students that progress is per-browser and to export backups
