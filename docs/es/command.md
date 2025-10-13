# Línea de Comandos

Aquí tienes los comandos más comunes que puedes usar en **ROCKETDOO**.

* Iniciar el framework:

~~~
rocketdoo
~~~

* Levantar el entorno una vez finalizado el lanzador:

~~~
docker compose up
~~~

* Levantar el entorno en modo desacoplado (sin mostrar los logs en la terminal):

~~~
docker compose up -d
~~~

* Conceder permisos para usar Docker sin necesidad de *sudo*:

~~~
sudo groupadd docker
~~~

~~~
sudo usermod -aG docker $USER
~~~

Después de esto, deberías poder ejecutar Docker sin *sudo*.  
Si no funciona de inmediato, prueba con:

~~~
sudo newgrp docker
sudo usermod -aG docker $USER
~~~

* Pausar todos los contenedores:

~~~
docker compose pause
~~~

* Reanudar todos los contenedores:

~~~
docker compose unpause
~~~

* Detener todos los contenedores:

~~~
docker compose stop
~~~

* Pausar o detener un contenedor específico:

~~~
docker container pause <nombre-del-contenedor> 
docker container stop <nombre-del-contenedor>
~~~

* Reiniciar los contenedores:

~~~
docker compose restart
~~~

* Eliminar los contenedores:

~~~
docker compose down
~~~

* Eliminar contenedores y sus volúmenes asociados:

~~~
docker compose down -v
~~~

* Forzar la reconstrucción del entorno:

~~~
docker compose up --build
~~~

* Ver el estado de los contenedores:

~~~
docker compose ps
~~~

* Listar las imágenes Docker disponibles:

~~~
docker image ls
~~~

* Eliminar una imagen Docker:

~~~
docker rmi <nombre-de-la-imagen>
~~~

* Limpiar tu sistema eliminando imágenes y datos Docker que ya no uses:

~~~
docker system prune
~~~

* Actualizar masivamente los paquetes de módulos cargados como repositorios públicos con Gitman:  
(Este comando debe ejecutarse dentro del contenedor.)

~~~
gitman update
~~~
