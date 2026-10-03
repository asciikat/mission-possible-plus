# Mission Possible Plus

Mission Possible, plus **The Hood**. Everything from the Mission Board to-do list
and its **Ezycal** calendar tab, with a new **Hood** tab: the GTA-style map of
your homies from [asciikat/homies](https://github.com/asciikat/homies).

Single page: `index.html`. The Ezycal tab shows the separate calendar at
`../calendar/`, and the Hood tab shows The Hood at `../homies/` (both in a frame,
loaded the first time you open the tab).

Tabs: **Today**, **Tomorrow**, **Jail**, **Ezycal** (the calendar), **Hood** and **Notes**.

## The Hood tab

The Hood lives in its own repo and site (`asciikat.github.io/homies/`). This tab
shows the live version, so any update to The Hood shows up here automatically.
Because both are on `asciikat.github.io`, your homies, missions and stash are the
same ones as in the standalone Hood on that device.

## Mission Possible Plus vs Mission Possible

Both apps live on `asciikat.github.io`, so on the same device they share the
same board (jobs, cash, notes) and, once you sign in, the same synced board.
Plus is Mission Possible with the extra tab. They install as two separate apps,
and Plus keeps its offline copy under its own name (`mpplus-*`) so the two never
clear each other's. The
**gear** at the top right opens Settings: switch any tab on or off (hiding a tab
never deletes what's in it), and look up the **pay rates & rules** and the **rank
ladder**. On a phone the tabs wrap onto a second row so none hide off the edge.
**Gangsta Credit** (5 wins = a heist) is the small panel under the job board. The **raccoon icon** next to it opens raccoon mode: one-tap 5, 10 and
15 minute timers plus a custom one, which fill the whole screen while they run
(Shrink tucks one into a corner chip; tap the icon to bring it back). **The Big
Score** is the slim gold bar under the banner; tap it to plan it, add prep steps or
cash it in.

Notes and jobs trade places: each note has **Today** and **Tomorrow** buttons that turn it
into a job (jobs are one line, 140 letters max), and a job's menu (tap its text) has
**Move to notes**. Every move has Undo.

Notes sync between devices and are included in Save/Merge backup (newest edit wins; a
deleted note stays deleted). Tab choices and the running timer stay on each device.


## Host free on GitHub Pages
1. Merge to `main`.
2. Repo **Settings → Pages → Deploy from a branch → `main` / root → Save**.
3. Open `https://asciikat.github.io/mission-possible-plus/`.

Without sync, your jobs and cash are saved only in each browser. **Save backup**
/ **Merge backup** at the bottom of the page combine two devices by hand: jobs
from both are kept, jobs finished or dropped on either device stay gone, and
cash, wins and heists keep the higher number.

## Install as an app
Open the site and tap **Install app** (bottom of the page, or the pink ⤓ icon at
the top when your browser offers it). Android Chrome and desktop Chrome/Edge
install it directly; on iPhone the button shows the Safari *Share → Add to Home
Screen* steps. Once installed it opens full-screen with its own icon and works
offline; sync catches up when you're back online.

## Turn on sync (phone ↔ desktop, free)
Sync uses Firebase's free plan: you sign in with Google on each device and
changes show up on the other one within a few seconds. About 10 minutes, once.

1. Go to <https://console.firebase.google.com>, click **Create a project**,
   name it (e.g. `mission-board`). Google Analytics can be switched off.
   The free "Spark" plan is all you need; no card required.
2. **Build → Authentication → Get started → Sign-in method → Google → Enable**,
   pick your email as support email, **Save**.
3. Still in Authentication: **Settings → Authorized domains → Add domain** →
   `asciikat.github.io` (your GitHub Pages address, without `/todo`).
4. **Build → Firestore Database → Create database**. Pick a location near you,
   choose **production mode**, **Create**.
5. In Firestore open the **Rules** tab, replace everything with this, then **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /boards/{uid} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```
   This makes each board readable and writable only by the Google account that owns it.
6. Click the **gear → Project settings → Your apps → Web (`</>`)**, register an
   app called `Mission Board` (no Firebase Hosting needed), and copy the
   `firebaseConfig = { ... }` values it shows.
7. On GitHub open `firebase-config.js` → **Edit** (pencil) → replace `null`
   with those values (see the example in the file) → **Commit changes** to `main`.
8. After a minute, reload the site on each device, scroll to the bottom and tap
   **Sign in to sync**. Sign in with the same Google account everywhere.

The values in `firebase-config.js` are meant to be public; the rules in step 5
are what keep your board private. Your to-dos are stored in your Firebase
project.

How sync combines changes: new jobs from either device are added; finished,
dropped or moved jobs disappear everywhere; the newest star change wins; cash,
wins and heists keep the higher number. Undoable actions (drop, reset stars,
call off a big score) wait about 6 seconds before syncing so Undo still works.

Optional: drop your own jingle next to `index.html` as
`Mission_passed_jingl_#2-1783759697834.mp3`; otherwise a built-in fanfare plays.
