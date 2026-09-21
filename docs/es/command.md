# Línea de Comandos

Aquí tienes los comandos más comunes que puedes usar en **RKD** **ROCKETDOO**.

> Podés usar tanto `rocketdoo` como el alias corto `rkd` — ambos son equivalentes.

> **TIP:** Combiná cualquier comando con `--help` para ver sus flags y opciones.  
> Ejemplo: `rkd build --help`

---

## General

* Versión de Rocketdoo:

~~~
rkd --version
~~~

![rkd --version](../img/term-version.svg)

* Ayuda y comandos disponibles:

~~~
rkd --help
~~~

![rkd --help](../img/term-help.svg)

---

## Configuración del Proyecto

* Crear el árbol de directorios y archivos:

~~~
rkd scaffold
~~~

* Iniciar el asistente de configuración:

~~~
rkd init
~~~

* O crear el entorno directamente desde un perfil soportado, sin responder preguntas:

~~~
rkd init --profile odoo18-ce
~~~

* Ver información detallada del proyecto actual:

~~~
rkd info
~~~

![rkd info](../img/term-info.svg)

> Desde la 3.5, `rkd info` también avisa cuando un módulo bajo `addons/` está en un subdirectorio
> que el `addons_path` de Odoo no cubre. Sólo avisa, nunca escribe. Los que escriben la ruta son:
> `rkd up`, `rkd restart`, `rkd build --rebuild`, `rkd ci prepare`, el Up de la GUI y el update por
> módulo.

---

## Golden Paths (`rkd profiles`)

> **Nuevo en 3.2.** Un *golden path* es una combinación soportada, con nombre propio, de versión de
> Odoo, edición y versión de PostgreSQL. Rocketdoo trae diez, y son la fuente de verdad tanto para el
> asistente de `rkd init` como para `rkd init --profile`.

* Ver la matriz completa:

~~~
rkd profiles list
~~~

![Matriz de golden paths: diez perfiles, Odoo 15 a 19, Community y Enterprise, con su versión de PostgreSQL y nivel de soporte](../img/term-profiles-list.svg)

* Ver el detalle de un perfil:

~~~
rkd profiles show odoo18-ce
~~~

![rkd profiles show odoo18-ce](../img/term-profiles-show.svg)

* Crear ese entorno de punta a punta, sin pasar por el asistente:

~~~
mkdir mi-proyecto && cd mi-proyecto
rkd scaffold
rkd init --profile odoo18-ce
rkd up -d
~~~

### Niveles de soporte

| Nivel | Qué significa |
|---|---|
| **golden** | El CI renderiza **y construye** esta combinación en cada PR de release. Son las que conviene elegir. |
| **best effort** | Está dentro de los requisitos que Odoo declara y el asistente la ofrece, pero el CI no la construye. Se aceptan reportes, sin garantía. |

Hoy las combinaciones golden son `odoo15-ce`, `odoo18-ce` y `odoo19-ee`: entre las tres cubren los
tres sistemas base distintos que usan las imágenes `odoo:` y las dos ediciones.

### Matriz de compatibilidad

Los datos por imagen se leyeron de las imágenes `odoo:` publicadas; los mínimos de PostgreSQL salen
de la documentación de instalación de Odoo.

| Odoo | Base | Python | pip | PostgreSQL mínimo | Recomendado |
|---|---|---|---|---|---|
| 15.0 | debian-bullseye | 3.9 | 20.3.4 | 12 | 14 |
| 16.0 | debian-bullseye | 3.9 | 20.3.4 | 12 | 14 |
| 17.0 | ubuntu-jammy | 3.10 | 22.0.2 | 12 | 15 |
| 18.0 | ubuntu-noble | 3.12 | 24.0 | 12 | 16 |
| 19.0 | ubuntu-noble | 3.12 | 24.0 | **13** | 16 |

Odoo 19 subió el mínimo de PostgreSQL de 12 a 13. Rocketdoo ahora valida la combinación al cargar el
perfil: un `db_version` por debajo del mínimo de esa versión de Odoo es un error, no una advertencia,
así que el asistente ya no puede generar un entorno que no arranque.

> **Odoo 19 y AI:** las funciones de AI de Odoo 19 necesitan la extensión `pgvector`, que se
> distribuye para PostgreSQL 15 en adelante. `rkd profiles show odoo19-*` avisa cuando el perfil usa
> una versión menor.

