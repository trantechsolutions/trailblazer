# Trailblazer

Offline GPS trail recorder. Records your route, plots it live, and exports GPX/KML
for import into Google Earth. Wrapped with Capacitor so the Android app can keep
recording location in the background — locked screen, other apps open, etc. —
which a plain browser tab can't do.

## Getting the app onto your phone

This repo builds the Android app automatically in the cloud, no computer or
Android Studio required.

1. Go to the **Actions** tab of this repo.
2. Open the latest **Build Android APK** run (or trigger one manually with
   "Run workflow" if none has run yet).
3. Once it finishes (a few minutes), scroll to **Artifacts** and download
   `trailblazer-debug-apk`. It's a zip containing `app-debug.apk`.
4. On your phone, open the downloaded `app-debug.apk` file. Android will ask
   to allow installs from this source the first time — allow it, then install.
5. Open the app. On first "Start," Android will prompt for location
   permission. Choose **"Allow all the time"** (not "only while using the
   app") — this is required for tracking to survive a locked screen or app
   switch. Android shows an extra confirmation step for this because it's a
   sensitive permission; that's expected.

## Project layout

- `www/index.html` — the entire app (UI + logic). Runs as a normal web page
  in a browser (screen must stay on), and automatically switches to
  background-capable tracking when running inside the installed app.
- `capacitor.config.json` — Capacitor app config (app id, web asset folder).
- `package.json` — dependencies: Capacitor core/android + the
  `@capacitor-community/background-geolocation` plugin (free, MIT licensed).
- `.github/workflows/build-apk.yml` — the cloud build. It generates the
  native `android/` project fresh on every run (not committed to the repo)
  and builds a debug APK.

## Making changes

Edit `www/index.html` (or add files under `www/`), commit, and push. The
next Actions run produces a new APK with your changes — no local Android
toolchain needed at any point.

## Known things to verify on first real install

- The background-geolocation plugin should merge its own required Android
  permissions (fine/background location, foreground service) automatically
  via manifest merging. If the location prompt doesn't include "Allow all
  the time" as an option on your device, that's the first place to check —
  open an issue or flag it and the manifest can be patched explicitly.
- App icon/splash screen are currently Capacitor defaults — cosmetic, can be
  customized later via `npx cap` icon tooling or by dropping assets in the
  generated `android/app/src/main/res` folders before the build step.
