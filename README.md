# Essay Listening Board

A private, single-page kanban board for listening to draft essays and leaving feedback. Live at
`index.html` — one self-contained file, no build step, no server needed.

## How it works

- Uses your phone's **built-in text-to-speech** (Web Speech API) to read each essay aloud —
  no audio files, no hosting cost, no server-side TTS needed.
- Three columns: **To Listen → Listened → Feedback Given**. Cards move themselves to "Listened"
  when playback finishes (or tap "Mark Listened" manually).
- Feedback is typed or dictated (tap the mic icon on your phone's keyboard) into a text box per
  essay, saved locally in the browser (`localStorage` — nothing leaves your phone).
- Once feedback is saved, a **"Copy Dispatch Prompt"** button builds a ready-to-paste prompt with
  the essay name and your feedback, so you can hand it straight to Claude mobile to apply.
- Currently loaded with the first 5 essays (posts 21, 22, 23, 24, 27 from
  `04-writeups/unlock-cross-game-essay/posts/` — the ones the critique file scored strongest).
  Add more by editing the `ESSAYS` array at the top of the `<script>` block in `index.html`.

## Important honest caveat: screen-off / driving

iOS Safari (and most mobile browsers) **pause or kill Web Speech API playback when the screen
locks or the tab backgrounds.** This is a browser sandboxing limit, not a bug in this page — there's
no reliable way around it with a plain webpage. Practically:

- This works well for **lunch breaks** (screen on, browser in focus) — the original use case.
- For **actual driving**, don't rely on the screen staying unlocked and in-app the whole time.
  Either treat this as a passenger-seat / parked-car tool, or if you want true screen-off
  background playback for the drive, that's what the podcast RSS feed idea (real mp3 files,
  played through your phone's native podcast app) solves properly — worth revisiting later if
  this pattern proves useful.

## Deploying so it's reachable off-wifi (GitHub Pages)

You need a GitHub account. This makes the site **public but unlisted** — anyone with the exact
URL could view it, but it won't be indexed or linked anywhere (`robots.txt`-equivalent meta tag
is already in `index.html`). Free GitHub Pages doesn't support real password-protection on
personal (non-Pro) accounts, so pick an obscure repo name if that matters to you.

1. Create a new **public** repo on github.com — e.g. `essay-listening-board-<random-suffix>`.
   Don't initialize it with a README (you already have one here).
2. From this folder, run:
   ```
   git init
   git add index.html README.md
   git commit -m "Essay listening board"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
3. On GitHub: repo → **Settings → Pages** → Source: "Deploy from a branch" → Branch: `main`,
   folder: `/ (root)` → Save.
4. Wait ~1 minute, then your site is live at:
   `https://<your-username>.github.io/<repo-name>/`
   This is reachable over cellular data, not just wifi — GitHub Pages is public internet hosting.
5. On your phone, open that URL in Safari/Chrome and use **"Add to Home Screen"** so it opens
   like an app.

## Adding more essays later

Each entry in the `ESSAYS` array needs `id`, `title`, `project`, and `body` (plain text, no
markdown — the TTS engine will read markdown syntax aloud literally). Fastest way: dispatch
Claude with *"Add posts [N, N, N] from 04-writeups/unlock-cross-game-essay/posts/ to the ESSAYS
array in AUDIO_REVIEW_SITE/index.html, stripped of markdown formatting."*
