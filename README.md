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

## Changing the plan
Exercises are a short list near the top of the script in `index.html`. Each entry can take:
- `sets: 2` for two working sets with their own weight and reps (all three days use this)
- `step: 2.5` for 2.5 lb steps instead of 5 (cable exercises: Cable Chest Fly, Cable Lateral Raises,
  Cable Overhead Triceps Extensions, Pallof Press)
- `warmups: 1` for a single 75% warmup (the 2nd exercise of a muscle group:
  Cable Chest Fly, Overhead Triceps Extensions, Machine High Row, Leg Press)
- `unit`, `sizes`, `defaultWeight` for a fixed weight list such as kettlebells
  (`KETTLEBELLS` lists the bells your gym has; 22 kg is left out)
  `DUMBBELLS` is the dumbbell rack: 2.5 lb steps below 40 lbs, then 5 lb steps (used by
  Dumbbell Incline Chest Press and Dumbbell Bicep Curls)
- `defaultWeight` is also the starting weight for a new exercise (Dumbbell Incline Chest Press
  starts at 40 lbs per dumbbell, Cable Chest Fly at 20 lbs)

## Full-screen exercises
On Push, Pull and Legs each exercise fills the screen below the top bar, and one swipe
moves exactly one exercise. The height is worked out on the phone, not hard-coded:
the top bar is measured at startup and each card takes the rest of the screen, so it
adjusts to installed-app vs. browser, gesture vs. button navigation, and font size.
The sizing values (`--gap`, button size limits) are at the top of the `<style>` block.

## Rest timer
Every exercise has a **Rest 3:00** button at the bottom. It opens a full-screen countdown:
- −1:00 / +1:00 change the time left; the circle in the middle restarts from 3:00; pause is under the time.
- At 0:00 the phone buzzes and beeps, and the screen shows how long you've gone over.
- Close with the X or Android's back gesture. The screen stays on while the timer is open.
- The length is `REST_SECONDS` at the top of the script in `index.html`.

## Saving workouts and Google Sheets
After the last exercise of each day, swipe once more to the **Finish workout** screen and tap
**Save workout**. It stores the day on the phone (one entry per date and day; saving again
replaces it) and sends it to Google Sheets if connected. Without signal it waits and sends later.
Save before bumping weights for next time.

To connect a sheet, follow `google-sheets-setup/SETUP.md` (it uses the `Code.gs` script there).
That folder is not part of the app, so it doesn't need to be uploaded with it.

## Updating the app later
Edit `index.html`, bump the version in `sw.js` (e.g. `ppl-workout-v7` → `ppl-workout-v8`), and re-upload.
Phones will pick up the new version the next time they open the app with a connection.
