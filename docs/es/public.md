# Repositorios Públicos

Nuestro lanzador **ROCKETDOO** ofrece la opción de utilizar tanto repositorios privados como públicos.  
En esta sección nos enfocaremos en los repositorios públicos, los cuales resultan especialmente útiles para los desarrolladores,  
ya que permiten la instalación automática de directorios, dependencias y bibliotecas necesarias.

**ROCKETDOO** utiliza la herramienta *Gitman* para gestionar esta configuración. Durante el proceso, te pedirá:

- La URL del repositorio público  
- La versión (rama o etiqueta) que deseas usar  
- Un nombre personalizado para identificar el repositorio o paquete de módulos

Esta funcionalidad es especialmente útil al trabajar con paquetes de módulos de terceros  
que suelen incluir múltiples módulos interdependientes. Un ejemplo común son los complementos  
relacionados con contabilidad que extienden la funcionalidad de Odoo en su edición Community.

Un buen ejemplo de esto son los paquetes de *Odoo Mates*, que ofrecen varios módulos orientados a contabilidad.

![rocketdoo-img-odooapps](../img/rocketdoo-docs-odooapps.png)

Como se muestra en la imagen, *odooapps* es un paquete que contiene varios módulos  
que amplían funciones contables, muchos de ellos con dependencias entre sí.

Sin esta automatización, el desarrollador tendría que investigar manualmente qué módulos son necesarios,  
verificar sus dependencias, leer sus contenidos y revisar si contienen un archivo `requirements.txt`,  
haciendo la tarea larga y propensa a errores.

Una vez que se cargan los repositorios públicos deseados, **ROCKETDOO** se encarga de todo:  
genera y configura un archivo `gitman.yml` que almacena toda la información proporcionada durante la configuración.

A continuación, una imagen de ejemplo de un archivo Gitman generado:

![rocketdoo-img-gitman](../img/rocketdoo-docs-gitman.png)

En la imagen podemos observar tres paquetes de módulos diferentes,  
cada uno con su versión correspondiente de Odoo y un nombre descriptivo.

Estos paquetes estarán disponibles dentro del contenedor de Odoo,  
en la carpeta **external_addons**, ubicada en:  
`/usr/lib/python3/dist-packages/odoo/external_addons`.

El archivo `gitman.yml` se genera automáticamente al proporcionar la información de los repositorios durante el lanzamiento de Rocketdoo.

### Resumen

El uso de *Gitman* es especialmente útil cuando se trabaja con **paquetes de módulos**,  
en lugar de módulos individuales. Para un solo módulo, basta con clonarlo dentro de la carpeta **addons**,  
pero cuando se trata de paquetes complejos con múltiples dependencias, Gitman simplifica enormemente el proceso.
