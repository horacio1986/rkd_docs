# Repositorios Privados

Si durante el lanzamiento de **Rocketdoo** decides usar un repositorio privado y se lo indicas al lanzador,  
deberás seguir las siguientes pautas.

---

## ¿Por qué usar un Repositorio Privado?

Esta funcionalidad está pensada para desarrollos que tú o tu empresa destinen a un cliente en particular o con fines comerciales.

Como es sabido, si un desarrollo está destinado a ser comercializado, este debe estar alojado en un repositorio privado de **GitHub**.

Lo que hace Rocketdoo es leer la carpeta oculta `*.ssh/*` que se encuentra en el directorio de tu usuario del sistema,  
y mostrar en pantalla las claves SSH que se encuentren allí.  
Para que esto funcione, debes haber creado previamente una o más claves SSH y haberlas registrado como claves de acceso en tu cuenta de GitHub.

Una vez que Rocketdoo lea y muestre tus claves SSH,  
serás tú quien elija cuál de ellas utilizar.

Rocketdoo enumerará todas las claves SSH encontradas para que puedas seleccionar la correspondiente según el número asignado.

---

## ¿Mis claves SSH son seguras?

En esta nueva version de Rocketdoo, ya no es necesario considerar ocultar las llaves privadas ssh; dado que
rkd version 2, al no ser un repositorio como tal, no necesitamos preocuparnos por esto.
Dado que puedes usar tus repositorios privados con normalidad, ya que no corren riesgos de estar expuestas en repositorio.
Sin embargo en la version 1, esto estaba contemplada la proteccion de las mismas
