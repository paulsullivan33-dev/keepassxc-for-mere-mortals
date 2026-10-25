# What KeePassXC Is

**KeePassXC** is a password manager that
keeps every password you own in **one
encrypted file** on your own computer. You
open the file with one **master password**
— the only password you have to memorise —
and inside it lives everything else: the
mail password, the bank password, the
router password, in neat folders, each with
its username, the site's address, and your
notes.

There is no account to create, no server to
trust, and no subscription to renew. The
program runs entirely on your machine. If
KeePassXC's project vanished tomorrow, your
file would still open, because the file
format — called **KDBX** — is documented
and shared with a whole family of KeePass
programs on every platform.

That design buys three things:

- **Privacy by construction.** Your
  passwords are never on someone else's
  computer unless you put the file there.
  There is no password database of millions
  of strangers for an attacker to steal in
  one go; there is just your file, on your
  disk, encrypted.
- **Control.** You decide where the file
  lives, what copies exist, and what else
  opens it. Chapter 11 is about carrying it
  between machines; chapter 13 is about
  backing it up.
- **Longevity.** KDBX databases from twenty
  years of KeePass tools open in current
  programs. A password manager should
  outlive fashions.

And it costs three things, honestly:

- **The file is yours to protect.** Lose
  the only copy, or forget the master
  password, and the contents are gone —
  there is nobody to call. Chapter 13
  exists because of this.
- **Syncing is your job.** The program
  will happily keep copies on two machines;
  keeping them from drifting apart is
  handled with merging (chapter 11), not
  magic.
- **An unlocked database is a trusting
  database.** While it sits open on your
  screen, anything running as you can, in
  principle, read it. The program locks
  itself when idle, and chapter 16 covers
  the rest of that story.

## What it is not

- **Not a cloud service.** If you want
  passwords that simply appear on every
  device by signing in, that is the
  neighbours' design (chapter 15), with
  their servers holding an encrypted copy.
- **Not a browser feature.** Browsers will
  keep offering to save passwords; chapter
  10 shows how to move out of one.
- **Not a backup.** One file on one disk is
  one disk failure from a very bad week.
- **Not a secret-sharing tool.** Sharing
  the vault means sharing the whole vault
  and its master password. Families do it
  with care (a recipe in chapter 14); teams
  at work usually want something else.

## The two doors into the same room

KeePassXC ships as two programs over the
same file:

- the **graphical program** (`keepassxc`),
  the window most people live in — group
  tree on the left, entries in the middle,
  one entry's details below; and
- the **command line** (`keepassxc-cli`),
  which creates, lists, searches, edits,
  exports, imports and merges databases in
  a terminal, one command at a time.

This guide teaches both, because they check
each other's work: anything the window
shows, the command line can print, and the
lab in this guide's repository did exactly
that, step for step.

Check: you are ready for chapter 2 if you
can say what the one file is called (a
KDBX database), what opens it (the master
password, possibly with help — chapter 8),
and who else holds a copy (nobody, unless
you made one).
