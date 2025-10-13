# Launching ROCKETDOO

Now we can start ***ROCKETDOO*** by launching its CLI with the following command:

~~~
rocketdoo
~~~

This will open its interactive CLI screen. It will greet us, and after pressing ***ENTER***, it will begin asking for the necessary information to create our development environment.

![rocketdoo-main](../img/rocketdoo-main.png)

## What does Rocketdoo ask for?

The CLI will present a series of questions that help configure your development environment according to your needs:

* Odoo version  
* Project name  
* PostgreSQL version  
* Database container name  
* Master password  
* Name of the Docker image to build  
* Name of the Odoo web container  
* Odoo port  
* VS Code port  
* Whether the containers should auto-restart, and how

After that, you’ll be asked:

* Which Odoo edition you’re using: Community (“ce”) or Enterprise (“ee”)  
* If you’ll be using private repositories  
* If you’ll be using third-party repositories:
    * Repository URL  
    * Repository version  
    * Local name for that repository  
    * ...and whether you want to continue adding more third-party repositories

We won’t go through every question here, as Rocketdoo already provides helpful defaults and suggestions throughout the process.

---

## Tips

The following usage guidelines are important to keep in mind:

1. The names you assign to the Odoo web container, the PostgreSQL container, and your project should all be unique.  
   Do **not** reuse names if you’ve previously created another development project.

   If you repeat a name and try to launch the project, Docker will return an error stating that the containers already exist.

   In such a case, press ***CTRL + C*** to cancel the operation and re-run Rocketdoo.

2. Similarly, do not reuse the same Odoo or VS Code ports across multiple projects.  
   This ensures that you can run multiple environments at the same time without conflict.

   For example, if your first project uses port **8069**, you can use **8169** for the next one. The same rule applies to the VS Code port.

3. Once you're done working on a development environment, you can stop it or even remove all related resources using the following commands:

To stop the containers:
~~~
docker compose stop
~~~

To remove the containers and associated volumes:
~~~
docker compose down -v
~~~

This will free up the ports, making them available for other development environments.

---

***If you're working with Odoo Enterprise, make sure to place the *Enterprise* folder inside the working directory at the same level as *addons* before launching ROCKETDOO.***

---

## All Done!

After answering Rocketdoo's questions, you can launch your environment by running:

~~~
docker compose up
~~~

*Note: This step may take anywhere from 1 to 8 minutes depending on the number of third-party repositories you've added.
During this time, Rocketdoo will clone the repositories, copy them into the web container, install all required dependencies, and download the official Odoo and PostgreSQL Docker images.*

Once the setup is complete and the Odoo logs are running, you can access your Odoo instance by visiting:

~~~
http://localhost:8069
~~~

From there, you can create your first database, install the required modules, and start working with the third-party packages you selected during setup.
