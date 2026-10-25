# The Moving Parts

Six ideas explain nearly everything
KeePassXC does. They are introduced here in
plain words and used, by name, for the rest
of the guide. All of them are in the
[glossary](glossary.md) too.

| Part | What it is |
|---|---|
| The database | One file, `something.kdbx`, holding everything |
| Credentials | The password, key file and/or hardware key that open it |
| The KDF | The slow machinery that turns your password into the file's key |
| The cipher | The algorithm that scrambles the contents (AES-256 in the lab) |
| Entries and groups | The passwords themselves, filed in folders |
| The two programs | The window, and `keepassxc-cli` in the terminal |

## The database file

Everything — entries, groups, attachments,
settings for the database itself — lives
in the one file. Copy the file and you have
copied the vault, which is why chapter 13's
backup advice is so short.

The format is **KDBX**. You will meet two
versions in the wild, **KDBX 3.1** and
**KDBX 4.x**. KeePassXC reads and writes
both; very old KeePass programs may not
know 4.x. The lab measured which is which
by reading the file headers
(`keepassxc-lab/transcripts/t11-kdbx-version.txt`):
a database created by the command line
starts life as KDBX 3.1, and — this caught
the lab by surprise — the same file became
KDBX 4.1 at the moment an entry was first
deleted into the Recycle Bin. Nothing about
the contents changes; treat the version as
plumbing, noticed only if you share files
with much older software.

## Credentials: what opens it

A database opens with one or more
**credentials**:

- a **master password** (the usual case,
  and the only one you can memorise);
- a **key file** — a small file whose
  contents are part of the key, so opening
  takes something you know *and* something
  you have (chapter 8, verified end to end);
- a **hardware key** such as a YubiKey,
  which answers a cryptographic challenge
  (documented only in this guide — no key
  was available to the lab).

The unlock screen shows all three at once:
a password field, a **Key File** field, and
a **Hardware Key** field. That photograph
is the lab's own (`keepassxc-lab/screenshots/`
holds it). Whichever combination you set is
required *in full*, every time — and a
wrong combination fails with one generic
message, so the error never tells an
attacker which part was wrong. The lab
confirmed that too: the right password
without its key file fails exactly like a
wrong password.

## The KDF: making guessing expensive

Your password is not the file's key. It is
fed through a **key derivation function**
(KDF) — a deliberately slow calculation —
and *its* output unlocks the cipher. Slow
is the point: you wait a fraction of a
second once; an attacker trying a billion
guesses waits a billion fractions.

The lab's database reported its own
settings (`db-info`): KDF **"AES (1000000
rounds)"** — the older AES-based KDF run a
million times. KDBX 4 databases can instead
use **Argon2**, a modern KDF designed to be
expensive in memory as well as time; the
graphical program's database security
settings offer it, and new databases made
in the window use KDBX 4. The command line
in 2.7.6 creates with the AES KDF unless
told a target decryption time (`db-create
-t` sets how long unlocking should take,
and the program raises the rounds to match
— that flag was in the help text; the lab
did not re-time it).

## The cipher

The contents are encrypted with a **cipher**
— **AES with a 256-bit key** in every
database the lab created, per `db-info`.
KDBX 4 also allows ChaCha20; the choice is
a database setting in the window, not
something an ordinary user needs to shop
for. Both are well-regarded; the defaults
are sane, and this guide leaves them alone.

## Entries and groups

An **entry** is one stored secret: title,
username, password, URL, notes, plus extras
like tags, an expiry date, file
**attachments**, and a place for one-time
(TOTP) codes. A **group** is a folder of
entries, nestable as deep as you like. The
top group of a new database is named
"Passwords" — you will see it at the root
of every listing in this guide's lab.

Deleting deserves its own sentence:
**deleting recycles**. A deleted entry or
group moves to the database's **Recycle
Bin** group and stays there, recoverable,
until the bin is emptied. Chapter 5 shows
the lab's listing with the Recycle Bin
holding exactly what was "deleted".

## The two programs, one file

The window and `keepassxc-cli` are separate
programs reading and writing the same
format. Nothing in the file belongs to one
or the other: the lab created its database
in a terminal, unlocked that same file in
the window, and merged it from the terminal
again. Your settings for the *program*
(colours, lock timeouts, where it looks for
files) live separately, in a configuration
file under your home directory — they are
not in the vault and do not travel with it.

Check: name the credential you know (the
master password), the one you can hold (a
key file), and the part that makes password
guessing slow (the KDF). Chapter 4 asks you
to choose the first of those well.
