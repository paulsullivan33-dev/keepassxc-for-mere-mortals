# Installing KeePassXC

KeePassXC is ordinary software: a package
on Linux, an installer on Windows and
macOS. Install it the way your system
installs things, from your system's own
sources — password managers are exactly the
software you do not fetch from random
download sites.

## Linux

On Debian and Ubuntu:

```console
$ sudo apt update
$ sudo apt install keepassxc
```

On Fedora it is `sudo dnf install
keepassxc`; on Arch, `sudo pacman -S
keepassxc`. The package brings both
programs: the window (`keepassxc`) and the
command line (`keepassxc-cli`). The lab
installed this way on Ubuntu 24.04 and got
version **2.7.6**.

Official Flatpak and AppImage builds also
exist. Prefer your distribution's package
unless you have a reason; it keeps updates
arriving with everything else.

## Windows and macOS (documented)

The project publishes an installer for
Windows and a disk image for macOS from
keepassxc.org. Neither was run in this
guide's lab, so no screenshots are offered
for them; the databases they create are the
same files, and everything from chapter 4
onward is identical once installed. The
command line is present on both: on Windows
it is `KeePassXC-CLI.exe` in the install
folder; on macOS it lives inside the app
bundle.

## Proving the install

The command line answers without any
display, database or fuss:

```console
$ keepassxc-cli --version
2.7.6
```

That is the lab's real output
(`keepassxc-lab/transcripts/t00-version-and-commands.txt`,
which also lists the CLI's 26 commands).

The graphical binary is fussier, and
meeting its refusal now saves confusion
later: with no display available it exits
with a Qt platform error —

```console
$ keepassxc --version
qt.qpa.xcb: could not connect to display
```

— which simply means "I am a window and
there is no screen". On a normal desktop it
opens instead. (The trick the lab used to
photograph it headlessly — a virtual
display — is a lab technique, not something
a reader needs.)

Check: `keepassxc-cli --version` prints a
version, and (on a desktop) `keepassxc`
opens a window offering to create or open a
database. Chapter 4 creates one.
