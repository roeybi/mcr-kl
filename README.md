# THE BLACK PARADE — My Chemical Romance Lyric Book

An offline-first lyric book for My Chemical Romance. Built for standing in a crowd
with 200,000 phones on the same overloaded network and no bars of signal.

**92 songs · 21 releases · 91 songs with lyrics · 88 songs with timed lyrics · ~1.4 MB total**

**Live:** <https://vkfibr0load96.space.mcode.io> · Built by Roey

---

## What it does

| | |
|---|---|
| **Offline** | The entire lyric pack is bundled and precached by a service worker. After the first load it works in airplane mode, on a dead network, on a plane. |
| **Blue light** | One tap fills the whole screen with pure `#0000FF` — for the moments when the crowd goes blue. Songs can be flagged as blue moments. |
| **Concert mode** | Fullscreen, screen-wake-lock, huge type. Tap the right side of the screen to advance a line, left side to go back. Or press play for automatic advance. |
| **Synced lyrics** | The app *listens to the song* and moves the cursor by itself — or you can time it by hand. |
| **Search** | Instant search across titles, releases, writers **and full lyric text** — matched lines are shown with the query highlighted. |
| **Setlist** | Build a running order. Stored on the device, survives reloads, nothing is uploaded. |
| **Installable** | A real PWA — add it to your home screen and it launches fullscreen with no browser chrome. |
| **Brightness pane** | One tap gives you a white fullscreen panel for when the venue is genuinely pitch black. |
| **Text sizing** | `A−` / `A+` on every song page, persisted. |

---

## Synced lyrics

Three ways to keep the cursor on the right line, from most automatic to most manual.

### 1. Timed (built in)

