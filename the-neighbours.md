# The Neighbours

KeePassXC is one point in a crowded field.
This chapter maps the neighbours fairly —
several are excellent — so the choice is a
choice, not a default. The recurring trade
is always the same: **who holds the file,
and how does it get to your other
machines?**

| Tool | Where the vault lives | Shape of it |
|---|---|---|
| KeePassXC | Your file, your machines | This guide |
| KeePass 2.x | Your file | The original, Windows-first |
| KeePassDX / Strongbox / AuthPass | Your file | Phone and cross-platform apps over KDBX |
| Bitwarden | Their server (or yours) | An account; sync built in |
| Vaultwarden | Your server | The self-hosted Bitwarden-compatible option |
| 1Password | Their server | Commercial, polished, subscription |
| pass / gopass | Your files, GPG-encrypted | One file per password, git-friendly |

## The same-format family

**KeePass 2.x** is the original Windows
program whose format KeePassXC descends
from; the two open each other's databases.
It is Windows-centric (Mono elsewhere) and
plugin-driven. **KeePassDX** (Android),
**Strongbox** (Apple platforms) and
**AuthPass** (several platforms) read and
write the same KDBX files — which is the
ecosystem's quiet superpower: KeePassXC on
the desktop and a phone app of your choice
over one file, carried by the sync tool of
your choice, beats most single-vendor
stories. If this guide's design appeals
but a platform feels unserved, look inside
the family before looking outside it.

## The account-and-server school

**Bitwarden** holds an encrypted copy of
your vault on its servers; you sign in
anywhere and sync is simply *there*. The
encryption is end-to-end, the clients are
open source, and the free tier is real.
**Vaultwarden** is the community's
self-hosted server speaking the same
protocol, for people who want that shape
without that landlord (a sibling guide in
this series covers running it). **1Password**
is the commercial polish leader of the same
school — strong family and team features,
a subscription, no self-hosting.

What you gain over KeePassXC: sync and
sharing as features, not disciplines —
inviting a person to *some* entries, not
the whole file; recovery flows through the
account. What you give up: the "no server,
no account, nothing to breach in bulk"
property, and a measure of format
portability. Neither answer is wrong; they
are different answers to "whose computer
should this live on".

## The files-and-git school

**pass** (and its friendlier front end
**gopass**) stores each password as its own
GPG-encrypted file in a folder you can keep
in git. It is the Unix-philosophy entry:
superb for scripts and servers, austere for
everyone else, and its security rests on
your GPG key rather than a vault password.
If chapter 9's command line felt like home
and the window felt like overhead, this
school deserves a look.

## Choosing, in four questions

1. **Must it sync without you thinking?**
   The server school, honestly.
2. **Must it work with no account and no
   server at all?** KeePassXC or pass.
3. **Sharing with granular permissions?**
   Bitwarden or 1Password; KeePassXC
   shares whole files only.
4. **Phones in the picture?** Either school
   serves them; KeePassXC does it through a
   family app over your synced file.

KeePassXC's bet — one standard file,
excellent tools, your discipline for the
rest — suits people who like owning their
infrastructure. This guide was written for
exactly them, by testing exactly that.