> **Enterprise:** cualquier perfil `*-ee` espera un directorio `./enterprise` con los add-ons de Odoo
> Enterprise (requiere suscripción), al mismo nivel que `addons/`, antes de correr `rkd up -d`.

---

## Gestión de Contenedores

* Lanzar el despliegue de Odoo (iniciar contenedores en modo detached):

~~~
rkd up -d
~~~

* Conocer el estado de tus contenedores:

~~~
rkd status
~~~

![rkd status](../img/term-status.svg)

* Detener todos los contenedores:

~~~
rkd stop
~~~

* Reiniciar los contenedores:

~~~
rkd restart
~~~

* Eliminar los contenedores:

~~~
rkd down
~~~

* Eliminar contenedores y sus volúmenes asociados:

~~~
rkd down -v
~~~

* Forzar la reconstrucción del entorno:

~~~
rkd build
~~~

* Reconstruir y reiniciar contenedores en un solo paso:

~~~
rkd build --rebuild
~~~

* Ver los logs de los contenedores:

~~~
rkd logs
~~~

* Seguir los logs en tiempo real de un contenedor específico:

~~~
rkd logs <nombre_contenedor> -f
~~~

---

## Compartir Entornos

* Empaquetar todo tu proyecto Odoo completo para compartir con otros desarrolladores:

~~~
rkd pack
~~~

* Desempaquetar el proyecto compartido en un directorio previamente creado:

~~~
rkd unpack
~~~

---

## Utilidades

* Eliminar archivos `.Identifier` generados por WSL2 al copiar archivos desde Windows:

~~~
rkd del -i
~~~

* Previsualizar qué archivos se eliminarían (simulación, sin borrar nada):

~~~
rkd del -i --dry-run
~~~

* Actualizar masivamente los paquetes de módulos cargados como repositorios públicos con Gitman:  
(Este comando debe ejecutarse dentro del contenedor.)

~~~
gitman update
~~~

---

## ⚙️ Integración Continua (`rkd ci`)

> **Nuevo en la 3.5.** `rkd ci` escribe un workflow de GitHub Actions **para tu proyecto** — no para
> Rocketdoo. Lintea tus addons y, opcionalmente, los instala contra un Odoo real.

![rkd ci --help](../img/term-ci-help.svg)

* Generar `.github/workflows/rkd-ci.yml`:

~~~
rkd ci init
~~~

En una terminal esto **te pregunta cuándo debe correr el job de instalación**, con `pull_request`
como valor por defecto. Respondé de entrada, o saltate la pregunta:

~~~
rkd ci init -y                              # toma el default, sin preguntar
rkd ci init --install-trigger never         # sólo lint
~~~

![rkd ci init -y](../img/term-ci-init.svg)

| Opción | Descripción |
|--------|-------------|
| `--install-trigger` | Cuándo corre el job de instalación: `pull_request`, `push`, `manual`, `never`. Se pregunta si lo omitís; `pull_request` sin terminal |
| `--force` | Sobreescribir un workflow que difiera del render actual |
| `-y, --yes` | Saltear la pregunta del install-trigger y tomar el default |

Nunca pisa un archivo que difiera de lo que escribiría, sin `--force`, venga la diferencia de una
edición a mano o de un cambio de configuración. Lo que te informa: `Created:` si el archivo es
nuevo, *already up to date* si coincide, una advertencia que nombra `--force` si difiere, y
`Overwritten:` cuando se lo pasás.

* Regenerar los archivos que un clon limpio no trae:

~~~
rkd ci prepare
~~~

![rkd ci prepare](../img/term-ci-prepare.svg)

`config/odoo.conf` y `odoo_pg_pass` están gitignoreados — tienen credenciales —, así que un clon
limpio no los tiene. El build copia `config/odoo.conf` dentro de la imagen
(`COPY ./config/odoo.conf`) y compose le pasa `odoo_pg_pass` como secret al levantar, así que sin
este paso un checkout limpio no llega a ningún lado. También sincroniza el `addons_path`. Nunca
sobreescribe un `odoo.conf` que ya exista, que es lo que está informando la salida de arriba.

Pasá `--admin-passwd` para fijar el master password que escribe; si no, se genera uno.

