# Keys Beyond the Password

Chapter 2 introduced the three credentials.
This chapter puts the second one to work —
the **key file** — covers changing a
database's credentials, and is honest about
the third, the **hardware key**, which the
lab could see but not touch.

## Why more than a password

A master password can be guessed, phished,
or shoulder-surfed. A key file adds the
second classic factor: something you
**have**. Opening the database then needs
the password in your head *and* a specific
file in your possession — on a USB stick,
say, that lives apart from the computer.
An attacker who steals the database file
alone gets nothing; one who steals the key
file alone also gets nothing.

It cuts both ways, and chapter 13 will say
it again: lose the key file *and* the
password is not enough any more. A key file
is a key. Keep copies the way you keep
spare house keys — deliberately, in more
than one safe place.

## Creating a database with a key file

The lab did this end to end:

```console
$ printf 'demo-master-passphrase\ndemo-master-passphrase\n' \
    | keepassxc-cli db-create --set-password \
      --set-key-file vault-key.key keyvault.kdbx
...
Successfully created new database.

$ ls -l vault-key.key
-r-------- 1 root root 128 Oct 25 02:20 vault-key.key
```

`--set-key-file <path>` *generates* the key
file at that path while creating the
database — 128 bytes of random data here,
created readable by nobody but you. (The
flag can also point at an existing file:
its contents then *become* part of the key,
which is how some people use a photo or a
document as a key. Generated randomness is
the safer default; a file that might be
edited, re-saved or recompressed is a key
that changes under you.)

## Opening it: both, or nothing

```console
$ keepassxc-cli ls keyvault.kdbx
Enter password to unlock keyvault.kdbx: 
Error while reading the database: Invalid credentials were provided, please try again.
If this reoccurs, then your database file may be corrupt.
```

That is the **correct password**, typed
right, with no key file offered — and it
fails with exactly the message a wrong
password gets. KeePassXC does not say which
part was missing, and that silence is a
feature. Offer both:

```console
$ keepassxc-cli ls -k vault-key.key keyvault.kdbx
Enter password to unlock keyvault.kdbx: 
[empty]
```

(the new database still has no entries —
`[empty]` is success).

(`-k` works on every command, and on other
databases it sits alongside the password
prompt in the window's unlock screen —
Key File field, chapter 4's photograph.)

## Changing credentials later

`db-edit` re-sets a database's credentials.
The lab changed a password:

```console
$ printf 'demo-master-passphrase\nnew-lab-passphrase\nnew-lab-passphrase\n' \
    | keepassxc-cli db-edit --set-password other.kdbx
Enter password to unlock other.kdbx: 
Enter password to encrypt database (optional): 
Repeat password: 
Successfully edited the database.
```

(the piped lines are the current password
once, then the new one twice).
Afterwards the old password failed and the
new one worked — both directions checked.
The same command adds or removes a key file
(`--set-key-file` / `--unset-key-file`) and
hardware keys. Changing credentials does not
change the database's contents or its file
name; it re-wraps the same key. Do it when
a master password feels stale, shared with
one person too many, or typed into a
machine you no longer trust.

## Hardware keys (documented only)

A **hardware key** — a YubiKey is the common
one — answers a challenge from the database
each time it opens, so the key itself never
leaves the device and cannot be copied off
a disk. KeePassXC supports it in both
programs: the unlock screen's **Hardware
Key** field (photographed in chapter 4) and
the CLI's `-y` flag ("slot:serial" of the
key). Setting one up means enrolling the
key as a credential with `db-edit` or the
window's database security settings, ideally
enrolling **two** keys — one in daily use,
one in a drawer — because a single lost key
with no spare and no other credential is a
locked vault.

None of that was exercised in this guide's
lab: there was no hardware key in the
sandbox. It is described here from the
project's documentation, and nothing in the
chapters pretends otherwise. The same
honesty applies to the **quick unlock**
features on Windows and macOS (Windows
Hello, Touch ID), which re-open a recently
used database with biometrics — convenient,
platform-specific, and documented rather
than tested here.

Check: if you use a key file, you can state
where the original lives and where its
spare lives. If you can't, the key file is
currently a single point of failure wearing
a security costume — make the spare today.
