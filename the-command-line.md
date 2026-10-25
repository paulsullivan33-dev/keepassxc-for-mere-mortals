# The Command Line

Everything the window does to a database,
`keepassxc-cli` does from a terminal — and
some things, like feeding a password to a
script, it does better. This chapter is the
working tour. The lab ran every command
quoted here; transcripts are in
`keepassxc-lab/transcripts/`.

## The shape of every command

```console
keepassxc-cli <command> [options] <database> [arguments]
```

`keepassxc-cli help` lists 26 commands;
`keepassxc-cli help <command>` documents
one. Nearly all take the database password
interactively — which raises the first
practical question.

## The password, without typing it every time

Every command prompts `Enter password to
unlock <db>:`. Scripts can't type, so the
lab's pattern throughout this guide is to
pipe the password in:

```console
$ echo 'demo-master-passphrase' | keepassxc-cli ls vault.kdbx
Enter password to unlock vault.kdbx: 
Rail Pass
Web/
Email/
Finance/
Recycle Bin/
```

Two honest warnings about that pattern.
Writing a real master password literally
into a script or a shell command puts it in
files and in your shell history — for real
vaults, prefer reading it from a prompt
into a variable (`read -s`) or keeping the
scripted work to fixture vaults like this
one. And there is an interactive
alternative: `keepassxc-cli open vault.kdbx`
unlocks once and gives a `vault.kdbx>`
prompt where `ls`, `show` and friends run
without re-prompting, until `close`. The
lab started it and saw the prompt; it drove
everything else one-shot, and presents the
shell as the interactive tool it is.

## Revealing a password: the redaction surprise

Chapter 5 showed `show` printing
`Password: <redacted>`. Here is the surprise
in full — the obvious flag does not lift
it:

```console
$ keepassxc-cli show -s vault.kdbx "Email/Example Mail"
...
Password: <redacted>

$ keepassxc-cli show --all -s vault.kdbx "Email/Example Mail"
...
Password: <redacted>
```

`-s` says "show protected attributes", and
the summary's password line *still* reads
`<redacted>` — the outputs are identical.
The way that works is to ask for the
password **by name**:

```console
$ keepassxc-cli show -s -a Password vault.kdbx "Email/Example Mail"
S3cure!Demo
```

That prints exactly the password and
nothing else — which is precisely what a
script wants:

```console
$ PW=$(keepassxc-cli show -s -a Password vault.kdbx "Finance/Example Bank")
```

It reads as a quirk and behaves like a
design: the password is never spilled as a
side effect of looking at an entry; you
have to name it. Custom attributes work the
same way — `-a <name>`, with `-s` for the
protected ones — and `-t` prints the
entry's one-time code if it has one.

## The clipboard, with a self-destruct

`clip` copies an entry's password to the
clipboard and counts down before wiping it:

```console
$ keepassxc-cli clip vault.kdbx "Email/Example Mail"
Entry's "Password" attribute copied to the clipboard!
Clearing the clipboard in 10 seconds...
```

The lab verified the whole cycle under a
virtual display: read the clipboard back
with `xclip` (the password was there), let
the countdown run, read again (empty). The
count is settable (`clip <db> <entry> 30`).
It needs a real clipboard to copy into —
on a headless box it has nowhere to put
anything, which chapter 17 covers. In daily
life this is the terminal user's paste: copy,
paste into the form, forget.

## Attachments

Entries can carry small files — a recovery
code sheet, a licence key file, a photo of
a hardware token's codes. The CLI's verbs
are `attachment-import`, `attachment-export`
and `attachment-rm`, and `--show-attachments`
lists them:

```console
$ keepassxc-cli attachment-import vault.kdbx \
    "Email/Example Mail" recovery.txt recovery-note.txt
Successfully imported attachment recovery-note.txt as recovery.txt to entry Email/Example Mail.

$ keepassxc-cli show --show-attachments vault.kdbx "Email/Example Mail"
...
Attachments:
  recovery.txt (43.0 B)
```

One gotcha, earned in the lab:
`attachment-export ... -` does **not** mean
"to standard output". It creates a file
literally named `-`. Give it a real path.

## What else is in the toolbox

Beyond this chapter's scope but present and
listed by `help`: `analyze` and `estimate`
(chapter 7), `export`, `import` and `merge`
(chapters 10–11), `diceware` and `generate`
(chapter 6), `db-create`, `db-edit` and
`db-info` (chapters 4, 7, 8), and the entry
verbs of chapter 5. One tool, one file,
twenty-six verbs — and every one of them
scriptable, which is the CLI's whole point.

Check: from a terminal, with no window
open, you can find an entry (`search`), read
everything but its password (`show`), print
just the password (`show -s -a Password`),
and have it wiped from the clipboard ten
seconds after copying it (`clip`). That is
the command line earning its place next to
the window.