* Listar los módulos instalables que Odoo puede realmente alcanzar:

~~~
rkd ci modules
~~~

![rkd ci modules](../img/term-ci-modules.svg)

Los imprime separados por comas en stdout, para `MODULES=$(rkd ci modules)` dentro del workflow.

### Qué hace el workflow generado

| Job | Cuándo corre | Qué cuesta |
|-----|--------------|------------|
| **Lint** | En cada pull request, en los push a la rama por defecto, y a mano | Segundos |
| **Instalación** | En pull requests contra la rama por defecto, por defecto | Varios minutos |

El workflow se dispara con `pull_request`, con `push` **sólo a la rama por defecto**, y con
`workflow_dispatch`. Pushear una rama de feature sin un pull request abierto no corre nada. El job
de lint corre `ruff check` sobre `addons/` y después `rkd deploy validate -p addons`.

El job de instalación es el caro, y por eso viene limitado por defecto. En repos **privados** el
plan Free da 2.000 minutos de Actions por mes compartidos entre toda tu cuenta; los repos
**públicos** no consumen esa cuota. Cambiá el disparador con `--install-trigger` si ese reparto no
te sirve.

> Esos números son de GitHub, no de Rocketdoo, y GitHub los cambia:
> [mirá la facturación de Actions vigente](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).

**Lint de los manifests.** Las reglas por defecto de ruff marcan `B018` en todo `__manifest__.py` —
por definición de Odoo es un dict literal suelto —, así que el workflow generado pasa
`--extend-per-file-ignores "**/__manifest__.py:B018"`. Sin eso el lint sale rojo en todo proyecto
Odoo.

> `rkd ci init` se despide diciendo *"the lint job runs ruff with its default rules"*. El workflow
> generado sí agrega la excepción de arriba — el impreciso es el mensaje, no el workflow.

### Qué queda fuera del camino generado

- **Enterprise y repos privados por SSH.** El workflow se emite igual, sin el job de instalación y
  con el motivo como comentario: un runner no tiene acceso a tus fuentes privadas.
- **Fuentes privadas de Gitman.** `rkd ci init` avisa si `gitman.yaml` tiene fuentes `git@`/`ssh://`,
  pero no puede distinguir un HTTPS privado de uno público: ese build falla en el runner sin aviso
  previo.
- **Deploy a staging.** Diferido a propósito: `rkd deploy` no tiene forma headless de generar un
  `deploy.yaml`, así que el workflow generado sólo lintea e instala.

---

## 📧 Mail — Testing de Email con Mailpit (`rkd mail`)

Mailpit es un servidor SMTP local con interfaz web que captura todos los emails salientes de Odoo en lugar de enviarlos realmente. Ideal para probar flujos de email sin afectar a usuarios reales.

* Activar Mailpit (inicia el servicio y configura el SMTP de Odoo automáticamente):

~~~
rkd mail on
~~~

* Desactivar Mailpit y restaurar la configuración SMTP predeterminada:

~~~
rkd mail off
~~~

* Ver el estado actual de Mailpit:

~~~
rkd mail status
~~~

![rkd mail status](../img/term-mail-status.svg)

* Abrir la interfaz web de Mailpit en el navegador:

~~~
rkd mail open
~~~

> La interfaz web de Mailpit está disponible en `http://localhost:8025` cuando está activo.

### El servidor de correo en Odoo

> **Nuevo en la 3.3.** Además del toggle de compose y `odoo.conf`, `on`/`off`/`status` ahora leen y
> escriben un registro `ir.mail_server` en tu base, llamado **`Mailpit (rkd)`**, con
> `smtp_host = mailpit` y `sequence = 1`. Antes de esto, activar Mailpit configuraba el transporte
> pero dejaba a Odoo apuntando al servidor de correo que tuviera la base.

`off` sólo archiva el registro (`active = false`) — nunca borra, así que un servidor de correo tuyo
no corre riesgo. El registro se identifica por nombre más `smtp_host`: si lo renombrás a mano, `off`
no lo va a encontrar (y te lo dice) y `on` va a crear uno nuevo.

**Los proyectos con varias bases necesitan `--db`:**

~~~
rkd mail on --db mi_base
rkd mail status --db mi_base
rkd mail off --db mi_base
~~~

