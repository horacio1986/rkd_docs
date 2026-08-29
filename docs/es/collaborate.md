# 🚀 Guía de Contribución a Rocketdoo

¡Gracias por tu interés en contribuir a **Rocketdoo**! Esta guía describe el camino completo que recorre un cambio: desde que detectás un bug o proponés una mejora, hasta que ese cambio queda publicado en PyPI.

El repositorio oficial es **[HDM-soft/rocketdoo](https://github.com/HDM-soft/rocketdoo)**.

---

## Tabla de contenidos

- [Reglas de oro](#reglas-de-oro)
- [Modelo de ramas](#modelo-de-ramas)
- [El camino de una contribución](#el-camino-de-una-contribucion)
- [Paso 1 — Abrir la issue](#paso-1-abrir-la-issue)
- [Paso 2 — Preparar el entorno de desarrollo](#paso-2-preparar-el-entorno-de-desarrollo)
- [Paso 3 — Crear la rama de trabajo](#paso-3-crear-la-rama-de-trabajo)
- [Paso 4 — Desarrollar y commitear](#paso-4-desarrollar-y-commitear)
- [Paso 5 — Probar antes de abrir el PR](#paso-5-probar-antes-de-abrir-el-pr)
- [Paso 6 — Abrir el Pull Request hacia `dev/v3`](#paso-6-abrir-el-pull-request-hacia-devv3)
- [Paso 7 — Revisión y merge](#paso-7-revision-y-merge)
- [Paso 8 — Promoción hasta producción](#paso-8-promocion-hasta-produccion)
- [Versionado y releases](#versionado-y-releases)
- [Hotfix urgente](#hotfix-urgente)
- [Convenciones de código](#convenciones-de-codigo)
- [Problemas frecuentes](#problemas-frecuentes)

---

## Reglas de oro

Antes de escribir una sola línea de código, cinco reglas que ordenan todo el proceso:

1. **Toda tarea empieza con una issue.** Sin issue no hay rama. El número de la issue nombra la rama y aparece en los commits y en el PR.
2. **Nunca se trabaja directo sobre `main`, `test/v3` ni `dev/v3`.** Siempre una rama corta creada desde `dev/v3`.
3. **Ningún PR de contribución apunta a `main`.** Los cambios entran por `dev/v3` y los mantenedores los promueven a `test/v3` y luego a `main`.
4. **La versión no la toca quien contribuye.** El bump de `pyproject.toml` lo hace el mantenedor que corta la release, en un commit dedicado. Ver [Versionado y releases](#versionado-y-releases).
5. **Un PR = un cambio con sentido propio.** Si estás resolviendo dos problemas distintos, son dos issues y dos ramas.

---

## Modelo de ramas

```
main              ← versión estable · mergear un PR acá publica en PyPI (CI)
  ↑ PR de release (lo hace un mantenedor)
test/v3           ← gate de QA: se instala el paquete desde esta rama y se prueba
  ↑ PR de integración probada (lo hace un mantenedor)
dev/v3            ← integración de desarrollo · base de todas las ramas de trabajo
  ↑ PR de tu contribución
fix/123-slug · feat/124-slug · mig/125-slug · chore/126-slug
```

Camino de promoción de cualquier cambio:

```
<tipo>/N-slug  →  dev/v3  →  test/v3  →  main  →  PyPI
```

> `test/v3` es la **rama intermedia** entre desarrollo y producción. Existe para probar el paquete **ya instalado** (no el repo local): un cambio que funciona en tu clon puede fallar cuando se empaqueta y se instala con `pipx`. Ese filtro es el que evita publicar una versión rota en PyPI.

`dev/v2` es la línea anterior, ya estabilizada; sólo recibe back-ports puntuales.

---

## El camino de una contribución

Este es el recorrido completo, de punta a punta:

```
issue #123  ─────────────────────────────────────────────────────────────────
  │  describe el problema: reproducción, esperado vs. actual, entorno
  │
  ├─ rama  fix/123-descripcion-corta        creada desde dev/v3
  │        commits "FIX: ..." con "Refs #123" en el cuerpo
  │        prueba local: pipx install + rkd scaffold en carpeta vacía
  │
  ├─ PR → dev/v3        "Refs #123"   → revisión de mantenedores → merge
  │
  ├─ PR → test/v3       gate de QA: se instala desde la rama y se corre el checklist
  │
  └─ PR → main          "Closes #123" → el CI construye y publica en PyPI
                        → tag vX.Y.Z + GitHub Release → la issue se cierra
```

Los tres primeros bloques son tu responsabilidad como contribuidor. Los dos últimos los ejecutan los mantenedores, pero conviene conocerlos para entender por qué se piden ciertas cosas en el PR.

---

## Paso 1 — Abrir la issue

La issue es el punto de partida: da contexto, permite discutir el enfoque antes de invertir tiempo en código, y le da nombre a la rama.

### 1.1 Buscar duplicados

```bash
# Con GitHub CLI
gh issue list --repo HDM-soft/rocketdoo --search "unpack filestore" --state all -L 10
```

O desde la web: [Issues del repositorio](https://github.com/HDM-soft/rocketdoo/issues).

### 1.2 Reproducir el problema en un entorno limpio

Rocketdoo genera toda la estructura del proyecto con `rkd scaffold`, así que la reproducción tiene que hacerse en una **carpeta vacía**, con la versión publicada:

```bash
mkdir -p /tmp/rkd-repro && cd /tmp/rkd-repro
rkd --version        # anotar la versión exacta
rkd scaffold
rkd init
# ... el comando que falla
```

Anotá: comando exacto, salida completa (o el traceback), versión de Rocketdoo, sistema operativo, versión de Python y de Docker.

### 1.3 Crear la issue

```bash
gh issue create --repo HDM-soft/rocketdoo \
  --title "FIX: rkd unpack no restaura el filestore cuando el volumen tarda en montarse" \
  --label bug \
  --body-file /tmp/issue.md
```

O desde la web con el botón **New issue**.

**Título:** una sola línea, con el mismo prefijo que va a llevar el commit final (`FIX:`, `FEAT:`, `MIG:`, `DOC:`, `REF:`), describiendo el síntoma si es un bug o el resultado esperado si es una mejora.

**Labels sugeridas:** `bug`, `enhancement`, `migration`, `docs`, `ci`, `v3`, `priority:high`.

### 1.4 Plantilla — reporte de bug

```markdown
## Descripción
<qué falla, en una o dos frases>

## Pasos para reproducir
1. `mkdir /tmp/rkd-test && cd /tmp/rkd-test`
2. `rkd scaffold`
3. `rkd init` (valores usados: Odoo 17.0, Community, sin gitman, puertos 8069/8888)
4. `rkd unpack`

## Comportamiento esperado
<qué debería pasar>

## Comportamiento actual
<qué pasa; pegar el traceback o la salida relevante en un bloque de código>

## Entorno
- rocketdoo: `rkd --version` → 3.2.0
- Python: 3.12.3
- SO: Ubuntu 24.04 (WSL2)
- Docker: 27.x / Compose v2.x

## Impacto
<bloqueante | degradado | cosmético>
```

### 1.5 Plantilla — mejora o feature

```markdown
## Problema o limitación actual
<qué no se puede hacer hoy y a quién afecta>

## Propuesta
<comportamiento propuesto; si suma un comando u opción, mostrar la interfaz>

## Alternativas consideradas
<opciones descartadas y por qué>

## Criterios de aceptación
- [ ] <condición verificable 1>
- [ ] <condición verificable 2>
- [ ] Documentado en el README y en la documentación

## Impacto en versionado
<patch | minor | major> — <justificación>
```

**Guardá el número de la issue.** Es el `N` que vas a usar en la rama, en los commits y en el PR.

---

## Paso 2 — Preparar el entorno de desarrollo

### Requisitos previos

- Python 3.10 o superior (según `requires-python` de `pyproject.toml`)
- Git
- `pip` actualizado (`pip install --upgrade pip`)
- `pipx` (recomendado para probar la CLI)
- GitHub CLI (`gh`) — opcional, pero simplifica issues y PRs

### 1. Hacer fork del repositorio

Desde GitHub, hacé clic en **Fork** en la esquina superior derecha de [HDM-soft/rocketdoo](https://github.com/HDM-soft/rocketdoo).

### 2. Clonar tu fork localmente

```bash
git clone https://github.com/TU_USUARIO/rocketdoo.git
cd rocketdoo
```

### 3. Agregar el repositorio original como remote

```bash
git remote add upstream https://github.com/HDM-soft/rocketdoo.git
```

Esto te permite sincronizarte con los cambios del repo original cuando sea necesario:

```bash
git fetch upstream
git merge upstream/dev/v3
```

### 4. Entorno virtual de desarrollo

Para modificar el código y ver los cambios reflejados de inmediato, usá un entorno virtual dedicado:

```bash
# Desde la raíz del repo clonado
python -m venv venv-dev

# Linux / macOS
source venv-dev/bin/activate
# Windows
.\venv-dev\Scripts\activate
```

### 5. Instalar el paquete y las dependencias de desarrollo

Instalá Rocketdoo en modo editable junto con las herramientas que usa el CI — `pytest`,
`pytest-cov`, `ruff` y `httpx`:

```bash
pip install -e . -r requirements-dev.txt
```

El modo editable hace que tus cambios en el código tomen efecto sin reinstalar. Verificá que la
suite esté en verde **antes** de tocar nada, así sabés que cualquier falla posterior es tuya:

```bash
pytest
```

---

## Paso 3 — Crear la rama de trabajo

Siempre desde `dev/v3` actualizado, nunca desde `main`:

```bash
git fetch upstream
git checkout dev/v3
git pull --ff-only upstream dev/v3
```

**Nomenclatura:** `<tipo>/<numero-de-issue>-<slug-corto>`

```bash
git checkout -b fix/123-unpack-filestore
# Ejemplos:
# git checkout -b feat/124-soporte-multi-modelo
# git checkout -b mig/125-odoo-18
# git checkout -b chore/126-actualizar-dependencias
```

Si usás GitHub CLI, este comando crea la rama **ya vinculada a la issue** en GitHub:

```bash
gh issue develop 123 --base dev/v3 --name fix/123-unpack-filestore --checkout
```

### Tipos de rama

| Tipo | Cuándo usarlo | Base |
|------|--------------|------|
| `fix/` | Corrección de un bug | `dev/v3` |
| `feat/` | Nueva funcionalidad o comando | `dev/v3` |
| `mig/` | Migración (versión de Odoo, dependencia, formato de config) | `dev/v3` |
| `chore/` | Mantenimiento (deps, CI, limpieza) | `dev/v3` |
| `docs/` | Sólo documentación | `dev/v3` |
| `refactor/` | Refactor sin cambio de comportamiento | `dev/v3` |
| `hotfix/` | Urgencia con producción rota | `main` |

---

## Paso 4 — Desarrollar y commitear

Un commit por cambio lógico, con prefijos en mayúscula (los que usa el historial del proyecto):

| Prefijo | Cuándo usarlo |
|---------|--------------|
| `FIX:` | Corrección de un bug |
| `FEAT:` | Nueva funcionalidad o comando |
| `UPD:` | Actualización general (CI, config, docs junto al código) |
| `UPG:` | **Exclusivo del bump de versión** (`UPG: version 3.2.0`) — reservado a mantenedores |
| `REF:` | Refactor sin cambio de comportamiento |
| `DOC:` | Documentación |
| `TEST:` | Agregar o corregir tests |
| `DEL:` | Eliminación de código o artefactos |

En el **cuerpo** del commit se referencia la issue:

```bash
git commit -m "FIX: espera el montaje del volumen antes de restaurar el filestore

Refs #123"
```

Usá `Refs #123` en los commits intermedios. `Closes #123` se reserva para el PR de release hacia `main`, porque GitHub sólo cierra issues automáticamente cuando el PR se mergea a la rama por defecto.

---

## Paso 5 — Probar antes de abrir el PR

### 5.1 Instalar tu código local y ejecutar el ciclo mínimo

```bash
pipx install /ruta/a/tu/clon/rocketdoo --force

mkdir -p /tmp/rkd-test && cd /tmp/rkd-test
rkd scaffold
rkd init
# ... y el comando que toca tu cambio
```

### 5.2 Pre-flight del gate de CI

La integración continua corre en cada pull request y **no hay ningún `continue-on-error`**: un job en
rojo significa que el cambio no se mergea. El único check requerido es `ci-ok`, que agrupa a todos
los demás:

| Job | Qué ejecuta |
|-----|-------------|
| `Lint (ruff)` | `ruff check .` y `ruff format --check .` |
| `Tests (py3.10 … py3.13)` | la suite `pytest` completa, con cobertura, en las cuatro versiones de Python soportadas |
| `E2E with Docker` | los tests marcados `docker` — en el CI no se pueden saltear en silencio |
| `Build distributions` | `python -m build`, `twine check`, y la comprobación de que el wheel realmente incluye `templates/` y la GUI |
| `Golden path …` | `rkd scaffold && rkd init --profile …` para `odoo15-ce`, `odoo18-ce` y `odoo19-ee`, y después `docker compose config` sobre el resultado |
| `Docker build (odoo:…)` | un `docker build` real contra las cinco imágenes de Odoo soportadas — solo en PRs hacia `test/v3` o `main` |

Corré lo mismo en local antes de pushear:

```bash
source venv-dev/bin/activate
pip install -e . -r requirements-dev.txt

ruff check .
ruff format --check .
pytest                      # la suite completa
pytest -m "not docker"      # si no tenés un daemon de Docker disponible
```

Los tests marcados `docker` se saltean solos cuando no hay daemon local, pero en el CI sí corren, así
que un cambio que solo funciona en tu máquina igual queda expuesto.

Si tu cambio toca una plantilla Jinja, los tests de snapshot van a fallar a propósito. Leé primero el
diff; si el render nuevo es el que buscabas, regenerá la referencia y commiteala:

```bash
UPDATE_SNAPSHOTS=1 pytest tests/test_templates.py
```

Si algo de esto devuelve errores, el PR va a quedar en rojo.

### 5.3 Probar una rama sin afectar tu instalación productiva

Para revisar la rama de otra persona (o la tuya ya pusheada) sin desinstalar tu Rocketdoo de trabajo:

#### Opción A — Con `pipx` (recomendado para la CLI) ✅

`pipx` instala en un entorno aislado y expone los comandos globalmente. Con `--suffix` conviven la versión productiva y la de prueba:

```bash
pipx install "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore" --suffix=-test
```

Esto te deja dos comandos en paralelo:

```bash
rkd             # ← versión productiva instalada desde PyPI (intacta)
rkd-test        # ← versión de la rama de prueba
```

> El sufijo se aplica a **todos** los comandos del paquete, así que también quedan disponibles `rocketdoo` y `rocketdoo-test`. Para `pipx` el paquete instalado se llama `rocketdoo-test`.

Para actualizar cuando se pushean nuevos cambios a la rama, y para limpiar al terminar:

```bash
pipx reinstall rocketdoo-test
pipx uninstall rocketdoo-test
```

#### Opción B — Con `pip` dentro de un `venv` (para imports o scripts)

```bash
mkdir test-rocketdoo && cd test-rocketdoo
python -m venv venv-test
source venv-test/bin/activate   # Linux / macOS
# .\venv-test\Scripts\activate  # Windows

pip install "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"

# actualizar tras nuevos commits
pip install --force-reinstall "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"
```

#### Repo privado: autenticación con token

```bash
pip install "git+https://TU_TOKEN@github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"
```

| Método | Cuándo usarlo |
|--------|--------------|
| `pipx install git+... --suffix=-test` | **Recomendado.** Probar comandos CLI sin tocar la versión productiva |
| `pipx install /ruta/local --force` | Probar tu propio código antes de pushear |
| `pip install git+...` dentro de un venv | Probar imports de Python o integraciones en scripts |

---

## Paso 6 — Abrir el Pull Request hacia `dev/v3`

```bash
git push -u origin fix/123-unpack-filestore

gh pr create --repo HDM-soft/rocketdoo \
  --base dev/v3 --head TU_USUARIO:fix/123-unpack-filestore \
  --title "FIX: espera el montaje del volumen antes de restaurar el filestore" \
  --body-file /tmp/pr.md
```

O desde la web con el botón **Compare & pull request**, verificando que el destino sea `dev/v3`.

> **El PR apunta a `dev/v3`, nunca a `main` ni a `test/v3`.** Un PR contra `main` dispara el workflow de publicación en PyPI y va a ser cerrado sin merge.

### Plantilla de PR

````markdown
## Qué cambia
<resumen en 2-4 líneas>

Refs #123

## Por qué
<causa raíz del bug, o necesidad de la mejora>

## Cómo probarlo
```bash
pipx install "git+https://github.com/TU_USUARIO/rocketdoo.git@fix/123-unpack-filestore" --suffix=-test
mkdir /tmp/rkd-pr && cd /tmp/rkd-pr
rkd-test scaffold && rkd-test init && rkd-test unpack
```

Resultado esperado: <...>

## Verificado
- [ ] La reproducción de #123 ya no ocurre
- [ ] `ruff check .` y `ruff format --check .` limpios
- [ ] `pytest` en verde
- [ ] El comportamiento nuevo está cubierto por al menos un test
- [ ] Ciclo local con pipx OK
- [ ] No se modificó la versión en `pyproject.toml`
- [ ] No se agregaron artefactos de build al repo
````

Usá `Refs #123`, no `Closes #123`: el cierre automático sólo ocurre al mergear a `main`.

---

## Paso 7 — Revisión y merge

Una vez abierto el PR, **pedí revisión** a los mantenedores (campo *Reviewers*, o mencionándolos en un comentario). Ningún PR se mergea sin al menos una aprobación.

Qué mira quien revisa:

- El cambio resuelve lo que describe la issue, y nada más.
- No hay cambios de versión en `pyproject.toml`, `README.md` ni `LICENSE`.
- No se colaron artefactos de build (`dist/`, `build/`, `*.egg-info`).
- El check `ci-ok` está en verde: `ruff` limpio, la suite `pytest` completa pasando, y un test que cubre el comportamiento nuevo.
- El comportamiento nuevo está documentado si cambia el uso de un comando.
- Hay una forma concreta de reproducir y verificar el arreglo.

Si te piden cambios: commiteá sobre la **misma rama** y pusheá; el PR se actualiza solo. No abras un PR nuevo.

```bash
git commit -m "FIX: aplica observaciones de la revisión

Refs #123"
git push
```

Cuando el PR queda aprobado, un mantenedor lo mergea a `dev/v3` (squash si la rama tiene commits de ida y vuelta) y borra la rama remota. A partir de ahí tu cambio viaja con el resto de la integración.

---

## Paso 8 — Promoción hasta producción

Estos dos PRs los abren los mantenedores. Se documentan acá para que se entienda dónde termina cada cambio.

### 8.1 `dev/v3` → `test/v3` (gate de QA)

Antes de abrir este PR, el mantenedor aplica el bump de versión en `dev/v3`. Tras el merge, se prueba el paquete **instalado desde la rama**, no el repo local:

```bash
pipx install "git+https://github.com/HDM-soft/rocketdoo.git@test/v3" --suffix=-test
rocketdoo-test --version
```

Checklist de QA: `scaffold`, `init`, `info`, `up/status/logs/down`, `mail on|off|status`, `traefik on|off|status`, `deploy init|list-modules`, `gui`, `pack`/`unpack` si el cambio los toca, y **la reproducción de cada issue incluida, verificando que ya no ocurre**.

Si algo falla en este punto, se corrige en una rama nueva desde `dev/v3` (vuelta al [Paso 3](#paso-3-crear-la-rama-de-trabajo)), nunca sobre `test/v3`.

### 8.2 `test/v3` → `main` (release)

Este PR sí lleva `Closes #123` (una línea por issue incluida) y las notas de versión. Al mergearlo se dispara `.github/workflows/publish-rkd.yml`, que corre los tests, construye el paquete y lo publica en PyPI mediante Trusted Publishing.

Después de la publicación, y de forma manual: tag sobre el merge commit (`git tag -a vX.Y.Z origin/main`), GitHub Release, cierre de las issues que no se cerraron solas, y back-merge de `main` hacia `test/v3` y `dev/v3`.

---

## Versionado y releases

**Si estás contribuyendo, no toques la versión.** El bump lo hace el mantenedor que corta la release, sobre `dev/v3`, en un commit dedicado `UPG: version X.Y.Z`. Esto evita conflictos entre ramas simultáneas y bumps duplicados.

La versión vive en **tres** archivos y se mueven juntos en el mismo commit:

| Archivo | Campo |
|---------|-------|
| `pyproject.toml` | `version = "X.Y.Z"` — fuente de verdad del paquete (lo que se publica en PyPI) |
| `README.md` | el bloque `## Version:` |
| `LICENSE` | la línea `Versión:` del encabezado (actualizar también `Fecha:`) |

`rocketdoo/__init__.py` **no** se edita: lee la versión instalada en tiempo de ejecución con `importlib.metadata`, y el `__version__ = "dev"` que aparece ahí es sólo el fallback para un checkout de código fuente.

Versionado semántico:

| Cambio | Incremento | Ejemplo |
|--------|-----------|---------|
| Bug fix, ajuste interno, documentación | patch | 3.2.0 → 3.2.1 |
| Comando u opción nueva, feature compatible | minor | 3.2.1 → 3.3.0 |
| Ruptura de compatibilidad (CLI, formato de config, layout generado) | major | 3.3.0 → 4.0.0 |

> Nunca se ejecuta `twine upload` a mano contra PyPI producción: las versiones de PyPI son inmutables, y una subida manual hace que el CI falle después con `400 File already exists`. Para ensayar la publicación se usa TestPyPI.

---

## Hotfix urgente

Sólo para producción rota, cuando esperar el gate de QA no es viable. La rama se crea desde `main`:

```bash
git fetch upstream
git checkout -b hotfix/130-slug upstream/main
```

El PR va directo a `main`. Como se saltea `test/v3`, la validación local es **obligatoria**: build del paquete, `twine check`, instalación del wheel con `pipx` y verificación de que el fallo quedó resuelto. Tras la publicación, back-merge obligatorio de `main` hacia `test/v3` y `dev/v3` para no perder el fix.

---

## Convenciones de código

- Seguimos [PEP 8](https://pep8.org/) para el estilo de código Python.
- Usamos **type hints** donde sea posible.
- Todo código nuevo debe incluir **docstrings** básicos.
- Si agregás una feature, incluí al menos un **test** que la cubra.
- Evitá introducir dependencias nuevas sin justificación y discusión previa en la issue correspondiente.
- No commitees artefactos de build (`dist/`, `*.egg-info`) ni archivos de entorno local.

---

## Problemas frecuentes

| Síntoma | Causa | Solución |
|---------|-------|----------|
| El PR quedó en rojo en `ci-ok` | Falló alguno de los jobs que agrupa | Abrí `ci-ok` para ver cuál, y reproducilo en local con el [Paso 5.2](#paso-5-probar-antes-de-abrir-el-pr) |
| El PR quedó en rojo en `Lint (ruff)` | Lint o formato | `ruff check --fix .` y `ruff format .` |
| Un test de snapshot falla después de editar una plantilla | El render ya no coincide con la referencia | Revisá el diff; si el render nuevo es el correcto, `UPDATE_SNAPSHOTS=1 pytest tests/test_templates.py` y commiteá el snapshot |
| La issue no se cerró al mergear el PR | El PR mergeó a `dev/v3`, no a la rama por defecto | Es lo esperado: se cierra en el PR de release a `main` |
| `rkd --version` muestra `dev` | Paquete instalado sin metadata (checkout editable) | Reinstalar con `pipx install ... --force` |
| Conflicto en `pyproject.toml` al rebasear | Tu rama tocó la versión | Descartá ese cambio: el bump no va en las ramas de trabajo |
| `rkd scaffold` falla en el directorio de prueba | La carpeta no estaba vacía | Rocketdoo genera la estructura desde cero; usá una carpeta nueva |
| `gh: command not found` | GitHub CLI no instalada | Instalala desde [cli.github.com](https://cli.github.com/) o hacé todo el flujo desde la web |

---

## ¿Dudas?

Abrí una [Discussion en GitHub](https://github.com/HDM-soft/rocketdoo/discussions) o contactá a los mantenedores del proyecto. ¡Toda contribución, por pequeña que sea, es bienvenida!

---

*Este documento es parte de la documentación oficial de Rocketdoo.*
