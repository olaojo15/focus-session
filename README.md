# Focus Session

A single-block focus tool: pick one project, name one outcome, set a timer for whatever
length suits the task, and park any stray thought that shows up instead of chasing it.
One HTML file, no build step, no framework — it syncs itself to a JSON file in your own
private GitHub repo, so the same log and parking lot follow you between your laptop and
your phone, and it installs to your phone's home screen like a native app.

## How it works

- **Set up a block** — a project tag (optional), an outcome ("what does done look
  like?", required to start), and any duration in minutes. Quick-set chips (25/50/90) are
  shortcuts only — the field stays freely editable.
- **Timer** — a burning-down ring and big MM:SS digits. Start, Pause/Resume, Reset (Reset
  while running asks you to confirm). Finishing plays a short chime, logs the block, and
  clears the outcome field so the next block starts fresh.
- **Parking lot** — capture a stray thought without switching tasks; tick it done or
  delete it later.
- **Today's log** — every block you've completed today, with a running total, clearable
  with confirmation.
- **Theme** — the moon/sun icon in the header switches between the dark "desk lamp"
  look and a light "paper" look; your choice is remembered on that device.
- The app is gated behind a passcode screen, and your data lives in a GitHub repo under
  your own account, read and written via the GitHub API using a personal access token
  scoped only to that one repo.

**Worth knowing:** GitHub Pages sites from a private (or public) repo are reachable by
anyone who has the exact URL — there's no built-in "only me" access control on free/Pro
plans. The passcode screen is a reasonable deterrent for a personal tool, not bank-grade
security. Don't put anything genuinely sensitive in it, and don't share the URL.

## One-time setup (about 10 minutes)

### 1. Create the repository

1. Go to [github.com/new](https://github.com/new).
2. Name it `focus-session` (or whatever you like).
3. Set it to **Private**.
4. Tick "Add a README file" (or not, it doesn't matter), then **Create repository**.

### 2. Upload the app

1. In the new repo, click **Add file → Upload files**.
2. Upload `index.html` from this folder to the **root** of the repo.
3. Commit directly to `main`.

*(You do not need to upload a data file — the app creates `data/focus.json`
automatically the first time you connect.)*

### 3. Turn on GitHub Pages

1. In the repo, go to **Settings → Pages**.
2. Under "Build and deployment", set **Source** to "Deploy from a branch".
3. Set **Branch** to `main` and folder to `/ (root)`, then **Save**.
4. GitHub will give you a URL like `https://<your-username>.github.io/focus-session/`.
   It can take a minute or two to go live the first time.

### 4. Create a personal access token

This lets the app read and write your `focus.json` file, without giving it access to
anything else in your GitHub account.

1. Go to [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new)
   (fine-grained tokens).
2. **Token name**: something like `focus-session-app`.
3. **Expiration**: your call — 90 days or a year is reasonable; you'll just need to
   generate a new one and re-enter it in Settings when it expires.
4. **Repository access**: "Only select repositories" → choose the repo you just created.
5. **Permissions → Repository permissions → Contents**: set to **Read and write**.
   Leave everything else as "No access".
6. Generate the token and **copy it now** — GitHub only shows it once.

### 5. First run

1. Open your GitHub Pages URL on your laptop.
2. You'll land on a setup screen. Fill in:
   - A passcode of your choosing (this device only — you'll set it again on your phone).
   - Your GitHub username.
   - The repository name (`focus-session`).
   - Branch: `main`.
   - Data file path: leave as `data/focus.json`.
   - The token you just copied.
3. Click **Save & connect**. The app creates `data/focus.json` in your repo and you're in.

### 6. Add your phone

1. Open the same URL on your phone's browser.
2. You'll hit the same setup screen — repeat step 5 with the *same* GitHub repo/token
   details (generate a second token if you'd rather keep them separate; either works).
3. Add the page to your home screen (Safari: Share → Add to Home Screen; Chrome: menu →
   Add to Home screen) for a native-app feel — full screen, own icon, no browser chrome.

From here, both devices read and write the same `data/focus.json` file, so a block you
log on your phone shows up on your laptop next time it syncs (on load, and whenever you
switch back to the tab/app).

### 7. Optional: a native backup alarm on iPhone (recommended)

The in-app chime is a web page sound — it can't fire while the phone is locked, and on
some iOS versions an idle tab's audio can be unreliable even when unlocked. For a chime
that's guaranteed to ring (it uses the same system alarm sound as the Clock app, plays
through the silent switch, and fires even while locked), set up a one-time Shortcut:

1. Open the **Shortcuts** app → **+** to create a new shortcut.
2. Name it exactly **`Focus Timer`** (the app links to it by this name).
3. Add one action: **Start Timer**.
4. Set its duration to use **Shortcut Input** (tap the duration field → look for
   "Shortcut Input" in the variable picker, or select it under the magic-variable menu)
   rather than typing a fixed number — the app passes the block's length in minutes.
5. Save it.

On iPhone, a **🔔 Also set a phone timer** link now appears under the Start/Reset
buttons. Tap it (in addition to Start) and it hands off to Shortcuts for a second to set
a real Clock timer for the same duration, then returns you to the app. It's a manual,
one-extra-tap step each time — iOS doesn't allow a web page to trigger this silently.

## Day to day

- Fill in a project (optional) and an outcome, set a duration, hit Start.
- Pause/Resume as needed; Reset asks for confirmation if a block is running.
- On completion you'll hear a chime and the block is logged automatically.
- Park stray thoughts in the parking lot instead of chasing them; tidy it up whenever.
- The small dot next to the theme/settings icons shows sync state (green = saved, amber =
  saving, red = couldn't reach GitHub — it'll keep your last synced copy and retry).
- Toggle light/dark with the moon/sun icon.

## Changing settings later

Click the gear icon to update the repo/branch/path, rotate your token, or change your
passcode on that device. "Forget this device" clears the passcode and GitHub connection
from that browser only — your data in GitHub is untouched.

## Running locally

Open `index.html` directly in a browser, or serve the folder with any static server
(e.g. `python3 -m http.server`) for local testing before you deploy.

## If something goes wrong

- **"GitHub PUT failed (401)"** — your token is missing, expired, or wasn't scoped to
  Contents: Read and write. Generate a new one and update it in Settings.
- **"GitHub PUT failed (404)"** — check the username/repo/branch in Settings match
  exactly (case-sensitive).
- **Page loads but nothing syncs** — check you're not on a network that blocks
  `api.github.com`; the app will keep working from its last local copy either way and
  retry automatically.
