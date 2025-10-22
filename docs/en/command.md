# Command Line Interface

Here are the most common commands you can use in **RKD** **ROCKETDOO**.

* Display Rocketdoo version:

~~~
rocketdoo --version
~~~

* Display help and available commands:

~~~
rocketdoo --help
~~~

* Generate the directory and file structure:

~~~
rocketdoo scaffold
~~~

* Launch the initialization wizard:

~~~
rocketdoo init
~~~

* Deploy Odoo:

~~~
rocketdoo up -d 
~~~

* Check the status of your containers:

~~~
rocketdoo status
~~~

* Stop all containers:

~~~
rocketdoo stop
~~~

* Pause or stop a container:

~~~
rocketdoo stop
~~~

* Restart the containers:

~~~
rocketdoo restart
~~~

* Remove containers:

~~~
rocketdoo down
~~~

* Remove containers and their associated volumes:

~~~
rocketdoo down -v
~~~

* Force environment rebuild:

~~~
rocketdoo build
~~~

* Build a new version of the same image

~~~
rocketdoo build --rebuild
~~~

* Delete Identifier type files when copied from Windows to WSL2

~~~
rocketdoo del -i
~~~

> YOU CAN ALSO COMBINE A COMMAND WITH **--help** TO LEARN ABOUT FLAGS AND OPTIONS
> FOR EXAMPLE ***rocketdoo build --help*** WILL SHOW YOU AVAILABLE OPTIONS LIKE: ***rkd build --rebuild***

* Bulk update module packages loaded as public repositories using Gitman:  
(This command must be executed inside the container.)

~~~
gitman update
~~~