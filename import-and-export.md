# Import and Export

Sooner or later passwords cross the border:
in from wherever they live now, or out to
somewhere else. KeePassXC speaks two export
formats, and the border has one asymmetry
the lab found the hard way. This chapter
covers both, plus how to handle what comes
out without hurting yourself.

## Exporting: everything, in the clear

```console
$ keepassxc-cli export vault.kdbx > vault-export.xml
$ keepassxc-cli export -f csv vault.kdbx > vault-export.csv
```

**XML** is the KeePass interchange format —
the whole database as structured text,
attachments and all. **CSV** is the lowest
common denominator every password manager
on earth can read (every field quoted, in
KeePassXC's own export):

```csv
"Group","Title","Username","Password","URL","Notes","TOTP","Icon","Last Modified","Created"
"Passwords/Email","Example Mail","reader@example.com","S3cure!Demo","https://mail.example.com","Demo mailbox entry","","0","2026-10-25T02:18:46Z","2026-10-25T02:18:46Z"
```

Look at that line and take the point:
**an export is plaintext.** Every password
you own, readable by anyone who opens the
file, forever, until you destroy it. Treat
an export the way you'd treat a printed list
of every key you own:

- export onto your own machine, ideally
  into an encrypted folder;
- use it immediately for its one purpose;
- then delete it properly — on Linux,
  `shred -u vault-export.csv` overwrites
  before removing — and empty the Recycle
  Bin this guide keeps warning you about:
  the desktop's, this time.

The window's **Database → Export** submenu
(verified real in the lab) offers CSV and
XML too, plus **HTML** — a readable,
printable listing, every bit as plaintext
as the others.

## Importing: the asymmetry

The command line imports **XML only**:

```console
$ keepassxc-cli import -p vault-export.xml imported.kdbx
Enter password to encrypt database (optional): 
Repeat password: 
Successfully imported database.
```

(`-p` says the new database gets a password,
supplied here by piping it in twice.) The
lab imported its own XML export into a
fresh database and listed the result: the
entries all came home. One cosmetic note:
the imported copy's groups sit at the top
level — the "Passwords" root level of the
original is not recreated — because the
XML describes the groups, not the wrapper.
The contents were intact; the missing
wrapper is a detail to know, not data loss.

Feeding the same tool the **CSV** export
fails, verbatim:

```console
$ keepassxc-cli import -p vault-export.csv from-csv.kdbx
...
Unable to import XML database: No root group
```

There is no `--format` flag hiding
anywhere — the CLI's `import` is an XML
importer, full stop. **CSV import lives in
the window**: **Database → Import** has a
submenu (the lab verified the menu; it did
not click the wizard through, so this part
is documented from the application's own
materials). The wizard asks for the file,
then shows a column-mapping screen — which
column is the title, which the username,
which the password — because the world's
CSVs do not agree on headers. KeePassXC's
own CSV exports map automatically; a
browser's export usually needs the mapping
checked.

## Moving in from somewhere else

The general recipe, whatever the source:

1. In the old place — browser, other
   manager — find its **export** and take
   the CSV.
2. In KeePassXC's window, **Database →
   Import → CSV File…**, pick the file, and
   check the column mapping against the
   first rows shown.
3. Unlock the new database and *spot-check
   five entries against the originals* —
   including one with an odd character in
   its password; CSV quoting is where
   migrations go to die.
4. Shred the CSV (above). It was the most
   dangerous file on your computer while it
   existed.

The GUI also carries dedicated importers
for several named sources (among them
1Password and KeePass 1.x files) — those
live in the same Import submenu and are
documented by the project; the lab
exercised the XML path and read the rest.
And if you are leaving *for* somewhere
else, the CSV export above is the door —
which is precisely as it should be. Your
passwords are yours; the format is a
standard; nothing in this design holds
them hostage.

Check: you can produce a CSV and an XML
export, import the XML back into a new
database and list its entries, and state
where the CSV importer lives (the window)
and what an export file is (all your
passwords, in the clear — handle it like
it matters).
