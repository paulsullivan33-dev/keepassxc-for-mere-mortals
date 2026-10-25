# Judging Passwords

A vault fills up with passwords of very
uneven quality: the generated ones are
mountains, the ones you invented in 2015
are molehills, and a few are probably the
*same* molehill in several places.
KeePassXC ships three judges. The lab ran
all three.

## `estimate`: one password, honestly scored

`keepassxc-cli estimate` feeds a password
to **zxcvbn**, the strength estimator that
scores by *how it would be guessed* —
dictionary words, names, dates, keyboard
patterns, leetspeak — rather than by which
character classes it ticks. Real output
from the lab:

```console
$ keepassxc-cli estimate hunter2
Length 7   Entropy 10.8   Log10 3.3

$ keepassxc-cli estimate 'S3cure!Demo'
Length 11   Entropy 30.8   Log10 9.3

$ keepassxc-cli estimate 'correct horse battery staple'
Length 28   Entropy 70.3   Log10 21.2
```

Read the pattern: a famous weak password
scores 10.8 bits — nothing. Dressing a
dictionary word in leetspeak and punctuation
bought only about 20 more. Four plain words
beat both by miles. Length and
unpredictability are what count; the
estimator shows its working with `-a`,
which breaks the input into the patterns it
recognised. The lab's advanced run on the
password the generator had made for the bank
entry totalled **102.2 bits** — and still
itemised six segments, including a spatial
run (`ikj`), a repeat (`zz`), a
dictionary-plus-leet piece and a plain
dictionary fragment (`My`). Even in a random
string, zxcvbn finds the small patterns;
what it cannot find is a shortcut, and the
total is the verdict.

`estimate` judges passwords you *don't*
have yet, too — run candidates through it
before adopting one by hand. For passwords
already in the vault, the next two judges
work on the whole database at once.

## `db-info`: the vault's own census

Chapter 4 met `db-info` as a health page.
Its second half is a census of your password
hygiene:

```console
$ keepassxc-cli db-info vault.kdbx
...
Number of entries: 5
Unique passwords: 4
Non-unique passwords: 1
Maximum password reuse: 2
Number of short passwords: 2
Number of weak passwords: 4
Average password length: 12 characters
```

The lab had planted the crime on purpose —
two entries sharing `hunter2` — and the
census caught it exactly: one password not
unique, reused twice, and the short/weak
counts damning the fixtures' typed
passwords while the generated one passed
unnoticed. On a real vault, **Non-unique**
is the number to drive to zero first: a
reused password means one site's breach is
every site's breach. Fix by editing each
sharer to a fresh generated password
(chapter 5's `edit -g`).

In the window, the same census lives in
**Database → Database Reports…**, and the
sidebar's standing **Weak Passwords** view
lists the offenders directly (the lab saw
the view in the main window; the reports
screens themselves were not exercised).

## `analyze`: the leaked-password check

Have I Been Pwned (**HIBP**) collects
passwords from public breaches. `analyze`
checks your vault against that collection
**offline**: you download the list of
hashed passwords once, and the check never
sends anything anywhere.

Two honest frictions, both met by the lab.
First, without the list it simply stops:

```console
$ keepassxc-cli analyze vault.kdbx
Cannot find HIBP file:
```

Second, the list is a particular format —
one `SHA1:count` pair per line — and the
lab's first hand-made file of bare hashes
was rejected with a **parse error** before
the correctly shaped one worked. With
`-H` pointing at a proper list:

```console
$ keepassxc-cli analyze -H hibp-mini.txt vault.kdbx
Evaluating database entries against HIBP file, this will take a while…
Password for 'Web/Shared Weak B' has been leaked 9542 times!
Password for 'Web/Shared Weak A' has been leaked 9542 times!
```

The lab's list was a two-line stand-in it
wrote itself (so "9542" is the fixture's
own number, quoted to show the shape of the
verdict). The real list is a large download
from haveibeenpwned.com — tens of
gigabytes in its full form is the honest
scale; fetch it on a connection that
doesn't mind, then keep it around and
re-run `analyze` after each refresh. Any
entry it names gets a new generated
password, starting with the ones you reuse.

Check: run `db-info` on your vault and read
the statistics block. Non-unique at zero,
weak at zero, and an `analyze` with no
leaks is a vault in good shape — and now
you know how to prove it rather than hope
it.
