# 3-Day PPL Workout (PWA)

Installable, offline-capable workout tracker for Android (Samsung Galaxy S24).

## Files
- `index.html` — the app
- `manifest.json` — tells Android this is an installable app (name, icon, colors)
- `sw.js` — service worker that caches the app so it opens with no signal
- `icons/` — home-screen icons

## Step 1: put it online (one time)
PWAs can only be installed from an `https://` address, so the folder needs to be hosted.
Either free option works; keep the files together in the same folder.

**Netlify Drop (fastest)**
1. Go to https://app.netlify.com/drop
2. Drag this whole folder onto the page.
3. Copy the `https://....netlify.app` URL it gives you. (Sign up for a free account to keep the site permanently.)

**GitHub Pages**
1. Create a new public repository and upload these files to its root (not inside a subfolder).
2. Settings → Pages → Source: "Deploy from a branch" → `main` / root → Save.
3. Your URL will be `https://<username>.github.io/<repo>/`.

## Step 2: install on the Galaxy S24
Open the URL on the phone, then:

- **Samsung Internet**: tap the ☰ menu → "Add page to" → "Home screen", or tap the install icon that appears in the address bar.
- **Chrome**: tap ⋮ → "Install app" (or "Add to Home screen").

An "Install app" button also appears at the top of the Info tab once the browser confirms the site qualifies.

## Updating the app later
Edit `index.html`, change `ppl-workout-v1` to `ppl-workout-v2` in `sw.js`, and re-upload.
Phones will pick up the new version the next time they open the app with a connection.
