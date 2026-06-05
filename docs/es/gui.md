# Interfaz Gráfica (GUI)

**Rocketdoo v3** incluye una Interfaz Gráfica web que te permite gestionar todo tu entorno de desarrollo Odoo desde el navegador — sin necesidad de escribir comandos Docker.

## Iniciar la GUI

Lanzá la GUI desde el directorio de tu proyecto:

~~~
rkd gui
~~~

El servidor inicia y la GUI queda disponible en:

```
http://localhost:8070
```

Presioná `Ctrl+C` en la terminal para detenerla.

### Opciones

| Opción | Descripción | Valor por defecto |
|--------|-------------|-------------------|
| `--port` | Puerto en el que corre el servidor de la GUI | `8070` |
| `--host` | Dirección de host a usar | `127.0.0.1` |
| `--open` | Abrir el navegador automáticamente al iniciar | `false` |
| `--cwd` | Directorio del proyecto (si es distinto al actual) | directorio actual |

**Ejemplos:**

~~~
rkd gui --port 9090               # Puerto personalizado
rkd gui --open                    # Abrir el navegador automáticamente
rkd gui --cwd /ruta/al/proyecto   # Directorio de proyecto específico
~~~

---

## Descripción de la Interfaz

La GUI tiene una barra de navegación lateral a la izquierda y un área de contenido principal a la derecha. Soporta **modo oscuro** y **modo claro** — podés cambiar el tema con el botón en la barra superior.

---

### 📊 Dashboard

La vista principal al abrir la GUI. Muestra:

- **Cantidad de contenedores** — cuántos están corriendo.
- **Versión de Odoo** — detectada desde la configuración del proyecto.
- **Versión de PostgreSQL** — motor de base de datos en uso.
- **Puerto Web** — el puerto donde está accesible Odoo.
- **Detalles del proyecto** — edición (Community / Enterprise), claves SSH activas, repos de Gitman.
- **Enlace de acceso rápido** — abre Odoo directamente en el navegador (`http://localhost:<puerto>`).
- **Lista de contenedores** — estado de cada contenedor con indicadores de color (verde = corriendo, gris = detenido).

**Botones de acción rápida en el Dashboard:**

| Botón | Acción |
|-------|--------|
| Start All | `docker compose up -d` |
| Build | `docker compose build` |
| Down | `docker compose down` |
| Restart | `docker compose restart` |
| Stop | `docker compose stop` |
| Pull | `docker compose pull` |

Cada acción abre una **terminal inline** al pie de la página que muestra la salida del comando en tiempo real.

---

### 📋 Logs

La página de Logs proporciona streaming en tiempo real de los logs de los contenedores.

- Seleccioná un contenedor de la lista.
- Hacé clic en **Connect** para comenzar a recibir los logs.
- Hacé clic en **Stop** para desconectarte.
- Hacé clic en **Clear** para vaciar el visor de logs.

El visor colorea automáticamente las líneas:

- **Rojo** — errores
- **Amarillo** — advertencias
- **Color por defecto** — salida estándar

---

### 📧 Mail (Mailpit)

Gestioná el servicio de pruebas de email Mailpit directamente desde la GUI.

- **Activar** — inicia el contenedor de Mailpit y configura el SMTP de Odoo para que enrute los emails hacia él.
- **Desactivar** — detiene el contenedor y restaura la configuración SMTP predeterminada.
- **Estado** — muestra si Mailpit está configurado, habilitado y corriendo.
- **Abrir interfaz web** — abre `http://localhost:8025` en una nueva pestaña para inspeccionar los emails capturados.

> Mailpit captura todos los emails salientes de Odoo. Ningún email llega a destinatarios reales mientras esté habilitado.

---

### 📦 Módulos

Lista los módulos add-on de Odoo encontrados en el directorio `addons/` de tu proyecto.

---

### 🚀 Deploy

Ejecutá operaciones de despliegue (equivalentes a los comandos `rkd deploy` de la CLI) desde la GUI.

---

### 🏗️ Instance

Gestioná tus destinos de despliegue en VPS (equivalente a `rkd instance`).

- Ver entornos configurados (stage / prod).
- Iniciar despliegues hacia servidores remotos.

---

### 📦 Pack / Unpack

- **Pack** — empaqueta todo tu entorno de desarrollo en un archivo `.zip` para compartir.
- **Unpack** — restaura un entorno compartido desde un archivo `.zip`.

---

### 📚 Gitman

Gestioná las dependencias de repositorios de terceros configuradas vía Gitman.

- Ver la lista de repositorios externos.
- Ejecutar `gitman update` para obtener las últimas versiones.

---

### 🌐 Traefik

Configurá la integración del reverse proxy Traefik para tu proyecto.

- Activar o desactivar Traefik.
- Ver el dominio y modo actuales (local / producción).

---

## Notas

- La GUI se comunica con tu motor Docker local vía CLI — Docker debe estar corriendo para que las operaciones de contenedores funcionen.
- El streaming de logs usa WebSockets; mantené la pestaña del navegador abierta mientras seguís los logs.
- La GUI no requiere conexión a internet — todo corre localmente.
- Los cambios de configuración realizados desde la GUI (Mailpit, Traefik) se reflejan inmediatamente en los archivos de tu proyecto (`docker-compose.yaml`, `odoo.conf`, `.rkd/traefik.yaml`).
