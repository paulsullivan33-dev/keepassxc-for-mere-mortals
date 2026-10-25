# Backup and Recovery

A password vault concentrates risk exactly
as much as it concentrates convenience:
every secret you own, in one file, behind
one password. This chapter is the small set
of habits that keeps that bargain safe. It
is short because the design makes it short.

## The backup is a copy of the file

There is nothing else to back up — no
database server, no account state, no
export ritual. Copy `vault.kdbx` somewhere
else:

- a second disk or a USB stick;
- the same sync folder chapter 11 already
  uses — a synced copy *is* a backup
  against disk death (though not against
  your own mistakes, which sync faithfully);
- occasionally, a printed nothing — you
  cannot print a database usefully, which is
  why the copies above matter.

Three refinements separate a backup from a
pile of files:

1. **Keep more than one generation.** A
   corrupted or half-saved vault copied over
   your only backup is how single files end
   careers. Keep dated copies —
   `vault-2026-10.kdbx` next to
   `vault-2026-09.kdbx` — and let them age
   out slowly. The window's **Database →
   Save Database Backup…** writes a
   timestamped copy for exactly this (seen
   in the lab's menu reading).
2. **The key file is backed up separately**
   (chapter 8). A backup set containing the
   database and no key file is a locked box;
   one containing both, in the same folder,
   is a box with the key taped to it.
   Different places, both deliberate.
3. **Test a backup by opening it.** Once in
   a while, copy a backup to a scratch
   folder, open it (`keepassxc-cli db-info`
   is enough), and confirm the entries are
   there and the password works. An untested
   backup is a hope, not a backup.

## The forgotten-password truth

There is no recovery. Not "no easy
recovery", not "recovery with support's
help" — none, by design. The master
password is not stored anywhere, in any
form, by anyone. If it is forgotten and no
key file or hardware key shares the job:

- an older backup made *before* a password
  change still opens with the *old*
  password — the one scenario where dated
  generations rescue you;
- otherwise, the contents are sealed for
  good, and the work is the miserable one:
  resetting every account through each
  service's own "forgot password" flow,
  using whatever recovery methods those
  accounts still have.

So the precaution from chapter 4 bears its
weight: **write the master password down,
physically, somewhere safe**, at least while
it is new — a sealed envelope at home, a
copy with a trusted person, the same places
you would keep a spare house key. Security
advice that pretends humans memorise a
90-bit passphrase on day one gets vaults
exactly as lost as no advice at all.

## The Recycle Bin is not a backup

It sits inside the same file. It protects
against a slip of the mouse; it does not
protect against a dead disk, a stolen
laptop or a corrupted save. Entries you
truly need back after *file* trouble come
from the dated generations above, or they
don't come back.

## When the machine itself is the problem

A stolen laptop with a *locked* vault on it
is a sealed box: the thief has ciphertext
and time. Your defence in depth is the
master password's strength (a long
passphrase makes the guessing economics
absurd, chapter 6) and, if you use one, the
key file *not* being on the same laptop.
Rotate the master password (chapter 8's
`db-edit`) if a copy of the file may have
been exposed — remembering that exposure of
an old copy means changing the important
*entries* too, since the old copy still
opens with the old password wherever it
went. The vault protects secrets at rest;
it cannot un-leak them.

Check: you have at least two copies of the
database, in two places, made at two
different times; any key file has its own
separate copies; and you can open the older
one. If all three are true, this chapter has
done its job.
