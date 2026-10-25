# Auto-Type and Browsers

> **Documented, not lab-tested.** There was
> no second application in the sandbox for
> KeePassXC to type into, and no browser to
> integrate with. This chapter describes
> both features from the project's
> documentation and what the lab could see
> (the Autotype tab exists on every entry —
> it is in chapter 4's photograph). Nothing
> here is presented as measured.

So far you have fetched passwords *out* of
the vault by hand: unlock, look, type or
paste. Two features close that last gap by
having KeePassXC deliver the password for
you. They solve the same problem two
different ways, and choosing between them
is mostly about how you browse.

## Auto-Type: the program types

**Auto-Type** simulates keystrokes. You put
the cursor in a sign-in form, press a
global hotkey, KeePassXC finds the entry
whose title or URL matches the window you
are in, and it "types" username, Tab,
password, Enter — as if you were a very
fast, very accurate typist.

Per entry, the **Autotype tab** (visible in
the lab's main-window screenshot) holds the
sequence, which is programmable:
`{USERNAME}{TAB}{PASSWORD}{ENTER}` is the
default, and forms with odd tab orders, extra
fields, or two-step sign-ins get custom
sequences and delays. Matching can use the
window's title, and entries can be kept out
of Auto-Type entirely.

Its virtues and risks are the same property:
it types into **whatever window has focus**.
The honest cautions:

- Check the window before the hotkey. A
  password typed into a chat box is a
  password posted.
- KeePassXC asks rather than types when
  several entries match — read the prompt.
- Two-channel obfuscation (splitting the
  password into pieces typed and pasted
  out of order) exists to frustrate simple
  keyloggers; the documentation describes
  it and its limits. It is a speed bump for
  malware, not a wall.

## Browser integration: the extension asks

**KeePassXC-Browser** connects the desktop
program to a browser extension (for
Firefox, Chrome and their relatives). The
extension recognises sign-in pages; the
program, once you authorise the pairing,
fills credentials from matching entries —
matched by the entry's URL. You click the
field, the extension asks the (unlocked)
database, and the form fills.

The pairing, and the trust decisions around
it, are the substance:

- The browser and KeePassXC authenticate to
  each other with keys stored on your
  machine at first setup. Nothing leaves
  the machine; there is still no server.
- The database must be **unlocked** for
  filling to work — the extension cannot
  open it for you beyond prompting.
- URL matching is the security boundary:
  the entry for `bank.example.com` is
  offered on that host, not on look-alikes.
  Keep entry URLs accurate (chapter 5) and
  be suspicious of fills offered anywhere
  unexpected.
- Group and entry settings can require
  manual confirmation per fill. For a bank,
  that friction is a feature.

Setup per browser is a handful of steps in
the project's documentation — enable browser
integration in KeePassXC's settings, install
the extension, connect, name the pairing.
This guide deliberately does not reproduce
click-by-click for software it did not run.

## Two smaller integrations, for the record

- **SSH agent**: KeePassXC can hold SSH keys
  as entry attachments and serve them to
  your SSH client while the database is
  unlocked, so keys are encrypted at rest
  and loaded on demand.
- **Secret service**: on Linux, KeePassXC
  can act as the system's credential store
  (the freedesktop Secret Service), so other
  applications' secrets live in your vault.

Both are real, both are settings away, and
both are documented-only here like the rest
of the chapter.

## Choosing

Auto-Type works anywhere — browsers, SSH
prompts, installers, that one legacy app —
and needs no one's permission but yours.
Browser integration is smoother inside the
browser and stricter about matching, at the
cost of a moving part in each browser. Many
people run both: the extension for the web,
the hotkey for everything else. And the
manual way from chapters 5 and 9 still
works everywhere, which is worth more than
it sounds the first time an integration
misbehaves.

Check: you can say which of your entries
you would *exclude* from any automatic
filling — the master-password-protected few
whose compromise would hurt most. That
instinct, set entry by entry, is the
feature used well.
