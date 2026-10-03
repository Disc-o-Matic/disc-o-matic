# Troubleshooting

The console's **setup checklist** (also under Settings → System) checks most of what follows
and says what to do. Every job's full log is in History; DOM's own notes are in it too.

## The drive

**"No drive at /dev/sr0"**: the drive isn't passed into the container. Unraid: the
template's *Optical drive* device. Elsewhere: `--device /dev/sr0`. On the host,
`ls -l /dev/sr*` shows it; a USB drive appears only while it is plugged in.

**"/dev/sr0 is there but cannot be opened"**: the app runs as `PUID:PGID` and gets the
groups of the drive's devices at start. If the device's group changed (or it was plugged in
after the container started), restart the container.

**The drive is found but MakeMKV sees no disc, or "drive control" fails in the checklist**:
MakeMKV talks to the drive through its *SCSI generic* device, a second device to pass in.
Find it on the host:

```sh
for d in /sys/class/scsi_generic/*; do [ "$(cat $d/device/type)" = 5 ] && echo /dev/${d##*/}; done
```

The number can change when disks or USB devices are added: check it again after such a
change.

## MakeMKV

**"The volume key is unknown for this disc"**: MakeMKV has no key for this disc yet. It
sent the disc's data for analysis; keys usually follow within 12–48 hours, try again then.
Another MakeMKV install that decrypts the disc can share its data: Settings → MakeMKV data
→ import its `_private_data.tar`.

**Key problems** (expired, rejected, the forum unreachable): the System panel and the
checklist say so. `MAKEMKV_KEY=BETA` fetches the free beta key and renews it when it
rotates; a registration key is used as given.

**UHD discs fail but Blu-rays work**: UHD needs the drive in LibreDrive mode; the System
panel shows what MakeMKV reports. Many drives get it with a firmware change; see the MakeMKV
forum's list of UHD-friendly drives.

**"MakeMKV crashed (SIGSEGV) during “Processing titles”" opening a backup**: a bug in
MakeMKV 2.0.0, which dies opening any Blu-ray folder. DOM bundles 1.18.4 since 0.4.42;
update the container.

**A scratched video disc**: raise *Read retries* in Settings → MakeMKV (how often MakeMKV
reads a bad sector again), clean the disc, try again.

## Files and folders

**Other users can't rename or delete the rips** (SMB): files are created with `UMASK`
(000 by default: everyone may change them). Settings → System shows the owner and mode of
new files; set `PUID`, `PGID` and `UMASK` in the template.

**A folder in Settings → Storage is refused** ("not on a folder mapped into the
container"): the container can only write where a host folder is mapped in. Add the
mapping first (Unraid: *Add another Path*, e.g. `/mnt/user/Music` → `/music`), restart,
then enter the container path (`/music`).

**A folder in the checklist: "the host folder mapped here is gone"**: the host folder behind
the container path was deleted. On Unraid that is the mover, when the template maps a
`/mnt/cache/...` path: it moves the folder to the array and removes it from the cache. Map a
`/mnt/user/...` path instead (the mover never breaks those) and restart the container.

**Jobs fail with `move_failed`**: finished output is moved into place from
`<folder>/.dom-partial/`. Across disks of an Unraid user share it is copied, then removed (no
longer an error since 0.4.63); any other failure is reported with its reason. The output is
kept: History → Leftovers.

**Leftovers**: partial output of failed or cancelled jobs, and old copies moved aside by
*Replace*, are never deleted by themselves: History → Leftovers lists and deletes them.

## Audio CDs

**AccurateRip: "not in the database"**: nobody has submitted this pressing. CTDB, checked
alongside, often has it.

**AccurateRip: no track matches**: the finished job explains which it is:
- found nowhere: a pressing nobody submitted; nothing to compare with;
- found exactly in place but the tracks differ: read errors, or another mastering;
- found shifted by some samples on every track: another pressing that sits apart (a
  *pressing offset*). Your read offset is not the problem unless other CDs fail too; the
  drive's offset is listed at accuraterip.com/driveoffsets.htm (Settings → Music → Drive
  read offset).

**CTDB: no track matches, while AccurateRip says all is well**: CTDB files rips of every
pressing under the same table of contents. When none of its rips matches, DOM looks for a
pressing offset (the same audio a few hundred samples apart) and, if it finds one, checks
every track at it. The audio is then proven right, only placed differently. With AccurateRip
confirming the rip as read, it is another pressing ("…, another pressing (offset −120
samples)"); without, it may also be the drive's read offset that is off by that much: if
other CDs match only when moved by the same amount, change it. It needs the WAVs kept for
AccurateRip's second look (AccurateRip on).

**Verify again** on a finished FLAC rip checks the saved files against both databases
again later: they grow.

**The server slows down or hangs while a scratched CD is read**: before 0.4.40 DOM kept
every line cdparanoia printed, and it prints each failed read: at a bad spot, or with a drive
that dropped off the USB bus, that is an endless stream, and it filled the memory (the
kernel log then shows `oom-killer` and `dom`). Now only the last lines are kept. DOM also
stops a read that makes no progress for *Give up on a stuck spot after* minutes (5 by
default, Settings → Music), and the template caps the container's memory (`--memory=4g` in
Extra Parameters; add it to an existing container), so the rest of the server is safe.

**The drive dropped off the USB bus** (kernel log: `usb … disconnect`, then the drive
attached again as `sr1` / another `sg` number): the container still points at the old
devices. Unplug and plug the drive back in once nothing else uses it (it takes `sr0` again),
check the `sg` number (see *The drive* above), and restart the container.

**"Could not read track N"**: a dirty or scratched spot. Each failing track is read again
by itself first (*Reads of a failing track*). Then clean the disc (from the middle
outwards), maybe raise *Retries of a bad spot*, and press **Resume**: the tracks already
done are kept, only the rest is read.

**Wrong or missing titles**: *Fix match…* searches MusicBrainz by artist and album (releases
not entered as CDs are found too); *Edit titles* in the Tracks panel takes titles by hand
or pasted from any track list.

## Data discs

**"Data disc" but it holds a film**: MakeMKV found no video on it (a home-made disc, a
damaged one). It is kept whole as an ISO; MakeMKV can open the image later.

**Unreadable sectors reported**: ddrescue retried them (*iso.retries*); they are zeros in
the image. Cleaning the disc and saving again may get them.
