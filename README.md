# KeePassXC for Mere Mortals

A plain-language guide to KeePassXC, the
offline password manager: all your passwords
in one encrypted file, on machines you
control, opened by one master password you
memorise. No account, no server, no
subscription — and no company to lose them,
either. The surprises are labelled; the
numbers quoted were produced by the program
itself.

The lab ran KeePassXC 2.7.6 on Ubuntu 24.04
in a sandbox. Command-line first: databases
were created, filled, searched, audited,
exported, imported and merged with
`keepassxc-cli`, every step transcribed. The
real graphical program was then launched on
a virtual display, unlocked, photographed,
and its password generator read on both
tabs. Everything the lab did used a fixture
database whose master password is published
in the transcripts on purpose, so you can
repeat it all.

Three findings are worth meeting here,
because they change how you handle the
thing. First, deleting is recycling:
deleted entries and groups sit in the
database's Recycle Bin until you empty it.
Second, the command line imports XML only
— the CSV import the program is famous for
lives in the window. Third, the command
line's `show` keeps the password redacted
even when you pass its "show protected"
flag; the password is printed only when
asked for by name, which turns out to be
the right design for scripts.

What the lab could not do: there is no
YubiKey in the sandbox, no second
application to type passwords into, and no
browser to integrate with — so hardware
keys, Auto-Type performed for real, and
browser integration are written from the
documentation and labelled as such wherever
they appear, never presented as tested. The
New Database and CSV import wizards in the
window were likewise read about, not
clicked through; their menus were opened
and verified real.

Built with MkDocs; the chapters live at the
repository root and the deploy workflow
stages them into `docs/` at build time.

## What this guide covers

- **[What KeePassXC Is](what-keepassxc-is.md)** —
  the one-file idea, and what it is not.
- **[The Moving Parts](the-moving-parts.md)** —
  the database, its credentials, the KDF
  and cipher, entries, groups, and the two
  programs that share one file.
- **[Installing KeePassXC](installing-keepassxc.md)** —
  the package or installer, and proving the
  install answers.
- **[Your First Database](your-first-database.md)** —
  creating the file, choosing the master
  password, and a tour of the main window.
- **[Entries, Groups and Search](entries-groups-and-search.md)** —
  what an entry holds, organising with
  groups, finding things again, and the
  Recycle Bin.
- **[The Password Generator](the-password-generator.md)** —
  random-character passwords and diceware
  passphrases, in the window and on the
  command line, with the entropy readouts
  shown.
- **[Judging Passwords](judging-passwords.md)** —
  `estimate`, the database's own statistics,
  and the offline leaked-password check.
- **[Keys Beyond the Password](keys-beyond-the-password.md)** —
  key files (verified end to end), changing
  credentials, and hardware keys.
- **[The Command Line](the-command-line.md)** —
  `keepassxc-cli` as a working tool:
  scripted lookups, the clipboard, and
  attachments.
- **[Import and Export](import-and-export.md)** —
  CSV and XML in both directions, the
  command line's XML-only limit, and
  handling plaintext exports safely.
- **[One File, Many Machines](one-file-many-machines.md)** —
  carrying the file between computers, the
  two-copies problem, and merging copies
  back together.
- **[Auto-Type and Browsers](autotype-and-browsers.md)** —
  having KeePassXC type for you, and the
  browser extension — documented, not
  lab-tested.
- **[Backup and Recovery](backup-and-recovery.md)** —
  copying the file is the backup; the
  forgotten-password truth, stated plainly.
- **[Recipes](recipes.md)** — moving in
  from a browser, a shared household
  database, a scripted lookup, a spring
  clean, and a travel copy.
- **[The Neighbours](the-neighbours.md)** —
  KeePass, the phone apps, Bitwarden and
  friends, and what each trades.
- **[Limits](limits.md)** — the honest
  list of what one encrypted file cannot
  do.
- **[Troubleshooting](troubleshooting.md)** —
  symptom → likely cause → fix.
- **[Glossary](glossary.md)** — the
  vocabulary, in plain words.

## What it doesn't cover

- **Recovering a forgotten master
  password.** Nobody can; chapter 13
  explains the design and the precautions.
- **KeePassXC-Browser setup in depth** —
  chapter 12 describes the mechanism and
  the risks; the project's own docs carry
  the per-browser steps.
- **Building KeePassXC from source**, and
  its portable and cross-platform packaging
  details beyond the install chapter.
- **Auditing the cryptography.** The
  formats and algorithms are named and
  their defaults measured where the tool
  reports them; this is not a security
  review.
- **Enterprise deployment** — policies,
  provisioning and shared vaults at work
  scale are other tools' territory
  (chapter 15).

## How to use this guide

The order above works front to back, and
each chapter stands alone with its own
"Check:" blocks. If you are brand new, read
chapters 1–4 and create the database
alongside. If you already keep passwords in
a browser, chapter 10 (and the first
recipe) is your way in. Every chapter names
what was actually run when it matters; the
[criteria](criteria.md) page carries the
full verification ledger, transcripts
included.
