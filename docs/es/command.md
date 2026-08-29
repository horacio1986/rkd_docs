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

* Ayuda y comandos disponibles:

~~~
rkd --help
~~~

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

---

## Golden Paths (`rkd profiles`)

> **Nuevo en 3.2.** Un *golden path* es una combinación soportada, con nombre propio, de versión de
> Odoo, edición y versión de PostgreSQL. Rocketdoo trae diez, y son la fuente de verdad tanto para el
> asistente de `rkd init` como para `rkd init --profile`.

* Ver la matriz completa:

~~~
rkd profiles list
~~~

~~~
                           Rocketdoo golden paths
╭───────────┬──────┬─────────┬────┬───────┬──────────────────┬─────────────╮
│ Profile   │ Odoo │ Edition │ PG │ Needs │ Base             │ Support     │
├───────────┼──────┼─────────┼────┼───────┼──────────────────┼─────────────┤
│ odoo15-ce │ 15.0 │ CE      │ 14 │   12+ │ bullseye · py3.9 │ golden      │
│ odoo15-ee │ 15.0 │ EE      │ 14 │   12+ │ bullseye · py3.9 │ best effort │
│ odoo16-ce │ 16.0 │ CE      │ 14 │   12+ │ bullseye · py3.9 │ best effort │
│ odoo16-ee │ 16.0 │ EE      │ 14 │   12+ │ bullseye · py3.9 │ best effort │
│ odoo17-ce │ 17.0 │ CE      │ 15 │   12+ │ jammy · py3.10   │ best effort │
│ odoo17-ee │ 17.0 │ EE      │ 15 │   12+ │ jammy · py3.10   │ best effort │
│ odoo18-ce │ 18.0 │ CE      │ 16 │   12+ │ noble · py3.12   │ golden      │
│ odoo18-ee │ 18.0 │ EE      │ 16 │   12+ │ noble · py3.12   │ best effort │
│ odoo19-ce │ 19.0 │ CE      │ 16 │   13+ │ noble · py3.12   │ best effort │
│ odoo19-ee │ 19.0 │ EE      │ 16 │   13+ │ noble · py3.12   │ golden      │
╰───────────┴──────┴─────────┴────┴───────┴──────────────────┴─────────────╯
~~~

* Ver el detalle de un perfil:

~~~
rkd profiles show odoo18-ce
~~~

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

* Abrir la interfaz web de Mailpit en el navegador:

~~~
rkd mail open
~~~

> La interfaz web de Mailpit está disponible en `http://localhost:8025` cuando está activo.

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

---

## 🖥️ Interfaz Gráfica (`rkd gui`)

Lanza la GUI web de Rocketdoo en tu navegador. Proporciona gestión completa de contenedores, logs en vivo, mail, deploy y más — todo sin escribir comandos.

* Iniciar la GUI en el puerto predeterminado (8070):

~~~
rkd gui
~~~

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

> La GUI está disponible en `http://localhost:8070` por defecto. Presioná `Ctrl+C` para detenerla.

>>> [Más información sobre la GUI](gui.md)
