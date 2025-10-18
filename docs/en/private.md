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

In this new version of Rocketdoo, there is no longer a need to consider hiding private SSH keys; since
rkd version 2 is not a repository itself, we do not need to be concerned about this.
Since you can use your private repositories normally, as they do not run the risk of being exposed in a repository.
However, in version 1, their protection was considered.