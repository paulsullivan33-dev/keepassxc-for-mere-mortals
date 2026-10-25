# One File, Many Machines

One file is simple to copy — chapter 13's
backups depend on it — and most people want
the vault on more than one machine. This
chapter is the honest engineering of that:
how to carry the file, what goes wrong when
two copies live separate lives, and the
merge that reconciles them. The merge was
run for real in the lab; the carrying is
ordinary file copying, described plainly.

## Carrying the file

KeePassXC does not sync. It opens a file,
wherever the file is. So the transporting
is done by whatever already moves your
files:

- **A USB stick**, for machines that rarely
  meet. Simple, physical, and the stick
  becomes a backup copy whether you meant
  it to or not.
- **A file-sync tool** (Syncthing,
  Nextcloud, a shared folder, even emailing
  it to yourself): the encrypted file is
  safe to carry through untrusted places —
  that is what the encryption is *for*.
  The master password never travels; only
  the sealed file does.
- **The same folder on both machines** via
  your network.

What matters is not the vehicle but the
discipline below, because every vehicle
creates the same hazard.

## The two-copies problem

Copy the vault to a laptop. Add a password
on the desktop on Monday; add a different
one on the laptop on Tuesday. Now neither
file is "the" vault — each holds something
the other lacks, and copying either over
the other loses a day's change. Two sealed
envelopes, diverging. This is not a
KeePassXC flaw; it is arithmetic. Three
habits keep it tame:

1. **One writer at a time.** Treat the
   freshest copy as the vault; other copies
   are read-mostly.
2. **Copy before you travel, copy back when
   you return** — while you still remember
   which one changed.
3. **When both changed anyway, merge.**
   Merging is a first-class operation, not
   a rescue trick.

## Merging, for real

The lab built a second database with an
entry of its own, then merged it into the
main one. A dry run previews the union
without touching anything:

```console
$ keepassxc-cli merge --same-credentials --dry-run vault.kdbx other.kdbx
	Creating missing Rail Pass [3a5592ba06ed47658fb73ccf7733878c]
Database was not modified by merge operation.
```

`--same-credentials` says both files open
with the same password (without it, the
command prompts for each). The real run
prints the same "Creating missing" line and
ends "Successfully merged other.kdbx into
vault.kdbx." The union is then simply
there — note where the new entry landed,
at the top level, because that is where it
lived in the other file:

```console
$ keepassxc-cli ls -Rf vault.kdbx
Rail Pass
Web/
Web/Shared Weak A
Web/Shared Weak B
Email/
Email/Example Mail
Finance/
Finance/Example Bank
Finance/Example Router
Recycle Bin/
Recycle Bin/Throwaway
```
(Trimmed: the full listing is transcript
t08.)

Merging works group by group: entries
present in one file are created in the
other; entries present in both are resolved
to the *most recently modified* version —
KeePassXC keeps each entry's history and
timestamps for exactly this purpose, and
that newest-wins rule is the documented
behaviour (the lab's merge needed only the
creation path). One deliberate exception:
**deletions do not travel by merge.** An
entry recycled on one machine still exists
in the other, and a merge will resurrect
it. Emptying recycle bins and then removing
the entry from each copy is a manual job —
merges unite; they do not prune.

The window has the same operation at
**Database → Merge From Database…** (seen
in the lab's menu reading). It also has a
setting to merge automatically when the
file changes on disk under an open window —
worth enabling if a sync tool rewrites your
vault while you work, and documented rather
than lab-tested here.

## A format footnote for travellers

Chapter 2's measurement matters once here:
a database can be **KDBX 3.1** or **KDBX
4.1**, and both open in any current
KeePassXC. If a copy must also open in a
much older KeePass program somewhere, test
that first — 4.x is the one old software
may refuse — rather than discovering it at
the far end of a trip.

Check: copy your vault to a second place,
add a differently named entry to each copy,
merge one into the other, and list the
result: both new entries, one file. Then
you understand syncing KeePassXC better
than most tools let you.
