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
- `machines: [...]` adds a machine picker to the card (see Machines below)
- `step: 2.5` for 2.5 lb steps instead of 5
- `warmups: 1` for a single 75% warmup (the 2nd exercise of a muscle group:
  Cable Chest Fly, Overhead Triceps Extensions, Machine High Row, Leg Press)
- `unit`, `sizes`, `defaultWeight` for a fixed weight list such as kettlebells
  (`KETTLEBELLS` lists the bells your gym has; 22 kg is left out)
  `DUMBBELLS` is the dumbbell rack: 2.5 lb steps below 40 lbs, then 5 lb steps (used by
  Dumbbell Incline Chest Press and Dumbbell Bicep Curls)
- `defaultWeight` is also the starting weight for a new exercise (Dumbbell Incline Chest Press
  starts at 40 lbs per dumbbell, Cable Chest Fly at 20 lbs, Cable Reverse Fly at 15 lbs)

## Machines
`MACHINES` near the top of the script lists the machines at your gym. The cable exercises
(Cable Chest Fly, Cable Lateral Raises, Cable Overhead Triceps Extensions, Cable Reverse Fly,
Pallof Press) pick from the three cable machines; Machine Triceps Pushdown picks Triceps Press
or Triceps Extension. Tap the machine name at the top of the card to switch.
- Freemotion Dual Cable Cross: 3, 7, 10 ... 80 lbs, as printed on the stack
- Dual Adjustable Pulley: 2.5 to 100 lbs in 2.5 lb steps (5 lb plates + 2.5 lb slider);
  weights that need the slider show e.g. "47.5 + slider" under the number
- Adjustable Cable Crossover: 5 ... 35 in 5s, then 42.5 ... 95 in 7.5s (no slider)
- Triceps Press, Triceps Extension: 5 lb steps
Weight and reps are remembered per machine for each exercise. The machine is saved with every
set and goes to the Machine column in Google Sheets.

## Full-screen exercises
On Push, Pull and Legs each exercise fills the screen below the top bar, and one swipe
moves exactly one exercise. The height is worked out on the phone, not hard-coded:
the top bar is measured at startup and each card takes the rest of the screen, so it
adjusts to installed-app vs. browser, gesture vs. button navigation, and font size.
The sizing values (`--gap`, button size limits) are at the top of the `<style>` block.

## Rest timer
Every exercise has a **Rest 3:00** button at the bottom. It opens a full-screen countdown:
- −1:00 / +1:00 change the time left; the circle in the middle restarts from 3:00; pause is under the time.
- The down-arrow (or Android's back gesture) minimizes it so you can log your set or look at the next
  exercise. Every Rest button then shows the countdown; tap one to bring the timer back. After time's
  up, tapping Rest starts a new 3:00. The X stops the timer.
- At 0:00 the phone buzzes and beeps, and the screen shows how long you've gone over.
- The screen stays on while the timer runs, minimized or not.
- The length is `REST_SECONDS` at the top of the script in `index.html`.

## Heart rate recording and workout summary
Tap the heart at the right of the top bar and pick your Polar strap (any standard Bluetooth
heart rate strap works). While recording, the top bar shows your heart rate and the heart takes
your current zone's color (grey below Zone 1), beating at your heart rate. It blinks with "--" if the signal drops, and keeps
reconnecting by itself. Turn the number off on the Info tab (Show live heart rate in the top bar).
- The screen stays on while recording, because phones pause Bluetooth for web apps when the
  screen is off.
- Readings are saved on the phone every few seconds; if Android closes the app, what was
  recorded is kept. Tap the heart again to carry on (Android makes you pick the strap again).
- **Save workout** stops recording and opens the summary image: average and max heart rate,
  the zone chart, duration and time in each zone. Share it or save it to the phone.
  **Summary** on the Finish screen reopens it.
- Zones are set on the Info tab (they start as 84 / 102 / 119 / 135 / 152 from your Polar).
- The chart has a grey band under Zone 1 so recovery dips between sets show; its bottom is the
  workout's lowest reading. Time there is the "Below" row in Time in zones.
- The font in the summary is Barlow Condensed (SIL Open Font License, `fonts/OFL.txt`).

## Saving workouts and Google Sheets
After the last exercise of each day, swipe once more to the **Finish workout** screen and tap
**Save workout**. It stores the day on the phone (one entry per date and day; saving again
replaces it) and sends it to Google Sheets if connected. Without signal it waits and sends later.
Save before bumping weights for next time.

To connect a sheet, follow `google-sheets-setup/SETUP.md` (it uses the `Code.gs` script there).
That folder is not part of the app, so it doesn't need to be uploaded with it.
With the v8 script, workouts with heart rate also get a row in a **Heart Rate** tab (duration,
average, max, time in each zone). The older script keeps working; it just skips heart rate.

## Updating the app later
Edit `index.html`, then bump the version in two places: the `Version 9.0` line at the top of the Info
tab in `index.html` and `CACHE` in `sw.js` (e.g. `ppl-workout-v9` → `ppl-workout-v9.1`).
Re-upload.
Phones will pick up the new version the next time they open the app with a connection.
