# Brahms MIDI

A static, no-build web app that plays Brahms MIDI files in the browser
through gentle synth voices, with each note rendered as a soft glowing
light — built for winding down and falling asleep.

## How it works

- `index.html` is the entire app (HTML/CSS/JS, no build step, no server).
- It loads two libraries from a CDN at runtime: **Tone.js** (synth +
  scheduling) and **@tonejs/midi** (MIDI parsing).
- There is no upload button by design — pieces come only from this
  repository's `tracks/` folder, listed in `tracks/manifest.json` (GitHub
  Pages can't list a folder's contents on its own, so the manifest fills
  that gap).
- Theme, chosen synth, volume, loop, sleep timer, and last-played piece are
  all saved in the browser's local storage, so the app remembers your
  settings between visits.

## Adding a track

1. Drop your `.mid` file into `tracks/`.
2. Add its filename to `tracks/manifest.json`:
   ```json
   [
     "wiegenlied-lullaby.mid",
     "intermezzo-op118-no2.mid"
   ]
   ```
3. Commit and push. It appears in the "Piece" dropdown, named after its
   filename (underscores/dashes become spaces).

## Hosting on GitHub Pages

1. Push `index.html`, `site.webmanifest`, and the `tracks/` folder to a
   GitHub repo's `main` branch.
2. Go to **Settings → Pages**.
3. Set **Source** to "Deploy from a branch", branch `main`, folder
   `/ (root)`, then save.
4. Your app is live at `https://yourname.github.io/repo-name/`.

## Playing through screen lock

Once you tap play, playback is scheduled on the Web Audio clock, which
keeps running independently of the page's visible UI — this is what lets
it continue after the screen turns off. To make that as reliable as
possible:

- The app registers a **Media Session** (lock-screen title/artist and
  play/pause/stop controls) and starts a barely-audible keep-alive tone,
  both of which help mobile browsers treat the tab as actively playing
  audio rather than suspending it.
- For the best results, especially on iPhone, **add the page to your home
  screen** (Safari share sheet → "Add to Home Screen") and launch it from
  there rather than from a browser tab — standalone home-screen apps are
  given more leeway to keep audio running than an ordinary browser tab.

Background-audio behavior still varies by phone and OS version and isn't
fully guaranteed by any web app — there's no way around that from within
the browser.

## Customizing

- **Synth voices**: `SYNTH_PRESETS` in `index.html` — add, remove, or
  tweak the Tone.js instrument settings for each preset.
- **Light colors**: `pitchColor()` maps pitch class to hue.
- **Glow decay**: the `DECAY` constant in the canvas render loop.
- **Quote of the day**: the `QUOTES` array — one is chosen deterministically
  based on the day of the year.
