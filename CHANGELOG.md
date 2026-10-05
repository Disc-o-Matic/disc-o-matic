# Changelog

What changed in each release of Disc-o-Matic. The image for each version is
`simplic17y/disc-o-matic:<version>`; `latest` is the newest.

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
