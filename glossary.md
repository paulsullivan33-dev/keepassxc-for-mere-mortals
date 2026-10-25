# Glossary

**Attachment** — A small file stored inside
an entry (a recovery note, a licence file).
Chapters 5 and 9.

**Auto-Type** — KeePassXC simulating
keystrokes to fill a sign-in form in another
window, driven by a per-entry sequence.
Chapter 12.

**Cipher** — The algorithm that encrypts
the database contents; AES-256 in every
database this guide's lab created.

**Credential** — One of the things that
opens a database: the master password, a
key file, or a hardware key, in whatever
combination the database was set up with.

**Database** — The single encrypted file
(`.kdbx`) holding all entries. The vault.

**Diceware** — Making a passphrase by
rolling dice (or a faithful simulation) to
pick words from a published list, so the
choice is random rather than invented.

**Entropy** — Strength measured in bits:
how many yes/no questions an attacker must,
in effect, answer to guess the secret. Each
diceware word from the EFF large list adds
about 12.9 bits.

**Entry** — One stored secret: title,
username, password, URL, notes, and extras.

**Export** — Writing the database's
contents out as CSV, XML or HTML — always
plaintext. Chapter 10.

**Group** — A folder of entries inside the
database. The top group of a new database
is named "Passwords".

**Hardware key** — A physical device (a
YubiKey, commonly) enrolled as a credential;
it answers a challenge at unlock and cannot
be copied from a disk.

**HIBP (Have I Been Pwned)** — The public
collection of breached passwords that
`analyze` checks against, offline, using a
downloaded list of hashes.

**KDBX** — The database file format,
documented and shared across the KeePass
family. Versions 3.1 and 4.x are both in
the wild; KeePassXC handles both.

**KDF (key derivation function)** — The
deliberately slow calculation that turns
your master password into the file's actual
key, making guessing expensive. AES-KDF and
Argon2 are the two you will meet.

**KeePassXC-Browser** — The browser
integration: an extension that asks the
desktop program to fill sign-in forms.
Chapter 12.

**Key file** — A file whose contents form
part of the database's key, so opening
needs the file as well as the password.

**Master password** — The one password you
memorise, from which (via the KDF) the
database's key is derived. Not stored
anywhere; not recoverable.

**Merge** — Combining two copies of a
database into their union, newest version
of each entry winning. Chapter 11.

**Passphrase** — A password made of several
whole words — longer, stronger, and easier
to type than a short clever string.

**Recycle Bin** — The special group deleted
entries and groups move to. Deleting in
KeePassXC is recycling until the bin is
emptied.

**TOTP** — Time-based one-time passwords:
the six-digit codes that change every 30
seconds. Entries can hold the seed and
produce the codes.

**zxcvbn** — The password-strength estimator
behind `estimate`, scoring by recognisable
patterns rather than character-class rules.
