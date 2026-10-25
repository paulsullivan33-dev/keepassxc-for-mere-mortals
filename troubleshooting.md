# Troubleshooting

Symptom → likely cause → fix. Everything
marked *lab* was produced on purpose in
this guide's sandbox; the transcripts are
in `keepassxc-lab/transcripts/t90-errors.txt`
and friends.

| Symptom | Likely cause | Fix |
|---|---|---|
| "Invalid credentials were provided" | Wrong password — or right password, missing key file/hardware key | See below; the message deliberately won't say which (*lab*) |
| "Not a KeePass database" | The file isn't a database — often a CSV export | Open the `.kdbx`; imports start from chapter 10 (*lab*) |
| The window won't start; Qt mentions a display | No graphical session (SSH, a console) | Use `keepassxc-cli`, or run it on the desktop (*lab*) |
| `show` prints `Password: PROTECTED` even with `-s` | The summary never prints the password itself | `show -s -a Password <db> <entry>` (chapter 9) (*lab*) |
| `analyze` stops at "Cannot find HIBP file" | No hash list given, or none downloaded | `analyze -H <list>`; the list's lines must be `SHA1:count` — bare hashes give a parse error (*lab*) |
| CLI `import` rejects a CSV: "No root group" | The CLI imports XML only | Import CSV in the window: **Database → Import → CSV File…** (*lab*) |
| `clip` seems to do nothing on a server | No X clipboard to copy into | Run it where a desktop/clipboard exists; the lab used a virtual display (*lab*) |
| Entries you deleted are back after a merge | Deletions don't travel by merge; the other copy still had them | Delete in each copy, and empty each Recycle Bin (chapter 11) |
| An entry you deleted is still in `ls` | It's in the Recycle Bin — deleting recycles | Move it back out, or empty the bin in the window (*lab*) |
| Database opens on one machine, not another | The other copy is older/newer, or a different file entirely | Compare `db-info` (location, UUID, timestamps); merge rather than overwrite |
| "HMAC mismatch" appears in the error detail | The credentials don't match this file at all | Same fix path as invalid credentials; also check you have the right *copy* of the file (*lab*) |
| Password worked yesterday, fails today | Someone changed credentials (`db-edit`), or you're opening a stale copy | Check for a newer copy; a pre-change backup still opens with the old password (chapter 13) |
| Forgotten master password | — | No recovery exists. Older backups + old password, else reset accounts one by one (chapter 13) |
| The generator's passphrase words look odd | The wordlist is the EFF large list — deliberately ordinary words | That's the design; strength is in the count, not the vocabulary (chapter 6) |
| An export opens as readable text | Exports are plaintext by design | That's expected — and why chapter 10 says shred them after use |

## When the error is the generic one

Worth its own paragraph, because it will
happen to you: **"Invalid credentials were
provided, please try again."** is the only
answer KeePassXC gives for *every* failed
unlock — wrong password, missing key file,
unplugged hardware key, or a key file whose
contents changed (a key file that was
edited or re-saved by something "helpful"
is a different key). Work it in order:

1. Retype the password slowly; check Caps
   Lock and keyboard layout — the classic.
2. If the database uses a key file, point
   at it explicitly (`-k file`, or the Key
   File field) and confirm it is the
   original, not a copy that passed through
   a text editor or a re-encoding sync.
3. Try a backup copy of the database: if a
   *backup* opens with the same credentials,
   the live file is the problem (or the
   credentials were changed since — chapter
   8). If no copy opens, the credentials in
   your head are the stale part, and chapter
   13's honesty applies.

Check: most rows in this table end at a
chapter that shows the working version.
If yours doesn't, `keepassxc-cli help` and
the project's documentation are the next
stop — and the database file itself, copied
before experimenting, is the safety net
under all of it.
