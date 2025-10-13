# Private Repositories

If, during the launch of **Rocketdoo**, you choose to use a private repository and indicate this to the launcher,  
you’ll need to follow the guidelines below.

## Why Use a Private Repository?

This functionality is intended for developments created for a specific client or for commercial purposes.

As you may know, if a development is intended for commercialization, it must reside in a **private** GitHub repository.

Rocketdoo helps with this by reading the hidden `*.ssh/*` folder in your system user directory and displaying any available SSH keys it finds there.  
To use this functionality, you must have already created one or more SSH keys and added them as access keys in your GitHub account.

Once Rocketdoo reads and displays your SSH keys,  
you’ll be prompted to choose which one to use.

It will list and number all your available SSH keys so that you can select the appropriate one by its corresponding number.

## Are My SSH Keys Secure?

SSH keys are a secure, encrypted method for connecting remotely to your repository.  
They provide a robust layer of protection when working with private repositories.

It’s important for developers to know that their keys are safe within our automated development environment.  
*Rocketdoo* maps your SSH key only inside the Odoo development container (the web container).

This container is local by default, and each container is **ephemeral**—meaning all data inside it is temporary and can be discarded at any time.

Additionally, SSH key data is not tracked by Git. So when you commit and push code, this information is **not** sent to your remote repository.

Lastly, if you choose to use a private repository, Rocketdoo will create an `*.ssh/*` folder inside your development directory,  
where the selected SSH key will be stored after the launcher finishes setting up your environment.
