# Entries, Groups and Search

A database earns its keep one **entry** at
a time. This chapter covers what an entry
holds, how **groups** organise them, how
**search** finds them again, and the
Recycle Bin that makes deleting forgiving.
Everything below was done by the lab on the
command line and cross-checked in the
window.

## What an entry holds

| Field | For |
|---|---|
| Title | The name you look for ("Example Bank") |
| Username | The account's sign-in name |
| Password | The secret itself |
| URL | Where it signs in |
| Notes | Free text: recovery hints, account numbers, warnings |
| Tags | Your own labels, searchable |
| Expiry | An optional date the entry flags itself old |
| Attachments | Small files stored inside the entry (chapter 9) |

Adding one in the window is **Entries →
Add New Entry** and a form. On the command
line, the lab ran:

```console
$ keepassxc-cli mkdir vault.kdbx Web
$ keepassxc-cli mkdir vault.kdbx Email
$ keepassxc-cli mkdir vault.kdbx Finance

$ keepassxc-cli add vault.kdbx "Email/Example Mail" \
    -u reader@example.com -p \
    --url https://mail.example.com \
    --notes "Demo mailbox entry"
Successfully added entry Example Mail.
```

`-p` means "ask me for the entry's
password" — it prompts once, invisibly.
Two siblings matter: `-P` would set the
prompted thing as the *database's* password
instead, and `-g` **generates** the entry's
password rather than asking:

```console
$ keepassxc-cli add vault.kdbx "Finance/Example Bank" \
    -u saver@example.com -g -L 20 \
    --url https://bank.example.com
Successfully added entry Example Bank.
```

The group is part of the entry's path
(`Email/Example Mail`), and the group must
exist first: a path naming a group that
doesn't fails outright —

```console
$ keepassxc-cli add vault.kdbx "NoSuchGroup/Ghost" -p
Could not create entry with path NoSuchGroup/Ghost.
```

— so `mkdir` before you file things in a
group. (That refusal was verified while
this guide was proofed; it is in transcript
t15.)

## Looking at what you have

`ls` lists a group; `ls -R` the whole tree;
`ls -Rf` flattens it into full paths:

```console
$ keepassxc-cli ls -R vault.kdbx
Web/
  Example Router
Email/
  Example Mail
Finance/
  Example Bank
```

`show` prints one entry — with its password
redacted, a surprise with its own section
in chapter 9:

```console
$ keepassxc-cli show vault.kdbx "Email/Example Mail"
Title: Example Mail
UserName: reader@example.com
Password: <redacted>
URL: https://mail.example.com
Notes: Demo mailbox entry
Uuid: {683f293b-6532-423e-9e44-d8a8030cb242}
Tags: 
```

## Search

In the window, the search box filters as
you type, across titles, usernames, URLs
and notes. The standing **Weak Passwords**
and **Expired** views in the sidebar are
searches the program keeps for you.

On the command line, `search` takes a term
and prints matching entries by full path:

```console
$ keepassxc-cli search vault.kdbx example
/Web/Example Router
/Email/Example Mail
/Finance/Example Bank

$ keepassxc-cli search vault.kdbx bank
/Finance/Example Bank
```

Search is deliberately shallow — it is a
finding tool, not a reporting tool. For
"which entries reuse this password",
chapter 7's `db-info` statistics are the
right instrument.

## Changing and removing

`edit` changes fields in place (the lab
fixed a URL from `http://` to
`https://` with `edit --url`); `mv` moves
an entry to another group. And `rm` —

```console
$ keepassxc-cli rm vault.kdbx "Web/Throwaway"
Successfully recycled entry Throwaway.
```

— **recycles**. It does not say "deleted",
and it is not being coy. The entry moved
to the **Recycle Bin** group, where `ls -R`
shows it, and `rmdir` treats a whole group
the same way. From the bin, an entry can be
moved back out with `mv` like any other.
Only emptying the bin — in the window,
right-click the Recycle Bin and choose
**Empty Recycle Bin** — actually destroys.
That two-step is a kindness: the day you
delete the wrong entry, it is a move, not
a loss. It is also a warning: a database
you "cleaned" still holds its secrets until
the bin is emptied, which matters when you
share or archive copies (chapters 11
and 13).

Check: `ls -R` shows your groups, your
entries, and a Recycle Bin that contains
anything you removed. If you can add, find,
edit, move and recycle an entry, you can
run a vault; the rest of the guide is about
doing it well.
