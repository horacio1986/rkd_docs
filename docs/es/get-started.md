# Primeros Pasos

Antes de lanzar nuestro entorno, revisaremos las herramientas que necesitas para comenzar sin complicaciones.

## Herramientas Necesarias

1. Sistema Operativo:  
    - Linux, distribuciones recomendadas: “Ubuntu” o “Debian”.  
    - Windows 10/11 (más adelante explicaremos cómo usar Rocketdoo con Windows).  
2. Terminal CLI  
3. Python instalado (se recomienda usar entornos virtuales para mayor seguridad).  
4. Tener instalado GIT en la terminal.  
5. Una cuenta en GITHUB  
6. Generar una clave SSH en tu equipo y configurarla en tu cuenta de GITHUB.  
7. Instalar Docker y Docker Compose  
8. GITMAN (ya viene instalado por defecto en el contenedor).  
9. Instalar Visual Studio Code  
10. Extensiones necesarias para Visual Studio Code:  
    - Dev Container  
    - Docker  

Estas son las extensiones básicas y fundamentales. Más adelante recomendaremos otras que también serán muy útiles.

## Observaciones

Aunque esta lista de herramientas pueda parecer intimidante para un desarrollador con poca experiencia, ¡no te preocupes!  
Nuestro entorno automatizado “Rocketdoo” gestionará todo esto por vos, de forma eficaz y eficiente, sin necesidad de que sepas cómo usar cada herramienta en profundidad.

Solo necesitás tener conocimientos básicos sobre herramientas como Git, GitHub y Visual Studio Code.

Obviamente será un plus si sabés cómo manejar Docker, pero no es obligatorio, y lo comprobarás al seguir los próximos pasos.

## ¿Por qué usar Docker?

Docker es una herramienta estándar en empresas de desarrollo de software que comprenden la importancia de trabajar en entornos controlados y preparados, donde todo funcione correctamente.

Trabajar con Docker nos da la tranquilidad de que nuestras aplicaciones funcionarán correctamente en cualquier entorno y navegador, tanto del lado del desarrollador como del cliente.

Nos evita la clásica frase:

> “En mi máquina sí funciona”.

Además de esto, utilizaremos las imágenes oficiales de Odoo proporcionadas por la propia empresa, que ya incluyen todos los requisitos básicos para que Odoo funcione de manera correcta.

Otro beneficio es que Docker nos proporciona un entorno de desarrollo efímero, es decir, fácil de crear y descartar.  
Gracias a Docker y Docker Compose, podremos levantar nuestro entorno con un simple comando, pausarlo cuando no lo necesitemos y continuar más adelante desde el mismo punto donde lo dejamos, sin consumir recursos innecesarios de nuestra PC.

¿No es genial?

Por último, vale destacar que Docker Compose creará un contenedor para Odoo (web) y otro para la base de datos (PostgreSQL), manteniéndolos aislados y permitiéndonos pausarlos, reiniciarlos o detenerlos de forma independiente.

### Guía para instalar Docker y Docker Compose

<a href="https://docs.docker.com/engine/install/ubuntu/" target="_blank">Guia Oficial Docker</a>

## ¿Qué es GITMAN y por qué usarlo?

Gitman es una herramienta que permite gestionar, a través de un archivo de tipo YAML (`yml`), todos los repositorios públicos o de terceros que puedan ser necesarios para nuestros desarrollos.

Con Gitman podrás tener listos todos los repositorios externos necesarios, sin necesidad de descargarlos manualmente ni moverlos a tu directorio de trabajo.

Simplemente copiando y pegando los enlaces de los repositorios necesarios, Rocketdoo te preguntará de forma dinámica con qué versión de Odoo querés trabajar para cada uno de ellos, sin que tengas que preocuparte por moverte entre ramas o versiones.

Para quienes trabajan con Odoo SH o están familiarizados con submódulos, Gitman ofrece una experiencia muy similar.

Además, no necesitás instalar Gitman en tu PC, ya que se instalará automáticamente dentro del contenedor de desarrollo cuando se inicie Rocketdoo.

Como extra, Rocketdoo no solo instalará Gitman y clonará todos los repositorios necesarios, sino que también detectará automáticamente archivos `requirements.txt` dentro de cada uno e instalará sus dependencias, evitando pérdidas de tiempo y errores por falta de librerías, algo que los desarrolladores sufrimos a menudo.

¡Rocketdoo también viene a solucionar eso!

Gitman es sin dudas una herramienta esencial para trabajar de forma rápida y eficiente, especialmente porque en muchos países las localizaciones no están incluidas o completas en el core de Odoo, y es necesario incorporar desarrollos de terceros que cubran estas funcionalidades.

Un ejemplo claro es el módulo completo de contabilidad de Odoo, que no viene incluido en la edición Community y que muchos desarrolladores resuelven utilizando el repositorio de “Odoo Mates”.

Si querés conocer más sobre Gitman, te recomendamos visitar su documentación oficial:

<a href="https://gitman.readthedocs.io/en/latest/" target=" blank">Guia Oficial Gitman</a>

## ¿Linux o Windows?

Como mencionamos al principio de esta documentación, este entorno fue diseñado y recomendado para ser utilizado en sistemas operativos Linux.

Y no es un capricho, sino una realidad: Linux consume menos recursos del sistema y es el entorno donde Docker funciona de forma óptima, lo cual no siempre ocurre en Windows.

Sin embargo, no hemos olvidado el sistema operativo más popular del mundo.  
¡Sí, también pensamos en Windows!

Bueno, técnicamente no del todo... pero podés usar **Rocketdoo** instalando WSL2 (Windows Subsystem for Linux) en tu sistema Windows.

Veremos cómo hacerlo en la siguiente sección.
