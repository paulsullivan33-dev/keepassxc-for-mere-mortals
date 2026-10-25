# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Built and verified 2026-10-24/25 against
**KeePassXC 2.7.6** (Ubuntu package
2.7.6+dfsg.1-1build3) on a 2-CPU, 7.7 GB
sandbox machine running Ubuntu 24.04.
The package was installed with apt; the
command-line tool is `keepassxc-cli`,
same version.

**The lab, declared.** Every database in
the lab is a fixture with a *published*
password (`demo-master-passphrase`),
invented example accounts
(`reader@example.com` and friends, on
`example.com` / `example.org` hosts and
a documentation-range IP), and nothing
else. The fixture databases, the key
file, the CSV and XML exports, and a
two-line stand-in for the HIBP list are
kept in `keepassxc-lab/fixtures/` so a
reader can repeat every step. Nothing
in the lab is modelled on any real
person, organisation or event.

**Ran for real** (transcripts in
`keepassxc-lab/transcripts/`):

- t00 — versions and the CLI command
  list (26 commands). The GUI binary
  refuses to start without a display;
  its refusal is kept in t15 and
  chapters 3 and 17 explain it.
- t01 — `db-create` with the password
  supplied twice on stdin (the
  non-interactive pattern every later
  transcript uses). `db-info` reports
  the result: AES 256-bit cipher, AES
  KDF at 1,000,000 rounds, recycle bin
  enabled. The fresh file's permissions
  are `0600`.
- t02 — three groups created with
  `mkdir`; three entries added (one
  with a typed password, one with a
  generated 20-character password, one
  for a router); `ls -R` shows the
  tree.
- t03/t03b — `show` redacts the
  password in its summary *even with
  `-s`*; the password comes out only
  when requested by name:
  `show -s -a Password` printed both
  the typed and the generated password
  verbatim. `search` found entries by
  partial term.
- t04 — `edit` changed a URL; `mv`
  moved an entry between groups; `rm`
  **recycled** an entry and `rmdir`
  recycled a group — both landed in
  the Recycle Bin group, which the
  listing then shows. Deleting in
  KeePassXC is recycling until the bin
  is emptied.
- t05 — `generate` in five shapes
  (bare, `-L 20`, letters+digits+
  specials, `--exclude-similar`, a
  custom hex character set) and
  `diceware` at the default 7 words
  and at `-W 6` and `-W 4`, all from
  the bundled EFF word list.
- t06 — `estimate` (zxcvbn) on four
  passwords: `hunter2` ≈ 10.8 bits,
  a leetspeak word ≈ 30.8, the famous
  four-word passphrase ≈ 70.3, and the
  advanced breakdown of the generated
  password from t02 (102.2 bits, with
  its pattern segments itemised).
  `db-info`'s statistics counted the
  lab's deliberately planted reuse:
  5 entries, 4 unique passwords,
  maximum reuse 2, 4 weak passwords.
- t06b/t06c — `analyze` is the offline
  Have I Been Pwned check and does
  nothing without the hash list
  ("Cannot find HIBP file"). The lab
  built a two-line stand-in list:
  plain hashes were rejected with a
  parse error; the `HASH:count`
  format parsed, and the check then
  flagged both planted entries:
  "Password for 'Web/Shared Weak A'
  has been leaked 9542 times!" (the
  count is the fixture's own line, of
  course — the real list is a large
  download the lab did not fetch).
- t07 — `export` to CSV (plaintext,
  kept as a fixture so readers see
  exactly what leaves the vault) and
  to XML; `import` of the XML into a
  new database, verified by listing
  it. Feeding the CSV to the CLI's
  `import` failed verbatim: "Unable
  to import XML database: No root
  group" — **the CLI imports XML
  only**; CSV import is a GUI feature.
- t08 — a second database with one
  entry of its own; `merge
  --dry-run` previewed the change
  ("Creating missing Rail Pass") and
  the real merge applied it, verified
  by listing.
- t09 — a database created with a
  password **and** a key file
  (`--set-key-file` wrote a 128-byte
  binary key file, mode `r--------`).
  The right password without the key
  file failed with the same generic
  "Invalid credentials" error as a
  wrong password; with `-k` it opened.
  `db-edit --set-password` changed
  another database's password: the old
  one then failed, the new one worked.
- t10 — `clip` under the virtual
  display: the password was read back
  off the X clipboard with `xclip`,
  the 10-second countdown ran, and
  the clipboard was empty afterwards.
- t11/t11b — the KDBX format version
  read from the file headers: a fresh
  CLI `db-create` writes **KDBX 3.1**;
  `import` writes **KDBX 4.1**; and in
  a controlled rerun, a 3.1 file
  stayed 3.1 through create, mkdir,
  add and merge, and became 4.1 at the
  moment an entry was first
  **recycled**. `db-info` reports both
  formats identically.
- t12 — the GUI run for real (below).
- t13 — an attachment imported into
  an entry, listed by
  `show --show-attachments`, exported
  back, and removed. Exporting to `-`
  does not mean stdout here: it
  created a file literally named `-`
  (kept out of the fixtures, described
  in chapter 9).
- t14 — the `open` interactive shell
  starts and prompts; the lab drove
  everything else with one-shot
  commands, and piped subcommands
  produced no further output, so the
  shell is presented as an interactive
  tool only.
- t15 — proofing checks run while the
  guide was written: the GUI binary's
  no-display refusal verbatim, the
  `-t` decryption-time and `edit -g`
  help lines, bare `export` defaulting
  to XML, and adding an entry under a
  nonexistent group failing with
  "Could not create entry with path".
- t90 — the error set: wrong password
  (with its "(HMAC mismatch)" tail),
  a missing file, a CSV mistaken for
  a database ("Not a KeePass
  database"), and an unknown command.

**The GUI, run for real.** KeePassXC
was launched under Xvfb (a virtual
display) on the sandbox, driven with
`xdotool` and photographed with
ImageMagick. Screenshots in
`keepassxc-lab/screenshots/`: the
unlock screen (password field, Key
File and Hardware Key fields); the
main window after the lab typed the
fixture password (group tree, entry
list, preview pane with its General /
Advanced / Autotype tabs, the
"Weak Passwords" saved search); the
standalone Password Generator on both
tabs — a 20-character password rated
125.72 bits, and a 7-word passphrase
from the EFF large wordlist rated
90.47 bits. The Database, Groups and
Tools menus were opened and read
(Export offers CSV, HTML and XML;
the Import submenu exists). What was
*not* done in the window: creating a
database through the New Database
wizard, and the CSV import wizard —
both are described from the
application's own menus and
documentation and are labelled
documented-only in chapters 4 and 10.

**Documented only, and labelled as
such in the chapters:** Auto-Type
performed into another application;
browser integration (KeePassXC-Browser
and its extension); hardware keys
(the CLI's `-y` flag and the unlock
screen's Hardware Key field were seen,
no YubiKey was exercised); TOTP
codes (the fields and `show -t`
exist; no seed was enrolled); GUI
database creation and CSV import
(above); the GUI's database reports;
SSH-agent and secret-service
integration; Windows and macOS
installs and their quick-unlock
biometrics; and KDBX 4's Argon2 KDF
settings (the GUI's security dialog
offers them, but every database the
lab created used the CLI's AES KDF,
and the numbers in chapter 2 are the
lab's own).

**Fixtures:** all accounts, hosts and
passwords are invented for the lab,
the master password is published in
the transcripts on purpose, and the
exports in `fixtures/` contain only
those invented entries — do not treat
a real export that casually (chapter
10 explains why).
