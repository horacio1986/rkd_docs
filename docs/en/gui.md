# Graphical User Interface (GUI)

**Rocketdoo v3** introduces a web-based Graphical User Interface that lets you manage your entire Odoo development environment from a browser — no Docker commands required.

## Starting the GUI

Launch the GUI from your project directory:

~~~
rkd gui
~~~

The server starts and the GUI is accessible at:

```
http://localhost:8070
```

Press `Ctrl+C` in the terminal to stop it.

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--port` | Port to run the GUI server on | `8070` |
| `--host` | Host address to bind | `127.0.0.1` |
| `--open` | Open the browser automatically after starting | `false` |
| `--cwd` | Project directory (if different from current) | current dir |

**Examples:**

~~~
rkd gui --port 9090           # Custom port
rkd gui --open                # Open browser automatically
rkd gui --cwd /path/to/project  # Specific project directory
~~~

---

## Interface Overview

The GUI has a sidebar navigation on the left and a main content area on the right. It supports both **dark mode** and **light mode** — toggle the theme using the button in the top bar.

---

### 📊 Dashboard

The main view when you open the GUI. It shows:

- **Container count** — how many containers are running.
- **Odoo Version** — detected from the project configuration.
- **PostgreSQL Version** — database engine in use.
- **Web Port** — the port where Odoo is accessible.
- **Project details** — edition (Community / Enterprise), active SSH keys, Gitman repos.
- **Quick access link** — direct link to open Odoo in the browser (`http://localhost:<port>`).
- **Container list** — status of each container with color indicators (green = running, gray = stopped).

**Quick action buttons on the Dashboard:**

| Button | Action |
|--------|--------|
| Start All | `docker compose up -d` |
| Build | `docker compose build` |
| Down | `docker compose down` |
| Restart | `docker compose restart` |
| Stop | `docker compose stop` |
| Pull | `docker compose pull` |

Each action opens an **inline terminal** at the bottom of the page that streams the command output in real time.

---

### 📋 Logs

The Logs page provides real-time streaming of container logs.

- Select a container from the list.
- Click **Connect** to start streaming logs.
- Click **Stop** to disconnect.
- Click **Clear** to empty the log display.

The log viewer automatically color-codes lines:

- **Red** — errors
- **Yellow** — warnings
- **Default** — standard output

---

### 📧 Mail (Mailpit)

Manage the Mailpit email testing service directly from the GUI.

- **Enable** — starts the Mailpit container and configures Odoo's SMTP to route emails to it.
- **Disable** — stops the container and restores the default SMTP settings.
- **Status** — shows whether Mailpit is configured, enabled, and running.
- **Open Web UI** — opens `http://localhost:8025` in a new tab to inspect captured emails.

> Mailpit captures all outgoing emails from Odoo. No emails reach real recipients while it is enabled.

---

### 📦 Modules

Lists the Odoo add-on modules found in the `addons/` directory of your project.

---

### 🚀 Deploy

Run deployment operations (equivalent to the `rkd deploy` CLI commands) from the GUI.

---

### 🏗️ Instance

Manage your VPS deployment targets (equivalent to `rkd instance`).

- View configured environments (stage / prod).
- Initiate deployments to remote servers.

---

### 📦 Pack / Unpack

- **Pack** — bundles your entire development environment into a `.zip` file for sharing.
- **Unpack** — restores a shared environment from a `.zip` file.

---

### 📚 Gitman

Manage third-party repository dependencies configured via Gitman.

- View the list of external repositories.
- Run `gitman update` to pull the latest versions.

---

### 🌐 Traefik

Configure the Traefik reverse proxy integration for your project.

- Enable or disable Traefik.
- View the current domain and mode (local / production).

---

## Notes

- The GUI communicates with your local Docker engine via the CLI — Docker must be running for container operations to work.
- Log streaming uses WebSockets; keep the browser tab open while following logs.
- The GUI does not require internet access — everything runs locally.
- Configuration changes made via the GUI (Mailpit, Traefik) are reflected immediately in your project files (`docker-compose.yaml`, `odoo.conf`, `.rkd/traefik.yaml`).