Con exactamente una base en el proyecto se elige sola. Con dos o más y sin `--db`, el comando no lee
ni escribe el registro, y te lo informa.

Para esta parte el contenedor `db` tiene que estar arriba. Si no lo está, el toggle de compose y
`odoo.conf` se hace igual y el paso del servidor de correo se informa como no realizado — corré
`rkd up -d` y repetí el mismo comando.

> **Límite conocido.** `sequence = 1` no garantiza que gane Mailpit. Odoo filtra los servidores de
> correo por `from_filter` *antes* de ordenar por `sequence`, así que otro servidor activo cuyo
> `from_filter` matchee al remitente puede ganarle igual. `rkd mail status` avisa cuando hay otros
> servidores activos, pero no lee ni escribe `from_filter`.

---

## 🌐 Reverse Proxy Traefik (`rkd traefik`)

Traefik permite exponer tu instancia local de Odoo con un dominio personalizado, tanto para desarrollo local (HTTP) como para entornos de producción (HTTPS con Let's Encrypt).

* Activar Traefik para este proyecto (asistente interactivo — solicita dominio y modo):

~~~
rkd traefik on
~~~

* Activar con opciones específicas:

~~~
rkd traefik on --domain miproyecto.local --mode local
rkd traefik on --domain miproyecto.com --mode production
~~~

* Desactivar Traefik para este proyecto (restaura el acceso directo por puerto):

~~~
rkd traefik off
~~~

* Ver el estado de la integración con Traefik:

~~~
rkd traefik status
~~~

* Ver la guía paso a paso para configurar dominios locales (`/etc/hosts` y WSL2):

~~~
rkd traefik guide
~~~

> Modos de Traefik:
> - **local** — Solo HTTP, dominio personalizado vía `/etc/hosts`.
> - **production** — HTTPS con certificado Let's Encrypt automático.

---

## 🚀 Despliegue de Instancias en VPS (`rkd instance`)

El comando `instance` despliega una instancia completa de Odoo en un VPS. Soporta dos tipos de despliegue:

- **docker** — transfiere el Dockerfile + compose y construye en el VPS.
- **native** — instala Odoo mediante los paquetes apt oficiales en el servidor.

* Configurar los entornos stage/producción de forma interactiva (guarda en `.rkd/instance.yaml`):

~~~
rkd instance init
~~~

* Sobreescribir una configuración existente:

~~~
rkd instance init --force
~~~

* Desplegar en el entorno staging:

~~~
rkd instance deploy --env stage
~~~

* Desplegar en producción (con confirmación):

~~~
rkd instance deploy --env prod
~~~

* Desplegar en producción omitiendo la confirmación:

~~~
rkd instance deploy --env prod --yes
~~~

* Previsualizar los archivos generados sin desplegar (dry run):

~~~
rkd instance deploy --env prod --dry-run
~~~

* Ver el estado de todos los destinos de despliegue configurados:

~~~
rkd instance status
~~~

![rkd instance status](../img/term-instance-status.svg)

---

## 🖥️ Interfaz Gráfica (`rkd gui`)

Lanza la GUI web de Rocketdoo en tu navegador. Proporciona gestión completa de contenedores, logs en vivo, mail, deploy y más — todo sin escribir comandos.

* Iniciar la GUI en el puerto predeterminado (8070):

~~~
rkd gui
~~~

> **Desde la 3.4, leé la URL que imprime.** Cada ejecución genera un token de sesión y la GUI no
> carga sin él: `http://127.0.0.1:8070/?token=<token>`. Entrar a `http://localhost:8070` a secas no
> te lleva a ningún lado, y reiniciar `rkd gui` invalida el token anterior. Ver
> [Interfaz Gráfica (GUI)](gui.md#el-token-de-sesion).

* Iniciar la GUI en un puerto personalizado:

~~~
rkd gui --port 9090
~~~

* Iniciar la GUI y abrir el navegador automáticamente:

~~~
rkd gui --open
~~~

* Iniciar la GUI para un proyecto en un directorio específico:

~~~
rkd gui --cwd /ruta/al/proyecto
~~~

> La GUI está disponible en `http://localhost:8070` por defecto — **en la URL con el token que
> imprime**. Presioná `Ctrl+C` para detenerla.

>>> [Más información sobre la GUI](gui.md)
