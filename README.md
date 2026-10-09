# Gym Log — setup guide

Your own iPhone app for the 5-day split: workouts, weights per set, cardio finisher, daily weigh-in, editable exercises, and Google Drive sync. It runs without Claude.

You need about 20 minutes once:

1. Put the app online with GitHub Pages (free)
2. Install it on your iPhone
3. Connect Google Drive (optional, recommended)
4. Move over anything you logged in the Claude version (optional)

---

## 1. Put the app online (GitHub Pages)

1. Go to https://github.com and create a free account (or sign in).
2. Click **+** (top right) → **New repository**.
   - Name: `gym-log`
   - Visibility: **Public** (GitHub Pages is free for public repos; only the app's code is public, never your workout data)
   - Click **Create repository**.
3. On the new repo page click **uploading an existing file**.
4. Unzip `gym-log.zip` on your computer and drag in **all files** from the folder:
   `index.html, config.js, manifest.webmanifest, sw.js, icon-192.png, icon-512.png, apple-touch-icon.png, README.md`
   Then click **Commit changes**.
5. In the repo go to **Settings → Pages**.
   - Source: **Deploy from a branch**
   - Branch: **main**, folder **/ (root)** → **Save**.
6. Wait 1–2 minutes and refresh. The page shows your link:
   `https://YOUR-USERNAME.github.io/gym-log/`

Open that link once on your computer to check it loads.

## 2. Install on your iPhone

1. Open **Safari** on the iPhone (must be Safari) and go to `https://YOUR-USERNAME.github.io/gym-log/`.
2. Tap **Share** (square with arrow) → **Add to Home Screen**.
3. Name: `Gym Log`, keep **Open as Web App** on → **Add**.

You now have a red weight-plate icon. It opens full screen, works offline in the gym, and saves everything on your iPhone.

## 3. Connect Google Drive

Google requires every app that touches Drive to have its own free "client ID". You create it once:

1. Go to https://console.cloud.google.com and sign in with the Google account you want to use.
2. Top bar → project picker → **New project** → name `Gym Log` → **Create**, then select it.
3. Search bar → **Google Drive API** → **Enable**.
4. Left menu → **Google Auth Platform** (older UI: *APIs & Services → OAuth consent screen*) → **Get started**:
   - App name: `Gym Log`, support email: your Gmail → Next
   - Audience: **External** → Next
   - Contact email: your Gmail → Next → agree → **Create**
5. **Audience** → **Test users** → **Add users** → your Gmail → Save.
   (Keep the app in *Testing*. Only you can sign in, which is exactly what you want.)
6. **Data Access** → **Add or remove scopes** → search `drive.file` → tick
   `.../auth/drive.file` → **Update** → **Save**.
   This lets the app see only the one file it creates, nothing else in your Drive.
7. **Clients** → **Create client** → Application type **Web application**, name `Gym Log iPhone`:
   - **Authorized JavaScript origins** → Add URI: `https://YOUR-USERNAME.github.io`
   - **Authorized redirect URIs** → Add URI: `https://YOUR-USERNAME.github.io/gym-log/`
     (with the trailing slash)
   - **Create**, then copy the **Client ID** (ends in `.apps.googleusercontent.com`).
8. Back on GitHub, open `config.js` → pencil icon (Edit) → paste the ID between the quotes:
   ```js
   googleClientId: "1234567890-abc123.apps.googleusercontent.com"
   ```
   → **Commit changes**. Wait about a minute.
9. On the iPhone open Gym Log → **Settings** tab → **Connect Google Drive**.
   Google will say *"Google hasn't verified this app"*. That's normal for your own app: tap **Continue**, allow access.

A file named `gym-log-data.json` appears in your Google Drive.

**How syncing works**
- Your data is always saved on the iPhone first, so nothing is lost offline.
- Google's sign-in lasts about an hour. While it lasts, every change syncs within a few seconds.
- After that, the top-right corner says **Tap to sync Drive**. Tap it once (usually after your workout); it signs in again quickly and syncs.
- If you ever reinstall the app or get a new phone, connect Drive and your whole log comes back.

## 4. Move data from the Claude tracker (optional)

1. In the Claude version: **Plan** tab → **Save backup to phone** → Save to Files.
2. In Gym Log: **Settings** → **Load backup file** → pick the file → **Replace**.

## Using the app

- **Today**: morning weigh-in at the top, then the day's exercises. Each set has a weight stepper (− / +), reps and a ✓. The hint under each exercise tells you to add weight or beat your reps, based on last time. Add an extra exercise for today only, or remove one for today only, at the bottom.
- **Plan**: tap **Edit** on any day to add, remove, reorder or rename exercises and change sets, rep range and weight step. Turn any day into a rest or training day.
- **Progress**: weight lost, a chart against a healthy pace, a daily table with 7-day averages, attendance grid, and strength progress.
- **Settings**: Google Drive, start date, start/goal weight, backup file, reset the program.

The app has no end date. It runs in 13-week blocks (Foundation, Build, Deload, Push) that repeat for as long as you train.

## Updating the app later

If Claude gives you a new `index.html`, upload it to the same GitHub repo (Add file → Upload files → Commit). Your iPhone picks it up the next time you open the app with internet. Your data isn't touched.

## Troubleshooting

- **"redirect_uri_mismatch"** when connecting Drive: the redirect URI in step 3.7 must be exactly `https://YOUR-USERNAME.github.io/gym-log/` with the trailing slash, and you must open the app from that same address.
- **"Access blocked: app not completed verification"**: add your Gmail as a test user (step 3.5).
- **"Drive API not enabled"**: do step 3.3 in the same project as the client ID.
- **Sign-in page doesn't return to the app**: use **Settings → Save backup file → Google Drive** as a manual backup, and tell Claude what happened so it can be fixed.
