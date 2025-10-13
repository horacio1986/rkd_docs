# Common Mistakes

In this section, we highlight some of the most frequent errors encountered when using the development environment.

***We also encourage users of this framework to report any incidents or bugs not covered here via our Help Desk, so we can update and review each case accordingly.***

* First and foremost, we want to emphasize that the **ROCKETDOO** repository is NOT CLONED but FORKED! The only way to use it is by creating a new repository from the template.

## Failed to Install *requirements.txt*

![rocketdoo-img-failreq](../img/rocketdoo-docs-fail-require.png)

When faced with this error, the system suggests using Python virtual environments, which is an excellent approach. However, if you prefer to install the requirements on your PC without a virtual environment, you can resolve it with the following command:

~~~
pip3 install -r requirements.txt --break-system-packages
~~~

If you want to use a virtual environment, execute:

~~~
python3 -m venv venv && source venv/bin/activate && pip install -r requirements.txt
~~~

## Failure to Install ROCKETDOO

Similar issues may occur during the framework installation:

![rocketdoo-img-fail2](../img/rocketdoo-docs-fail-rocketdoo.png)

Solution:

~~~
pip3 install rocketdoo --break-system-packages
~~~

## Launching ROCKETDOO from the Wrong Directory

It is common to attempt launching **ROCKETDOO** from the project's root directory; for example, as shown in the image, launching from the root of your created repository (*my-dev-on-odoo*) when it should actually be run inside the *rocketdoo* subfolder:

*my-dev-on-odoo/rocketdoo*

![rocketdoo-img-fail3](../img/rocketdoo-docs-fail-launch.png)

To resolve this, press **CTRL + C** to cancel the current action, then navigate to the correct directory and launch again:

~~~
cd rocketdoo
rocketdoo
~~~

If done correctly, you should see the **ROCKETDOO** welcome screen as below:

![rocketdoo-img-main](../img/rocketdoo-main.png)

---

For further assistance, please submit a ticket to our [Help Desk](https://odoo.hdmsoft.com.ar/mesa-de-ayuda)
