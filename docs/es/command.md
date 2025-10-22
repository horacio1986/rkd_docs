# Línea de Comandos

Aquí tienes los comandos más comunes que puedes usar en **RKD** **ROCKETDOO**.

* Version de Rocketdoo:

~~~
rocketdoo --version
~~~

* Ayuda y comandos disponibles:

~~~
rocketdoo --help
~~~

* Crear el arbol de directorios y archivos:

~~~
rocketdoo scaffold
~~~

* Iniciar el asistente:

~~~
rocketdoo init
~~~


* Lanzar el despliegue de Odoo:

~~~
rocketdoo up -d 
~~~

* Conocer el estado de tus contenedores:

~~~
rocketdoo status
~~~

* Detener todos los contenedores:

~~~
rocketdoo stop
~~~

* Pausar o detener un contenedor:

~~~
rocketdoo stop
~~~

* Reiniciar los contenedores:

~~~
rocketdoo restart
~~~

* Eliminar los contenedores:

~~~
rocketdoo down
~~~

* Eliminar contenedores y sus volúmenes asociados:

~~~
rocketdoo down -v
~~~

* Forzar la reconstrucción del entorno:

~~~
rocketdoo build
~~~

* Hacer un nuevo build de la misma imagen

~~~
rocketdoo build --rebuild
~~~

* Eliminar archivos del tipo Identifier cuando son copiads desde windows a WSL2

~~~
rocketdoo del -i
~~~

> TAMBIEN PUEDE COMBINAR UN COMANDO CON **--help** PARA CONOCER LOS FLAGS Y OPCIONES
> POR EJEMPLO ***rocketdoo build --help*** TE INDICAR LAS OPCIONES DISPONIBLES COMO: ***rkd build --rebuild*** 

* Actualizar masivamente los paquetes de módulos cargados como repositorios públicos con Gitman:  
(Este comando debe ejecutarse dentro del contenedor.)

~~~
gitman update
~~~
