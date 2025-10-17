# Get Started

Before launching your development environment, let’s go over the tools you’ll need to get started confidently.

## Required Tools

1. Operating System:  
    - Linux, recommended distributions: “Ubuntu” or “Debian”.  
    - Windows 10/11 with WSL2 installed (later we will explain how to use Rocketdoo with Windows).  
2. CLI terminal of your choice. For a better experience, we recommend the Warp terminal, available  
   for Windows and Linux systems.  
3. Python installed (it is recommended to use virtual environments for better security). In addition, `pip` and/or `pipx`.  
4. Have GIT installed in the terminal.  
5. A GITHUB account.  
6. Generate an SSH key on your machine and configure it in your GITHUB account.  
7. Install Docker and Docker Compose.  
8. GITMAN (It will be installed by default when you start your development environment).  
9. Install Visual Studio Code.  
10. Required extensions for Visual Studio Code:  
    - Dev Container  
    - Docker  
    - Python  
    - Pylance  
    - XML Tools  
    - Odoo IDE  
    - Odoo Snippets  
    - WSL (If you are developing on Windows)  


## Notes

Although this list might seem overwhelming to less experienced developers — don’t worry!  
Rocketdoo automates and handles everything for you efficiently, so you don’t need to master every tool beforehand.

You only need a basic understanding of tools like Git, GitHub, and Visual Studio Code.

Knowing Docker is definitely a plus, but it’s not a requirement — you’ll see just how easy it is when we start the step-by-step setup.

## Why Use Docker?

Docker is a global standard used by software development teams who value consistency and reliability across environments.

With Docker, you can be confident that your development will behave the same way across different systems, operating systems, and browsers — both yours and your client’s.

It helps eliminate the dreaded phrase:

> “It works on my machine.”

In addition, we use official Docker images provided by Odoo, which already include all the basic requirements for running the platform properly.

Docker also provides ephemeral environments — temporary, isolated containers that can be easily spun up or removed.  
Using Docker and Docker Compose, you can launch your entire environment with a single command, pause it, and return later without wasting system resources.

Pretty cool, right?

Finally, Docker Compose will create separate containers: one for Odoo (web) and one for the PostgreSQL database.  
This isolation lets you stop, restart, or manage each service independently when needed.

### Docker & Docker Compose Installation Guide

<a href="https://docs.docker.com/engine/install/ubuntu/" target="_blank">Official Docker GUide</a>

## What is GITMAN and Why Use It?

Gitman is a tool that lets you manage all the public or third-party repositories your project might require via a simple YAML (`.yml`) file.

With Gitman, you can prepare all necessary third-party modules without manually downloading and copying them into your project folder.

Just paste the links to the required repositories, and Rocketdoo will dynamically ask you which Odoo version you want to use for each one.  
No need to navigate different branches or versions.

If you’ve used Odoo SH or Git submodules before, Gitman offers a similar experience.

Best of all — you don’t need to install Gitman on your machine. It comes pre-installed in the Rocketdoo development container.

As a bonus, Rocketdoo will also automatically detect `requirements.txt` files inside the repositories and install any listed Python dependencies — saving you from manually installing libraries and avoiding frustrating errors when starting a new Odoo database.

Rocketdoo takes care of that too!

Gitman is an essential tool for fast and flexible development — especially because some localizations and features aren’t available in Odoo’s Community core.  
This often requires external modules like the popular “Odoo Mates” accounting suite.

Want to learn more? Visit Gitman’s official documentation:

<a href="https://gitman.readthedocs.io/en/latest/" target=" blank">Official Gitman Guide</a>

## Linux or Windows?

As mentioned earlier, Rocketdoo is built and recommended for Linux environments.  
That’s not just a preference — Linux naturally consumes fewer system resources and provides optimal compatibility with Docker.

That said, we haven’t forgotten about Windows — the world’s most widely used OS.

Well, not exactly… but you can still use **Rocketdoo** by installing **WSL2 (Windows Subsystem for Linux v2)** on your Windows machine.

We’ll cover that in the next section.
