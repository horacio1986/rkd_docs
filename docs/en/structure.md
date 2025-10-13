# Rocketdoo Structure

In this section, we’ll gain a clearer understanding of Rocketdoo’s structure. This will help us comprehend how it works behind the scenes and highlight the importance and usage of certain folders and files within Rocketdoo.

![rocketdoo-img-structure](../img/rocketdoo-docs-structure.png)

As shown in the image, there is a long list of folders and files that make up the complete Rocketdoo structure.  
However, we will focus on the folders and files that are most relevant for developers.

These directories should be familiar to any Odoo developer.

---

## /Addons

Let’s begin with the **addons** folder.

Once your environment is set up, whether you're working in Visual Studio Code or directly from the terminal, this is the folder where you’ll place your custom developments, including any standalone modules you may need.

You can either manually create a folder with the name of your module and add its contents, or use the **Docker** extension in VSCode. By selecting the *Attach Shell* option, you can open a terminal and navigate to the directory where modules are located:  
`/usr/lib/python/dist-packages/odoo/extra-addons`.

There, you can use Odoo’s **scaffold** command to generate a new module.

![rocketdoo-img-attach](../img/rocketdoo-docs-attach.png)

![rocketdoo-img-shell](../img/rocketdoo-docs-shell.png)

![rocketdoo-img-into-shell](img/rocketdoo-docs-into-shell.png)

![rocketdoo-img-shell2](../img/rocketdoo-docs-shell2.png)

> **Note:** The “addons” folder inside our development container is mapped as a volume to the “extra-addons” directory.  
> This is why you must navigate to that folder inside the container.  
> Remember, the original `addons` folder belongs to the Odoo core, which will be clearer once we start debugging inside the container.

To create a new module using the `scaffold` command, run:

~~~

odoo scaffold <my_module> .

~~~


This will create a module with the essential files and structure required to begin development in the current directory.

Once created, you’ll also see the new module appear in your working directory inside the **addons** folder in VSCode.

> **Reminder:** The `addons` folder in your working directory is referred to as `extra-addons` inside the Odoo container.

---

## /Config/odoo.conf

The **odoo.conf** file, located in the **/config/** directory, is especially important for experienced Odoo developers.

This file contains the full server-level configuration for the system.  
It handles the mapping of third-party and custom modules, the configuration of the master password, the path to the log file, and more.

That said, you typically won’t need to manually modify this file, as it is automatically generated and configured by the framework during startup.

---

## Enterprise

As you’ve seen, when launching **ROCKETDOO**, near the end of the process you are asked whether you want to develop with the Odoo Community or Enterprise edition.

If your answer is `"ce"` (Community Edition), no extra steps are required.

However, if your answer is `"ee"` (Enterprise Edition), it's important to ensure that the **Enterprise** folder is present in your working directory at the same level as the **addons** folder **before** launching Rocketdoo.

This allows the system to map the Enterprise modules correctly and configure the environment accordingly to support this edition of Odoo.
