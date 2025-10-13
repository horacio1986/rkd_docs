# Errores Comunes

En esta sección destacamos algunos de los errores más frecuentes al utilizar el entorno de desarrollo.

***También invitamos a los usuarios de este framework a reportar cualquier incidente o error no contemplado aquí a través de nuestro Help Desk, para poder actualizar y revisar cada caso particular.***

* Primero que nada, queremos enfatizar que el repositorio de **ROCKETDOO** NO SE CLONA, sino que se HACE UN FORK. La única forma de utilizarlo es creando un nuevo repositorio a partir de la plantilla.

## Fallo al instalar *requirements.txt*

![rocketdoo-img-failreq](../img/rocketdoo-docs-fail-require.png)

Ante este error, el sistema recomienda trabajar con entornos virtuales de Python, lo cual es una excelente práctica. Sin embargo, si prefieres instalar los requisitos directamente en tu PC sin entorno virtual, puedes resolverlo con este comando:

~~~
pip3 install -r requirements.txt --break-system-packages
~~~

Si deseas usar un entorno virtual, realiza lo siguiente:

~~~
python3 -m venv venv && source venv/bin/activate && pip install -r requirements.txt
~~~

## Fallo al instalar ROCKETDOO

Problemas similares pueden ocurrir durante la instalación del framework:

![rocketdoo-img-fail2](../img/rocketdoo-docs-fail-rocketdoo.png)

Solución:

~~~
pip3 install rocketdoo --break-system-packages
~~~

## Lanzar ROCKETDOO desde el directorio incorrecto

Es común intentar ejecutar **ROCKETDOO** desde la raíz del proyecto; por ejemplo, como se muestra en la imagen, intentar lanzarlo desde la carpeta raíz del repositorio creado (*my-dev-on-odoo*) cuando en realidad debe ejecutarse dentro de la carpeta *rocketdoo*:

*my-dev-on-odoo/rocketdoo*

![rocketdoo-img-fail3](../img/rocketdoo-docs-fail-launch.png)

Para solucionar esto, presiona **CTRL + C** para cancelar la acción, luego cambia al directorio correcto y lanza nuevamente:

~~~
cd rocketdoo
rocketdoo
~~~

Si lo haces correctamente, deberías ver la pantalla de bienvenida de **ROCKETDOO** como la siguiente:

![rocketdoo-img-main](../img/rocketdoo-main.png)

---

Para asistencia adicional, por favor envía un ticket a nuestro [Help Desk](https://odoo.hdmsoft.com.ar/mesa-de-ayuda)
