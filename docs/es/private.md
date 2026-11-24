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

Rocketdoo nunca copia una clave privada dentro de tu proyecto. Lee los nombres de las claves de
`~/.ssh/`, vos elegís una, y en la configuración queda guardado sólo ese nombre — la clave se queda
donde estaba. Desde la 3.14.1 una clave configurada por nombre se resuelve contra `~/.ssh/`, que es
donde miran todos los asistentes de Rocketdoo, así que un deploy ya no falla con "SSH key not found"
por una clave que estuvo ahí todo el tiempo.

> **El selector ofrece claves, y sólo claves (3.14.2).** Antes listaba todo lo que hubiera en
> `~/.ssh/`, incluidos `config`, `known_hosts` y `authorized_keys`. Ahora los saltea, junto con los
> archivos `.pub`, los `.ppk` (el formato de PuTTY, que `ssh -i` no lee) y los dotfiles — comparando
> sin distinguir mayúsculas y sobre el nombre hasta el primer punto, así que `known_hosts.old` y
> `config.bak` también quedan afuera. Cancelar el selector ya no escribe `~/.ssh/None` en tu
> configuración.

Hay tres lugares que conviene conocer:

- **`rkd pack` nunca se lleva tus claves.** El directorio `.ssh/`, `.rkd/secrets/` y todo `*.env`
  quedan afuera del ZIP, y ningún enlace simbólico puede entrar — un enlace apuntando a un archivo de
  secretos lo sacaba del proyecto. El panel final nombra lo que excluyó, que es la lista que el
  receptor reescribe con sus propias claves a través de `rkd unpack`.
- **Un build de Docker que necesita un repositorio privado usa tu agente SSH**, por el
  `--mount=type=ssh` de BuildKit. La clave nunca se escribe en una capa de la imagen. Desde la 3.12
  los proyectos cuyos repos externos son todos HTTPS públicos construyen sin agente.
- **Clonar Enterprise en un VPS reenvía el agente** en vez de copiar una clave al servidor, así que
  ninguna credencial tuya queda en una máquina compartida.
