# Private Repositories

If during the launch of **Rocketdoo** you decide to use a private repository and specify it to the launcher,  
you must follow the guidelines below.

---

## Why Use a Private Repository?

This functionality is designed for developments intended for a specific client or for commercial purposes.

As is well known, if a development is intended to be commercialized, it must be hosted in a private **GitHub** repository.

What Rocketdoo does is read the hidden folder `*.ssh/*` found in your system user's home directory,  
and display the SSH keys located there on screen.  
For this to work, you must have previously created one or more SSH keys and registered them as access keys in your GitHub account.

Once Rocketdoo reads and displays your SSH keys,  
you will choose which one to use.

Rocketdoo will enumerate all SSH keys found so you can select the appropriate one based on the assigned number.

---

## Are My SSH Keys Secure?

Rocketdoo never copies a private key into your project. It reads the names of the keys in `~/.ssh/`,
you pick one, and only that name is stored in the configuration — the key itself stays where it was.
Since 3.14.1 a key configured by name is resolved against `~/.ssh/`, which is where every Rocketdoo
wizard looks, so a deploy no longer fails with "SSH key not found" over a key that was there all
along.

> **The selector offers keys, and only keys (3.14.2).** It used to list everything in `~/.ssh/`,
> including `config`, `known_hosts` and `authorized_keys`. It now skips those, along with `.pub`
> files, `.ppk` files (PuTTY's format, which `ssh -i` cannot read) and dotfiles — matching
> case-insensitively on the name up to the first dot, so `known_hosts.old` and `config.bak` are
> filtered out too. Cancelling the selector no longer writes `~/.ssh/None` into your configuration.

Three places are worth knowing about:

- **`rkd pack` never ships your keys.** The `.ssh/` directory, `.rkd/secrets/` and every `*.env` are
  left out of the ZIP, and no symbolic link may enter it — a link pointing at a secrets file used to
  carry it out. The closing panel names what was excluded, which is the list the receiver rewrites
  with their own keys through `rkd unpack`.
- **A Docker build that needs a private repository uses your SSH agent**, through BuildKit's
  `--mount=type=ssh`. The key is never written into an image layer. Since 3.12 projects whose
  external repositories are all public HTTPS build without an agent at all.
- **Cloning Enterprise onto a VPS forwards the agent** rather than copying a key to the server, so
  no credential of yours is left behind on a machine you share.