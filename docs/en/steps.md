## Step by Step

### Step 1:

First, navigate to the **ROCKETDOO** repository <a href="https://github.com/HDM-soft/rocketdoo" target="_blank">here</a>

It is crucial to understand that this repository is configured as a **template**, so it should be used as such. Please do **not** clone or fork it directly.

### Step 2:

Use the template to create your own development repository by clicking the *Use this template* button, then selecting *Create new repository*.

This will take you to your GitHub account where you can start setting up your development repository.

![rocketdoo-docs-img](../img/rocketdoo-docs-paso2.png)

### Step 3:

Within your account, give your development project a name and optionally add a description.

Be sure **not** to select *Include all branches*, since you only need the *main* branch for development.

Once ready, choose whether your repository will be public or private, then click the *Create repository* button.

![rocketdoo-docs-img](../img/rocketdoo-docs-paso3.png)

### Step 4:

After your repository is created, clone it by copying either the *HTTPS* or *SSH* URL.

![rocketdoo-img-4](../img/rocketdoo-docs-paso4.png)

![rocketdoo-img-4/1](../img/rocketdoo-docs-paso4-1.png)

### Step 5:

Navigate into your newly created and cloned repository using the command:

~~~
cd my-dev-on-odoo
~~~

Once inside, list the files with:

~~~
ls
~~~

![rocketdoo-img-5](../img/rocketdoo-docs-paso5.png)

### Step 6:

Make sure the **ROCKETDOO** package is installed. If not, install it by running:

~~~
sudo pip install rocketdoo
~~~

*(If you encounter errors during installation, please refer to the "Common Mistakes" section for troubleshooting.)*

### Step 7:

Install the dependencies listed in the ***requirements.txt*** file:

~~~
sudo pip install -r requirements.txt
~~~

### Step 8:

Change directory into the **rocketdoo** subfolder, so your path is ***my-dev-on-odoo/rocketdoo***:

~~~
cd rocketdoo
~~~

![rocketdoo-img-8](../img/rocketdoo-docs-paso8.png)

### Step 9:

Open Visual Studio Code in the current directory by running:

~~~
code .
~~~

This will launch VS Code in your development workspace.

![rocketdoo-img-9](../img/rocketdoo-docs-paso9.png)

---

🚫 **Hold on!**

We are not starting development in VS Code just yet. On the next page, we will begin with [Launching Rocketdoo](launch.md).
