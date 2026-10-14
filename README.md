# Daily Tracker

A responsive task & work-progress tracker for phone and PC, backed by Firebase (Auth + Firestore). Real-time sync across devices, works offline, installable to your home screen.

## Features
- **Today** — overdue, in-progress, due-today and completed-today sections, quick add, day streak
- **Tasks** — search + filter (Open / In progress / To do / Completed / All)
- **Completed** — finished work grouped by day
- **Progress** — 7-day completion chart, week-over-week change, completion rate, avg. time to finish, per-project breakdown
- Per-task **progress %** slider and a timestamped **work log** ("what I did today")
- Projects, priorities, due dates
- Google sign-in or email/password
- Offline cache: edits made offline sync when you reconnect

## Project structure
```
daily-tracker/
├── firebase.json
├── firestore.rules
├── firestore.indexes.json
└── public/
    ├── index.html          ← the whole app
    ├── manifest.webmanifest
    ├── icon.svg / icon-192.png / icon-512.png
```

## Setup (≈10 minutes)

### 1. Create the Firebase project
1. Go to https://console.firebase.google.com → **Add project**.
2. **Build → Authentication → Get started** → enable **Google** and **Email/Password**.
3. **Build → Firestore Database → Create database** → production mode → pick a region close to you (e.g. `asia-south1` Mumbai).
4. **Project settings (⚙) → Your apps → Web (`</>`)** → register an app → copy the `firebaseConfig` object.

### 2. Add your config
Either:
- paste it into `FIREBASE_CONFIG` near the top of the `<script>` in `public/index.html` (recommended for deployment), **or**
- open the app and paste it into the "Connect Firebase" screen (stored only in that browser).

### 3. Deploy rules + hosting
```bash
npm install -g firebase-tools
firebase login
cd daily-tracker
firebase use --add            # pick your project
firebase deploy               # deploys hosting + firestore rules
```
Your app is live at `https://<project-id>.web.app`. Open it on your phone and PC and sign in with the same account.

### Run locally
```bash
npx serve public              # or: firebase serve
```
Open `http://localhost:3000`. (`localhost` is an authorized domain by default. Don't open the HTML as a `file://` — Firebase Auth won't work there.)

## Install on phone
- **Android (Chrome):** menu ⋮ → *Add to Home screen / Install app*
- **iPhone (Safari):** Share → *Add to Home Screen*

## Data model
```
users/{uid}/tasks/{taskId}
  title, notes, project, priority ('high'|'med'|'low'),
  status ('todo'|'doing'|'done'), progress (0–100), dueDate ('YYYY-MM-DD'|null),
  log: [{ text, at (ms), progress }],
  createdAt, updatedAt, completedAt, progressBeforeDone
```
`firestore.rules` restricts every user to their own `users/{uid}` path and validates fields.

## Troubleshooting
| Message | Fix |
|---|---|
| "This domain isn't authorized" | Authentication → Settings → Authorized domains → add your domain |
| "Permission denied — deploy firestore.rules" | Run `firebase deploy --only firestore:rules` |
| "Sign-in method isn't enabled" | Enable Google / Email-Password in Authentication |
| Google popup blocked on mobile | The app falls back to redirect sign-in automatically |

Keyboard shortcut on PC: **N** = new task, **Esc** = close.
