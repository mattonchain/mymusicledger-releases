# MyMusicLedger

A personal desktop app for macOS that turns your Apple Music library export into a browsable, searchable, exportable catalog — yours to keep offline, forever.

> **MyMusicLedger does not access, store, play, copy, or transmit any audio files or copyrighted music content.** It works exclusively with the metadata Apple exports from your own library — track titles, artist names, album names, playlist names, and related descriptive fields. No music files are ever touched.

---

## The Story

I am a lifelong music collector. For years I carefully built an Apple Music streaming library to complement my physical vinyl collection. The most valuable part of that library is nearly 200 hand-curated playlists representing years of attention and taste. At some point I realized how exposed that work was — it lives entirely inside Apple's ecosystem, and one wrong move (a cancelled subscription, a corrupted library, an Apple account issue) could wipe it out.

Apple provides a **File → Library → Export Library…** option that produces a `Library.xml` file. It contains everything: every track, every playlist, every piece of metadata. The problem is that the raw XML is nearly unusable — it's enormous, deeply nested, and not human-readable in any practical sense.

MyMusicLedger started as a simple XML parser to generate CSV extracts of my playlists. It grew from there into what it is today: a full desktop application that imports the XML, stores everything in a local SQLite database, and gives you a fast browsing and export experience that mirrors (and in some ways extends) what Apple Music itself shows you.

This is a personal app in every sense. It was built for a specific, unusual need. Maybe it has a feature or two that others find useful as well.

---

## What It Does

- **Imports** your `Library.xml` export and stores everything locally — tracks, albums, artists, genres, playlists, and playlist membership
- **Browses** your library: Artists, Albums, New Releases, Recently Added, Genres, Playlists — all with search and pagination
- **Surfaces tracks** that are no longer available for streaming via Apple Music (unavailable tracks), purchased tracks, pre-release tracks, and playlist-only tracks
- **Exports** your data to CSV: full track list, playlist list, playlist membership, and unavailable tracks
- **Archives** each imported `Library.xml` with a timestamp so you have a history of snapshots
- **Runs entirely offline** — no accounts, no telemetry, no internet required

---

## Requirements

- **macOS** (Apple Silicon or Intel)
- **Apple Music** (to generate a `Library.xml` export)

---

## Installation

1. Download the latest `.dmg` from [Releases](https://github.com/MattOnChain/MyMusicLedger-releases/releases)
   - `MyMusicLedger-x.x.x.dmg` — Apple Silicon (M-series Macs)
   - `MyMusicLedger-x.x.x-Intel.dmg` — Intel Macs
2. Open the `.dmg`. Review and agree to the Software License Agreement.
3. Drag **MyMusicLedger.app** to your **Applications** folder.
4. Open the app. macOS may ask you to confirm on first launch — click **Open**.

---

## Getting Started

### 1. Export your library from Apple Music

In the Music app: **File → Library → Export Library…**

Save the resulting `Library.xml` file somewhere accessible. For large libraries this file can be several hundred megabytes.

### 2. Import into MyMusicLedger

Open the app and go to **Import** in the left sidebar. Upload your `Library.xml`. The import typically takes a few seconds even for very large libraries (~45,000 tracks in under 2 seconds on Apple Silicon).

After import, a recap shows:
- Tracks inserted / skipped
- Albums derived
- Playlists imported (visible vs. hidden system playlists)
- Any broken playlist track references

### 3. Browse

Use the left sidebar to navigate:

| Section | What you see |
|---|---|
| **Home** | Summary stats: tracks, artists, albums, playlists, genres, unavailable |
| **Songs** | All tracks, searchable and paginated |
| **Artists** | All artists → click through to albums and tracks |
| **Albums** | All albums → click through to tracklist |
| **New Releases** | Albums with a release date in the current or previous calendar year |
| **Recently Added** | Albums added to your library in the past 60 days |
| **Genres** | All genres → click through to albums |
| **Playlists** | All playlists, grouped by folder → click through to playlist tracks |
| **Unavailable** | Tracks that are no longer streamable via Apple Music |

### 4. Export

The **Data** section of the sidebar provides four CSV downloads:

- **Tracks CSV** — one row per track, all metadata fields, plus `playlist_ids` and `playlist_names` columns
- **Playlists CSV** — one row per playlist
- **Playlist Tracks CSV** — one row per playlist-track link, with `playlist_index` preserved
- **Unavailable CSV** — same format as Tracks CSV, filtered to unavailable tracks only

Export Settings (Settings → Export) lets you exclude specific playlists from the playlist exports. Apple's system playlists (Library, Music, Downloaded, etc.) are excluded by default since they inflate the numbers without adding useful information.

---

## Track Indicators

Small inline icons appear next to track titles throughout the app:

| Icon | Meaning |
|---|---|
| 🛒 Green cart | **Purchased** — track was bought outright, not streamed |
| 👻 Violet ghost | **Playlist Only** — track exists only within a playlist context |
| 🕐 Orange clock | **Pre-Release** — track is in your library but not yet officially released |

---

## Unavailable Tracks

Tracks marked as unavailable are remote Apple Music tracks (`track_type = Remote`) with `apple_music = false`. This typically means Apple has removed the track from their catalog. The Unavailable page lets you search and review these tracks, and export them to CSV for reference.

---

## Data & Privacy

MyMusicLedger is entirely local. It stores data in:

```
~/Library/Application Support/MyMusicLedger/
```

This folder contains:
- `library.db` — the SQLite database with all your library data
- `archive/` — timestamped copies of each imported `Library.xml`
- `window-state.json` — saved window size and position

No personal data ever leaves your Mac. There are no accounts and no analytics. The only optional network request is a startup check for new versions (Settings → About), which can be disabled.

---

## Uninstalling

Drag **MyMusicLedger.app** from your Applications folder to the Trash. This removes the app itself.

The data folder at `~/Library/Application Support/MyMusicLedger/` is left behind — this is standard macOS behavior. It contains your library database and XML archives. If you want a complete clean uninstall, delete that folder manually after trashing the app.

[AppCleaner](https://freemacsoft.net/appcleaner/) can surface this folder automatically if you prefer a guided approach.

---

## Import Settings

Import Settings (Settings → Import) lets you exclude tracks by **kind** before import. By default, video files are excluded. You can adjust this if your library contains other kinds you want to filter out.

---

## Built With

ChatGPT · Claude Code · Electron · React · Vite · TypeScript · Fastify · SQLite (better-sqlite3) · Mantine · Tabler Icons · Nano Banana · Potrace

---

## License

[PolyForm Strict 1.0.0](LICENSE) — © 2026 MyMusicLedger

---

## Contact

hello@mymusicledger.com
