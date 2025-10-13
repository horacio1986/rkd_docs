# Welcome to ROCKETDOO

![rocketdoo-welcome](../img/banner.png)

Rocketdoo is a Python-based framework designed to provide a fast and efficient development environment.

With Rocketdoo, you can easily deploy one or multiple environments for developing in Odoo, across all editions and versions.  
It allows you to create new modules or functionalities for both the Enterprise and Community editions.

## Overview

This tool was built for developers who are either new to Odoo or experienced professionals looking to quickly set up their environments and focus solely on creating new modules and features.

It’s important to note that Rocketdoo is just one tool among many for Odoo development. It doesn’t claim to be the only or the best option — rather, it aims to be a helpful solution that addresses common development needs, while streamlining and automating the developer’s workflow.

We recognize that Odoo, as an ERP system, is vast and complex. Depending on the client, their needs, and the localization required, there are often additional requirements beyond the code itself — such as Python libraries, language-specific dependencies, third-party modules, or external repositories needed to ensure a successful deployment.

Managing all these dependencies and configurations can often be a real headache for developers.

That’s why Rocketdoo was created: to provide an intuitive and automated environment that simplifies the setup process for individual developers or entire teams. It reduces the overhead of launching a development environment and lets you focus on what matters — building new modules and features.

## Description

To better understand what Rocketdoo is and how to use it effectively, let’s go over the essential tools and how they are used.

First, it’s important to note that Rocketdoo was designed for Linux-based operating systems, such as Ubuntu or Debian. We believe Linux is the most suitable platform for working with this framework for several reasons.

Rocketdoo relies on the following tools to function properly:

* Docker and Docker Compose  
* Git and GitHub (or any version control system)  
* SSH key for managing private repositories  
* Gitman  
* Python and its package manager `pip`  
* A CLI terminal  
* Visual Studio Code  
* Essential Visual Studio Code extensions (we’ll list recommended ones later)

This set of tools — especially Docker and Docker Compose — is why we recommend using Rocketdoo on a Linux-based system.

However, we understand that many developers prefer Windows. In that case, we strongly recommend using the **Windows Subsystem for Linux version 2 (WSL2)**.