88 of the 92 songs ship with word-level timings (from [LRCLIB](https://lrclib.net)).
Open a song, tap **◷ Synced**, then enter concert mode and tap the **◷ clock** button
in the toolbar. The lyrics now advance on their own, word by word, with a seek bar
and a scrubber you can drag.

### 2. Listen (the mic)

Tap the **🎤 microphone** button and the phone listens to the room. It detects vocal
onsets, matches them against the song's timing grid and locks onto the right moment
— usually within about 15–20 seconds of listening, after which it follows the song
and corrects for drift.

- Works with the phone on the floor or in your hand. Put it somewhere that can hear
  the speaker.
- **If the lock lands wrong, tap the right line.** A manual tap is treated as
  ground truth and the lock realigns around it.
- The chip in the toolbar shows the state: `searching…` → `locked`.
- Nothing is uploaded. The audio never leaves the device — it is analysed locally.

### 3. Tap to sync (no mic needed)

On any song page tap **◷ Tap to sync**. Press **START** the instant the song begins,
then tap once every time a new line starts. You can stop early and save what you
have — the rest is filled in. The timing is stored **on your device only** and can be
reset to the official timing at any time.

---

## Blue light

The **blue button** in the top bar (also inside concert mode, and on every song page)
fills the entire screen with pure `#0000FF`. Tap once to bring up the exit bar, tap
again to hide it, or press `esc` to get out.

Three songs ship pre-flagged as **blue moments**, from the Kuala Lumpur blue-light
poster (@mcrxmalaysia):

| Day | Songs |
|---|---|
| Day 1 | *Disenchanted* · *Welcome to the Black Parade* |
| Day 2 | *I Don't Love You* |

Flag any other song with the **Blue moment** tag on its page. The **Blue light**
filter chip on the library shows only the flagged songs, and *Add blue moments* in
the setlist queues them all at once. Marks are stored on your own device.

---

## Running it

### Option A — open it directly (fastest)
Unzip and double-click `index.html`. It works straight from disk, no server needed.

### Option B — serve it locally (needed for "Add to Home Screen")
```bash
cd mcr-lyrics-app
python3 -m http.server 8080
# then open http://localhost:8080
```

### Option C — Netlify (recommended)

1. Go to **app.netlify.com/drop**.
2. Drag **`mcr-lyrics-netlify.zip`** onto the page. (Netlify unzips it and
   deploys the contents — `index.html` sits at the root of the archive.)
3. Wait ~30s. You get a URL like `https://your-site.netlify.app`.
4. Open it once on wifi and wait for the **OFFLINE READY** badge.

No build command, no publish directory to set, no dependencies — the folder is
already the built site. `netlify.toml`, `_headers` and `_redirects` are included
and pre-configured.

If you connect a Git repo instead, set **Publish directory = `.`** and leave the
build command empty.

### Option D — host it anywhere
The app is 100% static. Drop the folder on GitHub Pages, Netlify, Vercel,
Cloudflare Pages, or any static host. There is no build step and no backend.

---

## Installing on your phone

**Android / desktop Chrome** — open the app, then *Install this app* in the About tab
(or the browser's install icon in the address bar).

**iPhone / iPad (Safari)** — tap **Share → Add to Home Screen**. Launch it from the
home screen and it runs fullscreen with no Safari chrome.

> Do this **at home on wifi**. The first visit caches everything; after that the
> app never needs the network again.

---

## Fixes in the 18 November 2026 build

A bug report from a phone test drove a full pass over the app. What changed:

**Navigation & finding the words**
- "Back to library" now restores the whole library — the release view used to
  overwrite it permanently, leaving a dead page until a reload.
- Browser Back closes concert mode instead of leaving it running behind the
  library.
- The header search button works from the Songs page (it used to focus a hidden
  field).
- The A–Z rail actually scrolls to the letter you tapped.
- You can search by year — `2006` returns 19 songs.

**Data**
- *Sister to Sleep*, *The End.*, *The Black Parade Is Dead* and *Interlude*
  were carrying a neighbouring track's lyrics (the lyrics API fuzzy-matches).
  All four are corrected and the build now validates every lyric against its
  timing track so it cannot happen again.
- The 2002 debut is *I Brought You My Bullets, You Brought Me Your Love* and
  the 2004 album is *Three Cheers for Sweet Revenge* — they were swapped.

**Concert mode on a phone**
- The active lyric is no longer clipped at the right edge.
- The header clears the notch's safe area.
- The white brightness pane can be dismissed by tapping it.
- The transport row no longer sits under the host's floating badge.
- Blue light now goes fullscreen like concert mode does.

**From the 19 November retest**
- The microphone no longer opens behind your back. Asking for it and then
  closing concert mode used to leave the track live on the next screen; the
  capture that arrives late is now handed straight back and the app never
  claims to be listening.
- Reconnecting starts the download again. A failed install used to leave the
  badge stuck on "incomplete" until the page was reloaded — now the worker is
  installed again the moment the network comes back.
- A partial tap-to-sync save is no longer crammed: the lines you did not
  stamp get an even walk-through instead of all landing in the four seconds
  after your last tap, and the song page says "Synced · custom" straight away.
- Moving to an untimed song in a setlist stops the clock. The cursor used to
  walk on by itself while the hint still said "tap right to advance".

**Offline & privacy**
- The service worker installs all-or-nothing: one missing file means no cache
  at all, instead of a half-populated one.
- The header carries a **tick and a count** — `✓ 92` means all 92 songs are
  on the device and will open with no signal. The chip in the library filters
  the song list down to exactly those songs, every row carries the same tick,
  and a song page says whether that one is saved.
- The pack is pulled four files at a time. Some hosts drop the connection when
  a service worker asks for the whole pack in one burst, and a dropped burst
  used to leave the app with no offline pack at all.
- Each file gets three tries with a back-off, and a single request may not
  hang for longer than 20 seconds — a wedged host fails the install loudly
  instead of leaving the worker stuck.
- The badge only says "offline ready" when the pack really is whole; otherwise
  it says how many files are missing.
- A Content-Security-Policy blocks outbound connections, so the hosting
  platform's route-tracking beacon cannot fire. Nothing leaves the device.

**Synced lyrics**
- The microphone is released on every exit path — turning sync off, leaving
  concert mode, locking the phone, changing song.
- A denied permission can be retried; the app no longer pretends it is
  listening.
- A track that ends moves to the next song in the setlist instead of looping.
- The mic sampler no longer depends on the animation frame rate, so it keeps
  working when the browser throttles the render loop.
- A manual tap now sticks: the engine re-anchors to the line you chose.

---

## Files

```
index.html              app shell
app.js                  all logic (no framework, no dependencies)
styles.css              the whole design system
data/lyrics.js          the lyric pack (songs, releases, lyrics)
data/timings.js         word-level lyric timings for 88 songs
sync.js                 the sync engine: timing grid, mic onset matching, tap editor
fonts.css + fonts/      self-hosted woff2 (latin subsets, ~177 KB)
sw.js                   service worker: precache + stale-while-revalidate
manifest.webmanifest    PWA manifest
icons/                  app icons (192 / 512 / maskable / apple-touch / favicon)
```

Fonts are **self-hosted**, not pulled from a CDN — a CDN is exactly what will fail in a
crowded venue. All ten WOFF2 files ship inside the app and are precached by the service
worker, so they render identically offline. (They are technically web fonts via `@font-face`;
what matters is that they come from the same origin as the app, with no third-party request
and no external DNS lookup when the venue network is saturated.)

---

## Where the data came from

- **Tracklists and release metadata** — [MusicBrainz](https://musicbrainz.org)
  and [Wikipedia](https://en.wikipedia.org/wiki/List_of_songs_recorded_by_My_Chemical_Romance)
- **Lyrics** — [lyrics.ovh](https://lyrics.ovh)
- **Timed lyrics** — [LRCLIB](https://lrclib.net) (word-level where available)
- **Fonts** — Big Shoulders Display, Spectral, Space Mono (SIL Open Font License)

Covers MCR recorded are credited to the original writer and marked in the app
(*Astro Zombies* — Danzig, *Desolation Row* — Bob Dylan, *Under Pressure* —
Queen, *Song 2* — Blur, *Jack the Ripper* — The Smiths, *Common People* — Pulp,
*All I Want for Christmas Is You* — Mariah Carey, *Romance* — Shakespears Sister).

### Deliberately excluded

**The Paper Kingdom** — MCR's unreleased fifth album — is **not** included.
It was never released by the band; the only copies in circulation are leaked
demos, and the band has made clear the record was scrapped. The same applies
to the leaked songs from it (*Witch*, *War Beneath the Rain*, and the rest).

All lyrics are reproduced here for personal, non-commercial reference. The words
and music belong to their respective rights holders.

---

## Keyboard shortcuts (concert mode)

| Key | Action |
|---|---|
| `→` / `Space` | Next line |
| `←` | Previous line |
| `Esc` | Exit concert mode / blue light |

In synced mode `→` / `←` still move the cursor by hand, and each move re-anchors
the microphone lock to that line.

---

*Built for the front row.*
