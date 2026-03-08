# 🚀 Guía de Contribución a Rocketdoo

¡Gracias por tu interés en contribuir a **Rocketdoo**! Esta guía te explica todo lo que necesitás saber para sumarte al proyecto de manera ordenada y efectiva.

---

## Tabla de contenidos

- [Estructura de branches](#estructura-de-branches)
- [Configurar el entorno de desarrollo](#configurar-el-entorno-de-desarrollo)
- [Instalar Rocketdoo en modo desarrollo](#instalar-rocketdoo-en-modo-desarrollo)
- [Probar tu rama sin afectar la versión productiva](#probar-tu-rama-sin-afectar-la-versión-productiva)
- [Flujo de trabajo paso a paso](#flujo-de-trabajo-paso-a-paso)
- [Convenciones de código](#convenciones-de-código)
- [Cómo abrir un Pull Request](#cómo-abrir-un-pull-request)
- [Reportar bugs o sugerir mejoras](#reportar-bugs-o-sugerir-mejoras)

---

## Estructura de branches

Rocketdoo utiliza el siguiente modelo de ramas:

```
main              ← versión estable, publicada en PyPI
  └── dev/v2      ← desarrollo activo de la próxima versión
        └── feature/nombre-de-la-feature   ← tu contribución
        └── fix/nombre-del-bug
        └── chore/tarea-de-mantenimiento
```

> **Regla de oro:** nunca trabajar directamente sobre `main` ni `dev/v2`. Siempre crear una rama nueva desde `dev/v2`.

---

## Configurar el entorno de desarrollo

### Requisitos previos

- Python 3.8 o superior
- Git
- `pip` actualizado (`pip install --upgrade pip`)

### 1. Hacer fork del repositorio

Desde GitHub, hacé clic en **Fork** en la esquina superior derecha del repositorio de Rocketdoo.

### 2. Clonar tu fork localmente

```bash
git clone https://github.com/TU_USUARIO/rocketdoo.git
cd rocketdoo
```

### 3. Agregar el repositorio original como remote

```bash
git remote add upstream https://github.com/OWNER/rocketdoo.git
```

Esto te permite sincronizarte con los cambios del repo original cuando sea necesario:

```bash
git fetch upstream
git merge upstream/dev/v2
```

---

## Instalar Rocketdoo en modo desarrollo

Para poder modificar el código y ver los cambios reflejados de inmediato, instalá el paquete en **modo editable** dentro de un entorno virtual dedicado.

### Crear un entorno virtual exclusivo para desarrollo

```bash
# Desde la raíz del repo clonado
python -m venv venv-dev
```

Activarlo:

```bash
# Linux / macOS
source venv-dev/bin/activate

# Windows
.\venv-dev\Scripts\activate
```

### Instalar la rama de desarrollo

Una vez activado el `venv-dev`, instalá la rama específica directamente desde GitHub:

```bash
pip install git+https://github.com/OWNER/rocketdoo.git@feature/mi-feature
```

### Probar los comandos en un directorio vacío

Rocketdoo requiere comenzar desde un **directorio vacío**, ya que el primer comando (`rkd scaffold`) es el que genera toda la estructura del proyecto. Por eso el entorno de prueba debe ser una carpeta nueva y limpia:

```bash
# Crear carpeta de prueba vacía
mkdir mi-prueba && cd mi-prueba

# Ejecutar el primer comando de Rocketdoo
rkd scaffold
```

A partir de ahí podés continuar probando el resto de los comandos sobre la estructura generada por `scaffold`.

---

## Probar tu rama sin afectar la versión productiva

El método recomendado es instalar la rama directamente desde GitHub en una **carpeta de prueba vacía**, sin necesidad de clonar el repo completo. Así podés ejecutar todos los comandos de Rocketdoo en un entorno limpio y aislado.

### Opción A — Con `pipx` (recomendado para CLI) ✅

`pipx` es ideal porque instala Rocketdoo en un entorno aislado y expone los comandos globalmente. Usando `--suffix` podés tener la versión productiva y la de prueba **al mismo tiempo**, sin conflictos:

```bash
# Instalar la rama de prueba con un sufijo para distinguirla
pipx install git+https://github.com/OWNER/rocketdoo.git@feature/mi-feature --suffix=-test
```

Esto te da dos comandos disponibles en paralelo:

```bash
rocketdoo        # ← versión productiva instalada desde PyPI (intacta)
rocketdoo-test   # ← versión de tu rama de prueba
```

Para actualizar cuando pusheás nuevos cambios a la rama:

```bash
pipx reinstall rocketdoo-test
```

Para desinstalar cuando terminaste de probar:

```bash
pipx uninstall rocketdoo-test
```

---

### Opción B — Con `pip` dentro de un `venv` (para probar imports o scripts)

Si necesitás probar integraciones de Python además de los comandos CLI:

```bash
# Crear carpeta de prueba vacía
mkdir test-rocketdoo && cd test-rocketdoo

# Crear y activar un venv exclusivo
python -m venv venv-test
source venv-test/bin/activate   # Linux / macOS
# .\venv-test\Scripts\activate  # Windows

# Instalar la rama específica
pip install git+https://github.com/OWNER/rocketdoo.git@feature/mi-feature
```

Para actualizar cuando pusheás cambios:

```bash
pip install --force-reinstall git+https://github.com/OWNER/rocketdoo.git@feature/mi-feature
```

---

### Repo privado: autenticación con token

Si el repositorio es privado, necesitás un token de acceso personal de GitHub:

```bash
pip install git+https://TU_TOKEN@github.com/OWNER/rocketdoo.git@feature/mi-feature
```

---

### Resumen

| Método | Cuándo usarlo |
|--------|--------------|
| `pipx install git+... --suffix=-test` | **Recomendado.** Probar comandos CLI sin tocar la versión productiva |
| `pip install git+...` dentro de venv | Probar imports de Python o integraciones en scripts |

---

## Flujo de trabajo paso a paso

### 1. Sincronizate con la última versión de `dev/v2`

```bash
git checkout dev/v2
git pull upstream dev/v2
```

### 2. Crear tu rama de trabajo

```bash
git checkout -b feature/nombre-descriptivo
# Ejemplos:
# git checkout -b feature/soporte-multi-modelo
# git checkout -b fix/error-en-parser
# git checkout -b chore/actualizar-dependencias
```

### 3. Desarrollar y testear

Hacé tus cambios, ejecutá los tests y asegurate de que todo funcione:

```bash
# Ejecutar tests (ajustar según el runner del proyecto)
pytest
# o
python -m pytest tests/
```

### 4. Hacer commits descriptivos

Usamos el estilo [Conventional Commits](https://www.conventionalcommits.org/):

```bash
git add .
git commit -m "feat: agregar soporte para modelos locales via Ollama"
git commit -m "fix: corregir error de encoding en respuestas largas"
git commit -m "docs: actualizar README con nuevo ejemplo de uso"
git commit -m "chore: actualizar versión de httpx a 0.27"
```

**Tipos de commit:**
| Tipo | Cuándo usarlo |
|------|--------------|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un bug |
| `docs` | Cambios en documentación |
| `chore` | Tareas de mantenimiento (deps, CI, etc.) |
| `refactor` | Refactorización sin cambio de comportamiento |
| `test` | Agregar o corregir tests |
| `style` | Cambios de formato (sin lógica) |

### 5. Subir tu rama al fork

```bash
git push origin feature/nombre-descriptivo
```

### 6. Abrir el Pull Request

Abrí el PR desde tu fork hacia `dev/v2` del repositorio original (no hacia `main`).

---

## Convenciones de código

- Seguimos [PEP 8](https://pep8.org/) para estilo de código Python.
- Usamos **type hints** donde sea posible.
- Todo código nuevo debe incluir **docstrings** básicos.
- Si agregás una feature, incluí al menos un **test** que la cubra.
- Evitá introducir dependencias nuevas sin justificación y discusión previa en el issue correspondiente.

---

## Cómo abrir un Pull Request

Al abrir el PR, por favor incluí:

1. **Descripción clara** de qué cambia y por qué.
2. **Issue relacionado** si existe (ej: `Closes #42`).
3. **Cómo probar** los cambios: comandos, casos de prueba, etc.
4. Si aplica, **capturas de pantalla** o ejemplos de output.

El PR debe apuntar a `dev/v2`, no a `main`. Los mantenedores revisarán el código, pueden solicitar cambios, y cuando esté aprobado lo mergearán.

---

## Reportar bugs o sugerir mejoras

Antes de abrir un issue, buscá si ya existe uno similar. Si no, abrí uno nuevo con:

- **Descripción del problema** o la mejora propuesta.
- **Versión de Rocketdoo** (`pip show rocketdoo`).
- **Versión de Python** (`python --version`).
- **Sistema operativo**.
- **Pasos para reproducir** el bug (si aplica).
- **Comportamiento esperado vs. comportamiento actual**.

---

## ¿Dudas?

Abrí una [Discussion en GitHub](https://github.com/OWNER/rocketdoo/discussions) o contactá a los mantenedores del proyecto. ¡Toda contribución, por pequeña que sea, es bienvenida!

---

*Este documento es parte de la documentación oficial de Rocketdoo.*
