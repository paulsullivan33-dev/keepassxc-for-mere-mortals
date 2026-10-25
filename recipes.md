# Recipes

Five jobs, end to end. Commands quoted were
run in the lab unless a step is the
window's, and window-only steps are marked.

## 1. Move in from a browser's password store

*You need: the browser's CSV export, the
window's CSV importer (documented in
chapter 10 — the lab verified the menu,
not the wizard).*

1. In the browser's password settings,
   choose **Export passwords** and save the
   CSV. It is plaintext — chapter 10's
   warnings apply from this second.
2. In KeePassXC: **Database → Import → CSV
   File…**, choose the file, and check the
   column mapping (browser exports usually
   make you map the URL column by hand).
3. Spot-check five entries in the new
   database against the browser — including
   the one with a symbol in its password.
4. Shred the CSV (`shred -u passwords.csv`
   on Linux; delete and empty the Recycle
   Bin elsewhere — and consider that "delete"
   on a modern SSD is weaker than shredding,
   which is another reason the export should
   live as briefly as possible).
5. Turn *off* the browser's offer to save
   passwords, so new passwords have one
   home. Leave the browser's stored copies
   until the vault has proven itself for a
   week; then clear them too.

## 2. A household database two people keep

*You need: one database, one shared master
password, a shared folder or stick.*

1. Create the database (chapter 4) with a
   passphrase both people can remember —
   this is the "something you both know"
   school of access control, and for a
   household it is usually the honest one.
2. Keep the live copy in the shared place;
   treat other copies as read-only
   travellers (chapter 11's discipline).
3. When both of you edited since the last
   copy — it will happen — merge, don't
   overwrite:
   `keepassxc-cli merge --same-credentials mine.kdbx theirs.kdbx`
   after a `--dry-run` first to see what
   will change.
4. Backups (chapter 13): two dated copies,
   two different pieces of hardware.
5. Agree the exit rule in advance: if the
   arrangement ever ends, the database gets
   a new master password and the entries
   that matter get new passwords. Vaults
   outlive arrangements; plan like it.

## 3. The router entry, complete with its recovery note

*Everything here was run in the lab, in
substance.*

```console
$ keepassxc-cli add vault.kdbx "Web/Home Router" \
    -u admin -p --url http://192.0.2.1 \
    --notes "Admin page login"
$ keepassxc-cli attachment-import vault.kdbx "Web/Home Router" \
    reset-note.txt reset-note.txt
```

The password is prompted for, typed once,
and never appears in the shell history; the
router's factory-reset note rides inside
the entry as an attachment (chapter 9),
where a plain file on the disk would drift
away. Six months later,
`keepassxc-cli show --show-attachments` is
how you remember it exists.

## 4. A script that fetches one password

*For a backup script, a deploy step, a home
automation trigger.*

```bash
#!/bin/sh
read -s -p "Vault password: " VAULT_PW; echo
DB_PASS=$(printf '%s\n' "$VAULT_PW" |
  keepassxc-cli show -s -a Password vault.kdbx "Automation/Backup Job")
```

The shape to copy: the master password is
read invisibly into a variable — never
written into the script; `show -s -a
Password` (chapter 9) returns exactly the
one secret, nothing else to parse; the
secret lands in an environment variable for
the one command that needs it. What not to
do: `echo the-master-password |` in the
script itself, which files your master
password in plaintext next to the thing it
protects.

## 5. The spring clean

*Twenty minutes, twice a year. Every tool
here is chapter 7's.*

1. `keepassxc-cli db-info vault.kdbx` — read
   the statistics. Reused passwords: your
   work list, in order.
2. For each reused entry: `edit -g -L 20`
   to give it a fresh generated password,
   then change it at the site itself.
   The vault's copy is only correct once
   the site agrees.
3. `keepassxc-cli analyze -H hibp-list.txt vault.kdbx`
   — anything leaked gets the same
   treatment, bank and mail first.
4. Empty the Recycle Bin (in the window) —
   noticing, as you do, anything in it you
   did not mean to delete, and moving it
   back instead.
5. Save a dated backup copy (chapter 13)
   of the cleaned vault. Clean *and* copied
   is the finished state.

Check: each recipe names the moment it
could hurt you — the plaintext CSV, the
diverged copy, the password in a script —
and handles it in the step where it
appears. That is the difference between a
recipe and a liability.
