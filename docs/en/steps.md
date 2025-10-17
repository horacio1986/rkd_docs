# Step by Step

### Step 1:

Verify you have Docker and Docker Compose installed, since it is a tool as important as **Rocketdoo** itself

### Step 2:

Install **Rocketdoo**

~~~~
pipx install rocketdoo
~~~~

### Step 3:

Install the requirements from the file ***requirements.txt*** if they were not installed by default:

~~~
sudo pip install -r requirements.txt
~~~

### Step 4:

Check the installed version to make sure it is version 2; the old version will remain available.

~~~~
rocketdoo --version
~~~~


![rocketdoo-docs-img](../img/rkd-versionado.png)

### Step 5:

Now you can create any directory you like to get started. However, it is recommended that your directory reference
a development intended for Odoo — I think that makes the most sense, right?


### Step 6:

Once your working directory is created, keep it empty and, once inside it, you can run your first command:

~~~~
rkd scaffold
~~~~
This command will create inside your working directory all the files and folders necessary to build your 
Odoo development environment and to debug code with VSCode.



![rocketdoo-img-5](../img/rkd-scaffold.png)

### Step 7:

Run the command to start the **Rocketdoo** wizard; it will guide you step by step through everything 
needed to configure your development environment.  
Follow the steps and answer every single question.

~~~
rkd init
~~~

![rocketdoo-img-5](../img/rkd-init.png)


### Step 8:

Once the wizard has finished you can verify how your environment was built with the command:

~~~~
rkd info
~~~~

![rocketdoo-img-5](../img/rkd-info.png)

### Step 9:

With the following command you will be able to deploy your development environment

~~~~
rkd up -d
~~~~

### Step 10:

Once the deployment finishes successfully you can access it with your preferred browser at the URL:
http://localchost:{puerto-odoo}


## NOTES 

> Rocketdoo can detect if the ports chosen for Odoo or for code debugging with Visual Studio Code are  
> already in use. If the selected ports are in use, the system will offer two alternative ports  
> or the option for you to choose new ports as you wish.

> If you made a mistake during the wizard, you can cancel with the key combination **CTRL + C** and start again  
> without major issues.


## Learn the available commands

With the following command you can see the help:

~~~~
rkd --help
~~~~

![rocketdoo-img-5](../img/rkd-help.png)
---

