# Changelog

What changed in each release of Disc-o-Matic. The image for each version is
`simplic17y/disc-o-matic:<version>`; `latest` is the newest.

## 0.5.55 (2026-10-06)

### New

- **DSD Discs**: an album of DSF (or DFF) files burnt to a DVD-R, DVD-RW, DVD+R or DVD+RW,
  for players that play DSD from a disc. Put a blank DVD in a DVD writer and pick the album
  (Burn a DSD Disc…); a DVD-RW with something on it can be written over (Write a DSD Disc
  over it…), at the drive's speed or one you choose. What a player takes varies: try one
  disc first (a Sony UBP-X700 plays them). DSD albums aren't offered for CD burning any
  more.
- **Notifications on your phone or in a chat**, also with no Disc-o-Matic tab open: when a
  job is done, when one fails, and when a disc needs you (the automatic job didn't start,
  and why). Settings → Notifications → *Phone & chat* takes Apprise addresses: ntfy (free,
  no account), Telegram, Discord, e-mail and many more, checked as you enter them; *Send
  test* tries them. How to set up ntfy or Telegram: the troubleshooting guide.
- **Plex and Jellyfin know at once.** Settings → Media servers: give Plex's address and
  token and/or Jellyfin's address and an API key, and when a rip finishes the server is
  asked to scan its libraries of that kind (a movie: its Movies, a CD: its Music …). Choose
  which kinds: only music, say. *Check* shows whether the server is reached and which
  libraries it has.
