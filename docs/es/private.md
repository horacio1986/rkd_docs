# Repositorios Privados

Si durante el lanzamiento de **Rocketdoo** decides usar un repositorio privado y se lo indicas al lanzador,  
deberás seguir las siguientes pautas.

## ¿Por qué usar un Repositorio Privado?

Esta funcionalidad está pensada para desarrollos que tú o tu empresa destinen a un cliente en particular o con fines comerciales.

Como es sabido, si un desarrollo está destinado a ser comercializado, este debe estar alojado en un repositorio privado de **GitHub**.

Lo que hace Rocketdoo es leer la carpeta oculta `*.ssh/*` que se encuentra en el directorio de tu usuario del sistema,  
y mostrar en pantalla las claves SSH que se encuentren allí.  
Para que esto funcione, debes haber creado previamente una o más claves SSH y haberlas registrado como claves de acceso en tu cuenta de GitHub.

Una vez que Rocketdoo lea y muestre tus claves SSH,  
serás tú quien elija cuál de ellas utilizar.

Rocketdoo enumerará todas las claves SSH encontradas para que puedas seleccionar la correspondiente según el número asignado.

## ¿Mis claves SSH son seguras?

Las claves SSH son una herramienta cifrada que nos permite conectarnos de forma remota a nuestros repositorios,  
y por lo tanto, brindan seguridad al trabajar con repositorios privados.

Frente a esta realidad, es importante que el desarrollador sepa que sus claves están seguras dentro de nuestro entorno de desarrollo automatizado.  
*Rocketdoo* se encarga únicamente de mapear tu clave SSH dentro del contenedor de desarrollo de Odoo (contenedor web).

Este contenedor, como sabemos, es local por defecto y **efímero**,  
lo que significa que toda la información que contiene es temporal y puede ser eliminada si se desea.

Además, esta información no se encuentra bajo seguimiento de Git, por lo tanto al hacer *commit* y *push*,  
la clave no se enviará al repositorio remoto.

Finalmente, si decides usar un repositorio privado, al terminar el lanzador del entorno de *Rocketdoo*,  
verás que se ha creado una carpeta `*.ssh/*` dentro de tu directorio de desarrollo,  
donde se almacenará la clave seleccionada.
