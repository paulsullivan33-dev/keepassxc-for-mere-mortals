# Limits

What KeePassXC cannot do, or cannot do for
you. Collected in one place so none of it
surprises you later; several items were
measured in the lab and are marked as such.

- **No recovery.** A forgotten master
  password with no second credential means
  the vault is gone (chapter 13). This is a
  design property, and it is also a limit —
  both things are true.
- **No sync of its own.** The program opens
  files; moving and reconciling them is
  yours (chapter 11). Diverged copies are
  untangled by merge, and deletions do not
  travel by merge at all.
- **Sharing is all-or-nothing.** Anyone who
  can open the database sees every entry in
  it. There are no per-entry permissions,
  no view-only members, no audit log of who
  looked. Teams needing those need another
  tool (chapter 15).
- **An unlocked vault trusts its machine.**
  While the database is open, malware
  running as you can read what you can
  read. Auto-lock timers, clipboard clearing
  (measured at 10 seconds in the lab) and
  locking when you walk away shrink the
  window; they do not close it. A password
  manager is not a defence against a
  compromised computer.
- **The clipboard is a public place,
  briefly.** Anything copied can be read by
  other programs until it is cleared, and
  clipboard managers may keep their own
  history of it. KeePassXC clears what it
  copied; it cannot police what watched.
- **The command line imports XML only**
  (measured). CSV import needs the window,
  and the window needs a display (measured:
  the GUI binary refuses to start without
  one). Scripted migrations plan around
  both facts.
- **`show` will not print a password as a
  side effect** (measured). You must ask
  for the attribute by name — a limit that
  is also the correct default, listed here
  because everyone trips on it once.
- **The CLI creates KDBX 3.1 files**
  (measured), with the older AES KDF,
  unless a decryption time is specified;
  the file upgrades to 4.1 on later saves
  in some paths (the lab caught it at the
  first recycle) and the window creates 4.x
  natively. Contents are identical; mixed
  toolchains with very old KeePass programs
  are where this bites.
- **Auto-Type types into the focused
  window** (documented). The wrong window
  gets your password. Chapter 12's caution
  is a limit of the mechanism, not of your
  care alone.
- **Strength estimates are estimates.**
  `estimate`'s bits come from zxcvbn's
  model of guessing; `analyze` knows only
  the breaches in its list. Both are good
  instruments; neither is a guarantee, and
  a password absent from every known breach
  can still be a bad password.
- **One password still rules it all.** Key
  files and hardware keys (chapter 8) add
  factors, but the master password remains
  the thing a targeted attacker works on.
  Length is your armour — the tools make it
  easy (chapter 6) and cannot make it
  unnecessary.

None of these are secrets the project
keeps; most are the same list its own
documentation would give you. A tool whose
limits you can recite is a tool you can
trust with the rest.
