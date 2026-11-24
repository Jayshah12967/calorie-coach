# Calorie Coach – Android app (Capacitor)

Your existing web app, wrapped as a native Android app. All app code is in `www/index.html`.

## What was fixed

- **CSV download**: the script had a missing `});`, so the `btn-save-weight` handler swallowed every handler after it (Analyze, both CSV buttons, CSV modal, meal and exercise forms). It is now closed correctly.
- CSV export was rebuilt:
  - Browser: normal file download.
  - Android app: saves to the phone's **Documents** folder, and **Share** opens the Android share sheet (Drive, Sheets, WhatsApp, Gmail).
  - Proper CSV escaping and a UTF-8 BOM so Google Sheets and Excel open it cleanly.
- Added `manifest.json`, `sw.js` and icons, which your HTML already referenced but did not exist.
- Meal and workout reminders are now real Android notifications that fire even when the app is closed.

## Build the APK

Requires Node.js 18+, Android Studio (with JDK 17+) and an internet connection for the first run.

```bash
npm install
npx cap add android
npx cap sync android
npx cap open android
```

In Android Studio: **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
The APK is at `android/app/build/outputs/apk/debug/app-debug.apk`. Copy it to your phone and install it.

For Play Store, use **Build > Generate Signed Bundle / APK**.

### Custom app icon (optional)

```bash
npm install -D @capacitor/assets
npx capacitor-assets generate --android
```

This uses `resources/icon.png`.

### After editing `www/index.html`

```bash
npx cap sync android
```

then rebuild in Android Studio.

## Notes

- The app loads Tailwind and Chart.js from CDNs, so the first launch needs internet. To work fully offline, download `https://cdn.tailwindcss.com/3.4.17` and `chart.umd.min.js` into `www/` and point the two `<script src>` tags at them.
- Android 13+ asks for notification permission. Tap **Remind > Enable Browser Alerts** once to allow it and schedule your reminders.
- Data is stored on the phone (localStorage). Use the CSV export for backups.
- Web/PWA alternative: host the `www/` folder on GitHub Pages or Netlify and use pwabuilder.com.
