# Windows y WSL2

Ciertamente no veremos paso a paso cómo instalar WSL2 en Windows, ya que internet está lleno de guías.  
Incluso la página oficial de Windows tiene una guía paso a paso sobre cómo instalar WSL2 en tu computadora.

<a href="https://learn.microsoft.com/en-us/windows/wsl/install" target="_blank">Guia Oficial WSL2</a>

Pero lo que sí abordaremos son algunas recomendaciones sobre cómo usar esta herramienta para poder desarrollar con Rocketdoo y VSCode.

Suponiendo que ya tienes instalado WSL2 en tu computadora, y que está correctamente configurado con acceso sudo para el usuario que creaste,  
continuamos con algunos pasos necesarios para tener tu máquina lista para desarrollar con Rocketdoo y VSCode.

Recuerda que puedes usar cualquier distribución de Linux,  
aunque para uso diario suponemos que Ubuntu 20/22/24.04 o Debian 11/12 deberían funcionar sin problema.

## Pasos necesarios:

1. Ingresar a la terminal de WSL2.
2. Actualizar los paquetes de Linux:

~~~ 
sudo apt update
~~~

3. Instalar git:

~~~
sudo apt install git -y
~~~

4. Instalar Visual Studio Code, dentro de WSL2:

~~~
sudo apt install code
~~~

5. Luego de haber instalado VSCode, con el comando:

~~~
code .
~~~
podrás abrir el software en cuestión.

6. Finalmente es importante verificar que tienes instalado “pip”, si no lo tienes, instálalo.

~~~
sudo apt install python-pip
~~~

* Ahora puedes continuar con los [Pasos](steps.md)

***Luego será necesario continuar con los pasos de la página de Instrucciones porque, una vez listas estas herramientas,  
los pasos de ejecución son los mismos.***
