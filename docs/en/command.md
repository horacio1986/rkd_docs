# Command Line

Here are the most common commands you can use in **ROCKETDOO**.

* Start the framework:

~~~
rocketdoo
~~~

* Start the environment after the launcher finishes:

~~~
docker compose up
~~~

* Start the environment in detached mode (without showing logs in the terminal):

~~~
docker compose up -d
~~~

* Grant Docker permissions without using *sudo*:

~~~
sudo groupadd docker
~~~

~~~
sudo usermod -aG docker $USER
~~~

After this, you should be able to run Docker commands without *sudo*.  
If it doesn't work immediately, try:

~~~
sudo newgrp docker
sudo usermod -aG docker $USER
~~~

* Pause all containers:

~~~
docker compose pause
~~~

* Unpause all containers:

~~~
docker compose unpause
~~~

* Stop all containers:

~~~
docker compose stop
~~~

* Pause or stop a specific container:

~~~
docker container pause <container-name> 
docker container stop <container-name>
~~~

* Restart containers:

~~~
docker compose restart
~~~

* Remove containers:

~~~
docker compose down
~~~

* Remove containers and their associated volumes:

~~~
docker compose down -v
~~~

* Force a rebuild of the environment:

~~~
docker compose up --build
~~~

* Check the status of your containers:

~~~
docker compose ps
~~~

* List all Docker images:

~~~
docker image ls
~~~

* Remove a Docker image:

~~~
docker rmi <image-name>
~~~

* Clean your system by removing unused Docker images and data:

~~~
docker system prune
~~~

* Update all module packages loaded as public repositories using Gitman:  
(This command must be run inside the container.)

~~~
gitman update
~~~