- **Catalogue**: a page listing every disc you've put in, with what it is (its match and
  poster), what was made of it (backup, MKV, music, ISO: where it went, and a warning if
  it's gone since) and when it was last seen. Search it, filter it by kind or by "something
  gone", and download it as a CSV. Posters come from the ones saved with your backups and
  rips; a disc without one gets its match's, fetched once and kept as a small thumbnail.
- **A password** for the web UI and the API, off until you set one in Settings → Access.
  Browsers sign in once (for 30 days), scripts send it with HTTP Basic auth
  (`curl -u :password`), and the health check stays open. Forgotten: start the container
  once with `DOM_AUTH__RESET=true`.

### Changed

- A job's log says where its notification went and which libraries Plex and Jellyfin were
  asked to scan, or why not.
- Opening the burn dialog pauses Aut-o-matic's countdown for that disc, as Fix match does.
- The layout switch is two icons now (a drive to a row, or two side by side) instead of a
  drop-down, and Blu-ray discs get a blue tag.

## 0.5.40 (2026-10-06)

### New

- **Remux a video file.** Open a backup → Browse folders now lists video files too: MKV,
  and MP4, AVI, M2TS and the like. Open one and it shows like a disc's title, with its audio
  and subtitle tracks; *Remux* writes an MKV with only the languages you pick (or the track
  profile's), named and filed like any MKV rip, and called a remux throughout (Remuxing…,
  MKV remux in the history). Nothing is converted, so it's quick and lossless. Uses
  MKVToolNix, now in the image.

### Changed

- The backups to open are kept in an index, so the list opens and searches at once; it is
  brought up to date in the background, and *Reindex* looks through the backup folders now.
  Each backup says what it holds and where it is.
- Browse folders shows what each entry is: a folder, a movie's folder, a video file, a disc
  backup or image, an album, with a line on what's inside.
- Without a TMDB key, a movie or TV disc's card says so as an orange warning, with a link
  straight to the key's setting.

### Fixed

- A widescreen picture cropped in height counts by its width: a 1280×546 file is 720p (and
  named so), not "546p".
- A card could stay on "Ripping… 100 %" after the job had finished when two updates came at
  once; the newest one now always wins.

## 0.5.38 (2026-10-05)

### Changed

- Aut-o-matic waits for the disc to be looked up before its countdown starts, so the rip or
  backup is named after what was found, not the disc's label. It waits a minute at most.
- Changing what a disc is (movie, TV, concert) while Aut-o-matic waits pauses it: it starts
  when you say.
- While Aut-o-matic counts down, its button is now *Pause*: it waits for you to start it.
- The Movie / TV / Concert switch takes effect at once; the lookup as that kind follows, and
  the switch waits until it's done.
- The line under a disc's title keeps its height while it's looked up, so the card doesn't
  jump (a TV disc's season and episodes now sit on that line too).
- A tidier titles table in four columns: the title (its number, resolution and codec,
  playlist), its length (with size and chapters), audio with a line per track, and subtitle
  languages once each (with how many tracks, and F for forced ones). Nothing breaks mid-word
  any more in a narrow card.

## 0.5.37 (2026-10-04)

### New

- **Open a backup or burn an album from any folder.** Both pickers have a *Browse folders*
  switch that walks through every folder mapped into the container, not only Disc-o-Matic's
  own. Each entry says what it is: a disc backup or a disc image (open it to rip MKVs), an
  album (pick it to burn) or just a folder. A loose `.iso` file can be opened directly.

## 0.5.36 (2026-10-04)

### New

- **Backup and restore** in Settings → Housekeeping. *Settings* downloads everything you set
  in Settings, each drive's own settings and the track profiles as a small file; restoring it
  applies straight away. *Everything* downloads the whole database, history included;
  restoring it takes effect when the container restarts, and the database it replaces is
  kept next to it. Handy for moving to another server or going back to an earlier state.

## 0.5.35 (2026-10-04)

### New

- The burn dialog has a speed choice for each burn (4× to 48×, or the drive's choice). It
  starts at Settings → Ripping → Burning speed.
- The album picker for burning keeps an index of your music folder, so searching is instant
  instead of going through every folder again. It's built once, then only folders that
  changed are read again. If you retag files in place, Reindex in the picker reads everything
  afresh.

### Changed

- The burn dialog opens straight away and shows a loader while albums load or a search
  runs. Albums show with their cover, artist and track count, in the same style as the
  match picker.
- The Unraid template and the README now give a simpler way to find a drive's SCSI generic
  device: `ls /sys/block/sr0/device/scsi_generic` prints it (`sr1` for a second drive). The
  old command came out garbled in Community Applications, and it listed every drive anyway.

### Fixed

- A note like "Auto-rip skipped: ripped before" no longer stays on the card after the disc
  is taken out.

## 0.5.34 (2026-10-03)

### New

- **Storage exceptions.** Settings → Storage → Exceptions gives any mix of what's on a disc
  (movie, TV, concert) and disc type (UHD, Blu-ray, DVD) a folder of its own, separately for
  full backups and MKV rips. A concert DVD's backup can sit with your concerts, UHD rips can
  go apart from the 1080p ones, and so on. Anything left empty goes where it always did.

### Changed

- **All MKV rips go to one folder** (MKV) unless an exception says otherwise; the separate
  TV and Concerts folders are gone. If you had changed them, they carry over as exceptions on
  their own. If you left them at their defaults, new TV and concert rips now land in
  `/backups/MKV`: add `/backups/TV` and `/backups/Concerts` under Exceptions to keep them
  apart. Rips already made stay where they are.
- The MakeMKV data import is gone from Settings → MakeMKV. MakeMKV fetches what it needs by
  itself; the overview of what it has stays.

### Fixed

- A blank CD-R no longer goes through MakeMKV first (which filled the log with read errors):
  it's recognised as a blank straight away.
- A blank CD-R in a drive that doesn't report how much fits showed "Nothing on it" and could
  not be burnt to. It now offers Burn an album as usual.
- An empty DVD or BD no longer shows a Cancel button with nothing to cancel.

## 0.5.33 (2026-10-03)

Disc-o-Matic is now in Unraid's Community Applications: search for it there.

### Changed

- MakeMKV leaves your other drives alone. It used to probe every drive whenever it started,
  which could get in the way of a rip running in another drive. A backup that can't be
  opened like that is retried the old way.

### Fixed

- A disc MakeMKV can't open at the first attempt (a UHD still busy spinning up, say) was
  sometimes taken for a data disc. It is now asked about once more first.

## 0.5.31 (2026-10-03)

### Changed

- The MakeMKV key lives in Settings → MakeMKV only. It is BETA by default and kept up to date
  for you; the `MAKEMKV_KEY` variable is no longer used. If you had set your own key there,
  enter it in Settings once.

## 0.5.30 (2026-10-03)

### Changed

- The web UI is on port 8099 on the host in the Unraid template and the examples (still 8088
  inside the container, so existing setups keep working).
- A new README and screenshots.

### Fixed

- A blank CD-R that reads as a 2 KB disc is a blank again, not a data disc. A disc with next
  to nothing on it otherwise shows as an empty disc: no ISO, no automatic job.

## 0.5.29 (2026-10-03)

The first public release.
