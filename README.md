# 📘 Guía Consolidada de GitHub

Este documento reúne de forma estructurada y sin duplicados todo el contenido esencial sobre el uso de GitHub, incluyendo seguridad, automatización, releases, paquetes, organizaciones y más.

---

## 📚 Índice General

- [🚀 Publicación de Paquetes en GitHub y PyPI](#-publicación-de-paquetes-en-github-y-pypi)
- [🚀 Introducción a los GitHub Releases](#-introducción-a-los-github-releases)
- [🚀 Revisión y Fusión de Pull Requests en GitHub](#-revisión-y-fusión-de-pull-requests-en-github)
- [🧰 Guía Completa de GitHub CLI (gh)](#-guía-completa-de-github-cli-gh)
- [⚙️ Automatización de Procesos con GitHub Actions](#️-automatización-de-procesos-con-github-actions)
- [🏢 Administración de Organizaciones en GitHub](#-administración-de-organizaciones-en-github)
- [🔐 Mantenimiento de Repositorios Seguros en GitHub](#-mantenimiento-de-repositorios-seguros-en-github)
- [🔒 Gestión de Dependencias y Seguridad con Dependabot](#-gestión-de-dependencias-y-seguridad-con-dependabot)
- [🔐 Cómo Usar Tokens en GitHub para Acceso Seguro a Repositorios Privados](#-cómo-usar-tokens-en-github-para-acceso-seguro-a-repositorios-privados)

---

Puedes ayudarme por favor a consolidar todo esto en un solo documento por favor:

# 🚀 Publicación de Paquetes en GitHub y PyPI

Publicar un paquete en GitHub o en PyPI te permite compartir código reutilizable, distribuir fácilmente tus librerías y facilitar la instalación desde cualquier entorno.

---

## 📚 Índice

- [📌 ¿Por qué publicar paquetes?](##-📌-por-qué-publicar-paquetes)
- [🔹 Publicación en GitHub Packages](#🔹-publicación-en-github-packages)
  - [1️⃣ Configurar el archivo setup.py](#1️⃣-configurar-el-archivo-setuppy)
  - [2️⃣ Crear archivo .pypirc](#2️⃣-crear-archivo-pypirc)
  - [3️⃣ Construir y subir el paquete](#3️⃣-construir-y-subir-el-paquete)
- [🔹 Publicación en PyPI (Python Package Index)](#🔹-publicación-en-pypi-python-package-index)
  - [1️⃣ Crear cuenta en PyPI](#1️⃣-crear-cuenta-en-pypi)
  - [2️⃣ Crear setup.py y pyproject.toml](#2️⃣-crear-setuppy-y-pyprojecttoml)
  - [3️⃣ Construir y subir el paquete](#3️⃣-construir-y-subir-el-paquete-1)
- [📥 Instalación de Paquetes desde GitHub o PyPI](#📥-instalación-de-paquetes-desde-github-o-pypi)
- [📊 Diferencias clave entre GitHub Packages y PyPI](#📊-diferencias-clave-entre-github-packages-y-pypi)
- [✅ Conclusión](#✅-conclusión)
- [📚 Recursos oficiales](#📚-recursos-oficiales)

---

## 📌 ¿Por qué publicar paquetes?

Publicar tus librerías o herramientas como paquetes permite:

✅ Reutilizar código fácilmente  
✅ Compartir librerías con tu equipo o la comunidad  
✅ Instalar dependencias con un simple `pip install`  
✅ Automatizar despliegues de código en pipelines CI/CD

---

## 🔹 Publicación en GitHub Packages

GitHub Packages permite subir paquetes directamente a tu cuenta de GitHub, útiles especialmente para repositorios privados o internos.

---

### 1️⃣ Configurar el archivo `setup.py`

Crea el archivo `setup.py` en la raíz del proyecto:

```python
from setuptools import setup, find_packages

setup(
    name="miproyecto",
    version="0.1.0",
    packages=find_packages(),
    install_requires=[
        "numpy",  # Dependencias del paquete
    ],
    author="Tu Nombre",
    author_email="tuemail@example.com",
    description="Descripción breve del paquete",
    url="https://github.com/tuusuario/miproyecto",
)
```

---

### 2️⃣ Crear archivo `.pypirc`

Ubicación:

- Linux/Mac: `~/.pypirc`
- Windows: `C:\Users\TU_USUARIO\.pypirc`

Contenido:

```ini
[distutils]
index-servers =
    github

[github]
repository = https://upload.pypi.org/legacy/
username = __token__
password = TU_GITHUB_TOKEN
```

💡 Genera tu token en:  
`GitHub → Settings → Developer settings → Personal access tokens`  
Asegúrate de incluir permisos: `write:packages` y `read:packages`

---

### 3️⃣ Construir y subir el paquete

Instala herramientas necesarias:

```bash
pip install build twine
```

Construye el paquete:

```bash
python -m build
```

Sube a GitHub Packages:

```bash
twine upload --repository github dist/*
```

---

## 🔹 Publicación en PyPI (Python Package Index)

Ideal para publicar paquetes abiertos y disponibles para toda la comunidad Python.

---

### 1️⃣ Crear cuenta en PyPI

👉 Visita [https://pypi.org/account/register/](https://pypi.org/account/register/)  
Verifica tu correo para activar la cuenta.

---

### 2️⃣ Crear `setup.py` y `pyproject.toml`

#### `setup.py`

(Similar al ejemplo anterior)

#### `pyproject.toml`

```toml
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"
```

Este archivo es necesario para construir correctamente tu paquete.

---

### 3️⃣ Construir y subir el paquete

Instala herramientas si aún no las tienes:

```bash
pip install build twine
```

Construye:

```bash
python -m build
```

Sube a PyPI:

```bash
twine upload dist/*
```

🔐 Usa tu usuario y contraseña de PyPI (o token de acceso generado desde tu perfil).

---

## 📥 Instalación de Paquetes desde GitHub o PyPI

### ✅ Desde PyPI (público):

```bash
pip install miproyecto
```

### ✅ Desde GitHub (repositorio público):

```bash
pip install git+https://github.com/tuusuario/miproyecto.git
```

### ✅ Desde GitHub Packages (privado):

```bash
pip install --index-url https://pypi.github.com/tuusuario miproyecto
```

💡 Para esto necesitas autenticación previa usando un token.

---

## 📊 Diferencias clave entre GitHub Packages y PyPI

| Característica       | GitHub Packages                          | PyPI                                |
|----------------------|-------------------------------------------|--------------------------------------|
| Visibilidad          | Público o privado                         | Solo público                         |
| Integración          | Directa con repositorios GitHub           | Independiente de GitHub              |
| Autenticación        | Necesaria incluso para paquetes públicos  | Solo para subir, no para instalar    |
| Ideal para           | Proyectos internos, privados              | Librerías open-source                |

---

## ✅ Conclusión

✅ Usa **PyPI** para compartir paquetes públicos con la comunidad Python  
✅ Usa **GitHub Packages** para distribución interna o privada  
✅ Automatiza el proceso con herramientas como **twine** y **build**  
✅ Define bien tu `setup.py` y `pyproject.toml` para cumplir con estándares  
✅ Gestiona versiones semánticas (`v1.0.0`) y distribuye builds fácilmente

---

## 📚 Recursos oficiales

- [Documentación oficial de PyPI](https://packaging.python.org/)
- [GitHub Packages](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-python-registry)
- [twine (upload tool)](https://twine.readthedocs.io/en/latest/)
- [build (PEP 517)](https://pypa-build.readthedocs.io/en/latest/)

# 🚀 Introducción a los GitHub Releases

**GitHub Releases** es una funcionalidad que te permite empaquetar y distribuir versiones específicas de tu proyecto. Ideal para gestionar el ciclo de vida del software, documentar los cambios y distribuir binarios de forma profesional.

---

## 📚 Índice

- [📦 ¿Qué son los GitHub Releases?](#📦-qué-son-los-github-releases)
- [🎯 ¿Para qué sirven?](#🎯-para-qué-sirven)
- [📌 Diferencia entre Tags y Releases](#📌-diferencia-entre-tags-y-releases)
- [🛠 Cómo crear un GitHub Release](#🛠-cómo-crear-un-github-release)
  - [Opción 1: Interfaz web de GitHub](#opción-1-interfaz-web-de-github)
  - [Opción 2: Git CLI](#opción-2-git-cli)
  - [Opción 3: GitHub CLI (gh)](#opción-3-github-cli-gh)
- [📦 Casos de Uso Comunes](#📦-casos-de-uso-comunes)
- [🤖 Automatización con GitHub Actions](#🤖-automatización-con-github-actions)
- [✅ Conclusión](#✅-conclusión)
- [📚 Recursos Recomendados](#📚-recursos-recomendados)

---

## 📦 ¿Qué son los GitHub Releases?

Un **Release** es una versión de tu proyecto asociada a un **tag de Git**, que incluye:

- Descripción o notas de la versión
- Archivos adjuntos (como binarios o instaladores)
- Código fuente asociado al tag

---

## 🎯 ¿Para qué sirven?

✅ Marcar versiones estables de un proyecto (v1.0.0, v2.1.0)  
✅ Distribuir binarios, ejecutables o assets  
✅ Documentar cambios importantes (release notes)  
✅ Automatizar despliegues con CI/CD  
✅ Facilitar a los usuarios la descarga de versiones específicas

---

## 📌 Diferencia entre Tags y Releases

| Concepto | Descripción |
|----------|-------------|
| **Tag** | Marca específica en el historial de commits. No tiene archivos adjuntos ni descripción larga. |
| **Release** | Publicación creada a partir de un tag, que puede tener notas, documentación y binarios. |

Ejemplo:  
Tag `v1.0.0` → asociado a un commit específico  
Release `v1.0.0` → empaqueta ese tag + descripción + archivos

---

## 🛠 Cómo crear un GitHub Release

### Opción 1: Interfaz web de GitHub

1️⃣ Ve al repositorio → pestaña **Releases**  
2️⃣ Haz clic en **"Draft a new release"**  
3️⃣ Selecciona o crea un **tag** (ej: `v1.0.0`)  
4️⃣ Agrega un **título** y **notas de la versión** (changelog, mejoras, bugs, etc.)  
5️⃣ (Opcional) Adjunta binarios (`.zip`, `.exe`, `.apk`, etc.)  
6️⃣ Haz clic en **"Publish release"**

---

### Opción 2: Git CLI

```bash
# Crear un tag con mensaje
git tag -a v1.0.0 -m "Primera versión estable"

# Subir el tag al repositorio remoto
git push origin v1.0.0
```

Después de subir el tag, puedes ir a GitHub → pestaña **Releases** y hacer clic en **"Draft a new release"** seleccionando ese tag.

---

### Opción 3: GitHub CLI (gh)

```bash
gh release create v1.0.0 \
  --title "Versión 1.0.0" \
  --notes "🎉 Esta versión incluye: mejoras en login, correcciones de errores y nuevas dependencias actualizadas."
```

✅ También puedes añadir archivos adjuntos:

```bash
gh release create v1.1.0 ./build/app.zip --title "v1.1.0" --notes "Versión con build compilado."
```

---

## 📦 Casos de Uso Comunes

- Publicar **librerías** (npm, Python, PHP, etc.) con cada versión
- Distribuir instaladores de aplicaciones (`.exe`, `.dmg`, `.apk`, etc.)
- Entregar paquetes binarios o `.zip` a tus usuarios
- Mantener una **historia clara** del desarrollo usando versionado semántico (`v1.0.0`, `v1.1.0`, `v2.0.0`)
- Automatizar despliegues con GitHub Actions

---

## 🤖 Automatización con GitHub Actions

Puedes crear releases automáticos desde GitHub Actions al hacer push a una nueva versión con tag.

```yaml
name: Crear Release al hacer push a un tag

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Crear release
        uses: softprops/action-gh-release@v1
        with:
          name: "Release ${{ github.ref_name }}"
          tag_name: ${{ github.ref_name }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

✅ Este flujo:

- Se activa solo cuando haces `git push origin v1.2.3`
- Crea un release automáticamente con ese tag
- Usa el token de GitHub para autenticarse

---

## ✅ Conclusión

🔹 GitHub Releases es una herramienta esencial para publicar versiones estables de software.  
🔹 Permite mantener documentación clara con notas de versión.  
🔹 Facilita la distribución de binarios o builds a tus usuarios.  
🔹 Funciona perfectamente con Git CLI, GitHub CLI y GitHub Actions para automatizar todo el proceso.

---

## 📚 Recursos Recomendados

- [GitHub Releases - Documentación oficial](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)  
- [Guía: Cómo usar Tags en Git](https://git-scm.com/book/en/v2/Git-Basics-Tagging)  
- [GitHub CLI - gh release](https://cli.github.com/manual/gh_release)

# 🚀 Revisión y Fusión de Pull Requests en GitHub

Los **Pull Requests (PRs)** son fundamentales en el flujo de trabajo colaborativo de GitHub. Permiten que los desarrolladores propongan cambios, otros los revisen y finalmente se integren en la rama principal del proyecto.

Un buen proceso de revisión mejora la calidad del código, previene errores en producción y fortalece la colaboración del equipo.

---

## 📚 Índice

- [🔍 ¿Qué es un Pull Request?](#🔍-qué-es-un-pull-request)
- [📌 Revisión de Pull Requests](#📌-revisión-de-pull-requests)
  - [🧭 Pasos para revisar un PR](#🧭-pasos-para-revisar-un-pr)
  - [💻 Ejemplo usando GitHub CLI](#💻-ejemplo-usando-github-cli)
- [🔀 Fusión de Pull Requests](#🔀-fusión-de-pull-requests)
  - [📄 Tipos de fusión disponibles](#📄-tipos-de-fusión-disponibles)
  - [💻 Ejemplo de fusión desde la CLI](#💻-ejemplo-de-fusión-desde-la-cli)
- [✅ Buenas Prácticas](#✅-buenas-prácticas)
- [📚 Recursos adicionales](#📚-recursos-adicionales)

---

## 🔍 ¿Qué es un Pull Request?

Un **Pull Request (PR)** es una solicitud para fusionar cambios de una rama a otra (por ejemplo, de `feature/login` a `main`). Los PR permiten:

- Revisar código antes de integrarlo
- Ejecutar pruebas automáticas (CI/CD)
- Discutir y colaborar sobre los cambios propuestos
- Mantener el historial del proyecto limpio y rastreable

---

## 📌 Revisión de Pull Requests

La revisión implica analizar el código propuesto en el PR, verificar su funcionalidad, detectar errores, sugerir mejoras y validar su calidad antes de fusionarlo.

---

### 🧭 Pasos para revisar un PR

1. Ve a la pestaña `Pull requests` del repositorio.
2. Haz clic sobre el PR que deseas revisar.
3. Lee la descripción para entender el contexto de los cambios.
4. Revisa los archivos en la pestaña `Files changed`.
5. Agrega comentarios línea por línea si encuentras algo que mejorar.
6. Ejecuta las pruebas localmente o verifica que los checks automáticos pasen.
7. Elige una de las siguientes opciones:
   - ✅ **Approve**: si todo está bien.
   - 🔄 **Request changes**: si se requieren ajustes antes de aprobar.
   - 💬 **Comment**: para dejar feedback sin aceptar ni rechazar.

---

### 💻 Ejemplo usando GitHub CLI

```bash
# Ver lista de PRs abiertos
gh pr list

# Ver detalles de un PR específico
gh pr view 123

# Dejar un comentario en un PR
gh pr comment 123 --body "Este código se ve bien, pero considera mejorar la validación en la línea 42."

# Aprobar un PR
gh pr review 123 --approve

# Solicitar cambios
gh pr review 123 --request-changes --body "Falta validar el caso cuando el usuario no tiene sesión activa."
```

✅ Puedes revisar, comentar y aprobar sin salir de la terminal, ideal para equipos técnicos.

---

## 🔀 Fusión de Pull Requests

Una vez aprobado el PR, se puede **fusionar (merge)** con la rama de destino (`main`, `develop`, etc.).

---

### 📄 Tipos de fusión disponibles

1. **Merge commit (`--merge`)**  
   ➕ Mantiene todo el historial de commits  
   ➖ Puede llenar el historial si hay muchos cambios pequeños

2. **Squash and merge (`--squash`)**  
   ➕ Une todos los commits del PR en uno solo  
   ➕ Ideal para mantener un historial más limpio

3. **Rebase and merge (`--rebase`)**  
   ➕ Aplica los commits sobre la rama de destino  
   ➕ Crea un historial lineal  
   ➖ Puede ser más difícil de entender en equipos grandes

---

### 💻 Ejemplo de fusión desde la CLI

```bash
# Merge normal (merge commit)
gh pr merge 123 --merge

# Squash and merge
gh pr merge 123 --squash

# Rebase and merge
gh pr merge 123 --rebase
```

> Puedes agregar `--admin` si necesitas forzar el merge con permisos de administrador.

---

## ✅ Buenas Prácticas

✔ **Revisa con atención** el código para evitar errores en producción  
✔ **Comenta de forma constructiva**, clara y específica  
✔ **Asegúrate de que las pruebas automáticas pasen** antes de fusionar  
✔ **Usa linters y herramientas de análisis** para mantener calidad  
✔ **Elige la estrategia de fusión según la política del equipo**  
✔ **Evita merges directos a `main` sin revisión previa**  
✔ **Documenta las decisiones técnicas en la conversación del PR**

---

## 📚 Recursos adicionales

- [GitHub Docs: Reviewing Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests)
- [GitHub CLI: Pull Requests](https://cli.github.com/manual/gh_pr)
- [Best practices for code review](https://google.github.io/eng-practices/review/)

---

## 💡 Frase clave

> "Una revisión efectiva y una fusión bien gestionada garantizan código limpio, estable y fácil de mantener." 🚀

# 🧰 Guía Completa de GitHub CLI (gh)

La **GitHub CLI (`gh`)** es una herramienta poderosa que te permite interactuar con GitHub directamente desde la terminal. Con ella puedes clonar, crear, administrar repositorios, issues, pull requests, y mucho más, sin tener que abrir el navegador.

---

## 📚 Índice

- [🛠 Instalación de GitHub CLI](#🛠-instalación-de-github-cli)
- [🔐 Autenticación con GitHub](#🔐-autenticación-con-github)
- [🚀 Comandos Básicos](#🚀-comandos-básicos)
  - [📁 Repositorios](#📁-repositorios)
  - [🐛 Issues](#🐛-issues)
  - [🔀 Pull Requests](#🔀-pull-requests)
  - [📊 Información del Repositorio](#📊-información-del-repositorio)
- [🔄 Integración con Git](#🔄-integración-con-git)
- [📜 Ver Todos los Comandos Disponibles](#📜-ver-todos-los-comandos-disponibles)
- [🎯 Conclusión](#🎯-conclusión)

---

## 🛠 Instalación de GitHub CLI

Instala `gh` en tu sistema operativo según corresponda:

### Para macOS (con Homebrew)

```bash
brew install gh
```

### Para Windows (con Chocolatey)

```bash
choco install gh
```

### Para Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install gh
```

También puedes consultar la [página oficial de instalación](https://cli.github.com/manual/installation) para otras distros o gestores.

---

## 🔐 Autenticación con GitHub

Después de instalar, debes autenticarte para usar todas las funcionalidades:

```bash
gh auth login
```

### 🔁 ¿Qué sucede?

1. Te preguntará si deseas usar GitHub.com o GitHub Enterprise.
2. Elegirás si quieres autenticación por navegador o token.
3. Si eliges navegador, se abrirá una URL para iniciar sesión.
4. Una vez confirmado, tu CLI quedará autenticada.

✅ Puedes verificar tu sesión con:

```bash
gh auth status
```

---

## 🚀 Comandos Básicos

### 📁 Repositorios

**Clonar un repositorio:**

```bash
gh repo clone usuario/repositorio
```

**Crear un nuevo repositorio:**

```bash
gh repo create nombre-del-repo --private
```

**Ver tus repos o los de otro usuario:**

```bash
gh repo list
gh repo list usuario
```

---

### 🐛 Issues

**Listar issues de un repositorio:**

```bash
gh issue list
```

**Crear un nuevo issue:**

```bash
gh issue create --title "Nuevo bug" --body "Descripción del error"
```

**Asignar un issue a un usuario:**

```bash
gh issue edit <número> --add-assignee usuario
```

---

### 🔀 Pull Requests

**Listar pull requests abiertas:**

```bash
gh pr list
```

**Crear una nueva pull request:**

```bash
gh pr create --title "Nueva característica" \
             --body "Implementa la funcionalidad X" \
             --base main \
             --head feature-branch
```

**Revisar una pull request:**

```bash
gh pr review <número> --approve
```

**Merge de una PR:**

```bash
gh pr merge <número> --merge
```

---

### 📊 Información del Repositorio

**Ver información general del repo:**

```bash
gh repo view usuario/repositorio --web
```

**Ver actividad reciente:**

```bash
gh activity
```

**Ver eventos del repositorio:**

```bash
gh repo view --web
```

---

## 🔄 Integración con Git

GitHub CLI complementa Git perfectamente:

1. Puedes hacer `git push` y luego abrir un Pull Request con:

```bash
gh pr create
```

2. Puedes revisar y comentar issues desde la terminal.
3. Es ideal para automatizar flujos en equipos técnicos que usan Git y GitHub juntos.

---

## 📜 Ver Todos los Comandos Disponibles

```bash
gh help
```

O también puedes explorar comandos por tema:

```bash
gh repo --help
gh pr --help
gh issue --help
```

---

## 🧪 Ejemplo completo de flujo con GitHub CLI

```bash
# Clonar un repo
gh repo clone misadev/proyecto-demo

# Crear una nueva rama
git checkout -b feature/login

# Realizar cambios, luego...
git add .
git commit -m "Agrega componente de login"
git push --set-upstream origin feature/login

# Crear Pull Request desde terminal
gh pr create --title "Login feature" --body "Agrega login funcional con validación" --base main --head feature/login

# Revisar PRs abiertos
gh pr list
```

✅ Así puedes trabajar **de principio a fin** sin abrir el navegador.

---

## 🎯 Conclusión

✅ GitHub CLI optimiza tu flujo de trabajo en la terminal  
✅ Ahorra tiempo en tareas comunes como crear issues, clonar, revisar y hacer merge  
✅ Permite trabajar en colaboración sin salir de tu editor o consola  
✅ Ideal para automatización, DevOps, desarrollo ágil y flujos personalizados

---

## 📚 Recursos útiles

- [GitHub CLI - Documentación oficial](https://cli.github.com/manual/)
- [GitHub CLI en GitHub](https://github.com/cli/cli)
- [Guía interactiva de comandos](https://cli.github.com/manual/gh)

# ⚙️ Automatización de Procesos con GitHub Actions

**GitHub Actions** es una herramienta poderosa para automatizar flujos de trabajo directamente en tu repositorio. Permite implementar prácticas de integración continua (CI) y entrega continua (CD) sin salir de GitHub.

---

## 📚 Índice

- [🚀 ¿Qué es GitHub Actions?](#🚀-qué-es-github-actions)
- [🎯 Beneficios clave](#🎯-beneficios-clave)
- [🛠 Estructura básica de un workflow](#🛠-estructura-básica-de-un-workflow)
- [✅ Ejemplo práctico: Ejecutar pruebas automáticamente en cada push](#✅-ejemplo-práctico-ejecutar-pruebas-automáticamente-en-cada-push)
  - [Paso 1: Crear el archivo del workflow](#paso-1-crear-el-archivo-del-workflow)
  - [Paso 2: Explicación del flujo paso a paso](#paso-2-explicación-del-flujo-paso-a-paso)
  - [Paso 3: Ver el resultado en GitHub](#paso-3-ver-el-resultado-en-github)
- [📦 Usar el Marketplace de Actions](#📦-usar-el-marketplace-de-actions)
- [🔐 Buenas prácticas](#🔐-buenas-prácticas)
- [📚 Recursos oficiales](#📚-recursos-oficiales)

---

## 🚀 ¿Qué es GitHub Actions?

GitHub Actions te permite automatizar tareas como:

- Pruebas automáticas
- Builds de producción
- Deploys automáticos
- Escaneo de seguridad
- Envío de notificaciones
- Actualizaciones automáticas de dependencias

Todo esto sucede **dentro del ecosistema de GitHub**, utilizando archivos de configuración en formato YAML dentro del repositorio.

---

## 🎯 Beneficios clave

✅ **Automatización**: Ejecuta tareas sin intervención manual (CI/CD)  
✅ **Integración nativa**: No necesitas herramientas externas para automatizar  
✅ **Personalización total**: Puedes definir eventos, condiciones y pasos  
✅ **Marketplace**: Accede a miles de acciones creadas por la comunidad para tareas comunes

---

## 🛠 Estructura básica de un workflow

Los workflows se definen en archivos `.yml` dentro del directorio:

```
.github/workflows/
```

Ejemplo básico de estructura:

```yaml
name: Nombre del workflow

on: evento_disparador

jobs:
  nombre_job:
    runs-on: sistema_operativo
    steps:
      - name: Nombre del paso
        uses: acción_o_script
```

---

## ✅ Ejemplo práctico: Ejecutar pruebas automáticamente en cada push

### Objetivo:
Cada vez que haces `git push` al repositorio, se ejecutan los tests automáticamente usando Node.js.

---

### Paso 1: Crear el archivo del workflow

1. En tu proyecto, crea el archivo:

```
.github/workflows/test-ci.yml
```

2. Agrega este contenido:

```yaml
name: Ejecutar pruebas en cada push

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  run-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Clonar el repositorio
        uses: actions/checkout@v3

      - name: Configurar Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Instalar dependencias
        run: npm install

      - name: Ejecutar pruebas
        run: npm test
```

---

### Paso 2: Explicación del flujo paso a paso

- `name`: Nombre visible del workflow en la pestaña "Actions".
- `on`: Define los eventos que activan este flujo (push y pull request a `main`).
- `jobs`: Tareas que se ejecutan.
- `runs-on`: Elige un runner (máquina virtual) para ejecutar los pasos.
- `steps`: Lista de pasos que ocurren en orden:

  1. **checkout**: Clona tu código en el runner.
  2. **setup-node**: Prepara el entorno con la versión de Node.
  3. **npm install**: Instala dependencias.
  4. **npm test**: Ejecuta los tests definidos en tu proyecto.

---

### Paso 3: Ver el resultado en GitHub

1. Haz un commit al repositorio y realiza un `push`.
2. Ve a la pestaña **"Actions"** del repositorio.
3. Verás la ejecución del workflow en tiempo real.
4. Si las pruebas fallan, GitHub mostrará el error.
5. Si todo sale bien, verás un ✅ verde de éxito.

---

## 📦 Usar el Marketplace de Actions

Puedes aprovechar acciones de la comunidad para:

- Deploy a Vercel, Netlify, Firebase
- Publicar paquetes a npm o DockerHub
- Escanear dependencias o código
- Generar changelogs automáticos

👉 Visita [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)

Ejemplo para subir cobertura a Codecov:

```yaml
- name: Subir cobertura a Codecov
  uses: codecov/codecov-action@v3
```

---

## 🔐 Buenas prácticas

- Usa variables de entorno seguras (`secrets`) para API Keys, tokens, etc.
- Automatiza validaciones para evitar bugs en `main`.
- Usa diferentes jobs para stages (build, test, deploy).
- Agrega `if:` para ejecutar tareas solo en ciertos contextos.
- Utiliza matrices (`matrix`) para probar en múltiples versiones de Node, Python, etc.

---

## 📚 Recursos oficiales

- [Documentación de GitHub Actions](https://docs.github.com/en/actions)
- [Guía de CI/CD con GitHub](https://docs.github.com/en/actions/automating-builds-and-tests/about-continuous-integration)
- [YAML syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

---

## 🎯 Conclusión

✅ GitHub Actions permite automatizar pruebas, deploys y tareas repetitivas sin salir del repositorio.  
✅ Mejora la calidad del software con flujos CI/CD integrados.  
✅ Ahorra tiempo en revisiones y validaciones manuales.  
✅ El Marketplace y los secretos lo convierten en una herramienta robusta para desarrollo profesional.

# 🏢 Administración de Organizaciones en GitHub

GitHub permite gestionar equipos de desarrollo de manera eficiente a través de **Organizaciones**, proporcionando herramientas avanzadas para la colaboración, seguridad y control de acceso a los repositorios.

---

## 📚 Índice

- [1️⃣ ¿Qué es una Organización en GitHub?](#1️⃣-qué-es-una-organización-en-github)
- [2️⃣ Creación de una Organización en GitHub](#2️⃣-creación-de-una-organización-en-github)
- [3️⃣ Gestión de Miembros y Equipos](#3️⃣-gestión-de-miembros-y-equipos)
  - [👥 Roles disponibles](#👥-roles-disponibles)
  - [👥 Creación y gestión de equipos](#👥-creación-y-gestión-de-equipos)
- [4️⃣ Gestión de Repositorios en una Organización](#4️⃣-gestión-de-repositorios-en-una-organización)
- [5️⃣ Seguridad y Control en una Organización](#5️⃣-seguridad-y-control-en-una-organización)
- [6️⃣ Automatización con GitHub Actions](#6️⃣-automatización-con-github-actions)
- [🎯 Conclusión](#🎯-conclusión)

---

## 1️⃣ ¿Qué es una Organización en GitHub?

Una **Organización en GitHub** es un espacio de trabajo colaborativo donde varios usuarios pueden trabajar en uno o varios repositorios con roles, permisos y equipos personalizados.

### 🔹 Ventajas de usar una Organización

✅ Centraliza el desarrollo en equipos grandes  
✅ Mejora la gestión de accesos y permisos  
✅ Permite administrar múltiples repositorios desde un mismo lugar  
✅ Ofrece herramientas avanzadas de seguridad, control y auditoría  

---

## 2️⃣ Creación de una Organización en GitHub

### 📌 Pasos para crear una organización

1. Ve a: [https://github.com/organizations](https://github.com/organizations)  
2. Haz clic en **"New organization"**  
3. Selecciona un plan: Free, Team o Enterprise  
4. Asigna un nombre único a tu organización  
5. Invita a miembros o equipos  
6. Configura los permisos iniciales

✅ Listo, ahora puedes comenzar a crear y administrar repositorios dentro de la organización.

---

## 3️⃣ Gestión de Miembros y Equipos

### 👥 Roles disponibles

| Rol             | Permisos principales                                                                 |
|------------------|---------------------------------------------------------------------------------------|
| **Owner**        | Control total sobre la organización: repos, facturación, miembros, seguridad         |
| **Member**       | Acceso a repositorios según lo que se le asigne                                       |
| **Billing Manager** | Solo puede gestionar la facturación, no accede al código                          |

🛠 Configura roles desde:  
`Organization → Settings → People → Manage roles`

---

### 👥 Creación y gestión de equipos

1. Ve a: `Organization → Teams → New Team`  
2. Crea equipos por área (ej: Frontend, Backend, QA, DevOps)  
3. Asigna miembros a cada equipo  
4. Establece permisos sobre repos específicos

#### 📌 Ejemplo de niveles de acceso por equipo

- **Read:** Solo lectura del código  
- **Write:** Puede hacer commits, push y pull  
- **Admin:** Control total del repositorio (incluye settings)

✅ Esto permite mantener un control granular sobre los permisos según responsabilidades.

---

## 4️⃣ Gestión de Repositorios en una Organización

### 🔹 Crear y configurar repositorios

1. Desde la página principal de la organización, haz clic en **"Repositories" → "New"**  
2. Elige nombre, descripción y visibilidad:
   - **Public:** Accesible para todos en GitHub
   - **Private:** Solo para miembros autorizados
   - **Internal:** Solo visible dentro de la empresa (GitHub Enterprise)

### 🔹 Asignar acceso a repositorios

1. Ve al repositorio → `Settings → Manage Access`  
2. Agrega equipos o usuarios  
3. Asigna permisos: Read, Write, Admin

### 📌 Consejo

Protege ramas críticas como `main` o `release` con reglas de protección:

```bash
git branch -m main
git push --set-upstream origin main
```

Luego, en `Settings → Branches → Add rule` puedes:

- Requerir revisión de PRs
- Requerir pruebas exitosas
- Impedir force push o borrado accidental

---

## 5️⃣ Seguridad y Control en una Organización

### 🔑 Habilitar Autenticación en Dos Pasos (2FA)

1. Ve a: `Organization Settings → Security → Authentication security`  
2. Activa **"Require Two-Factor Authentication"**  
3. Esto obliga a todos los miembros a usar 2FA

✅ Previene accesos no autorizados incluso si una contraseña se ve comprometida.

---

### 🔎 Auditoría con GitHub Audit Log

En organizaciones (Team y Enterprise):

- Ve a: `Organization → Settings → Audit log`  
- Verás eventos como:
  - Cambios en repos
  - Agregado/quitado de usuarios
  - Uso de tokens API

✅ Útil para rastrear actividad sospechosa o cumplir con políticas de seguridad.

---

### 🚀 Activar GitHub Advanced Security

Funciones disponibles (en planes de pago o Enterprise):

- **Secret scanning:** Detecta claves, tokens y secretos en el código
- **Code scanning (CodeQL):** Encuentra vulnerabilidades en el código fuente
- **Dependabot:** Automatiza parches de seguridad en dependencias

✅ Estas herramientas ayudan a prevenir ataques por fuga de credenciales o uso de paquetes inseguros.

---

## 6️⃣ Automatización con GitHub Actions

Las organizaciones pueden usar **GitHub Actions** para automatizar tareas como:

- Pruebas automáticas en cada PR
- Despliegue continuo (CI/CD)
- Escaneo de seguridad
- Notificaciones y validaciones personalizadas

### 📄 Ejemplo: Workflow de CI/CD básico

```yaml
name: CI/CD Workflow

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Instalar dependencias
        run: npm install
      - name: Ejecutar pruebas
        run: npm test
```

✅ Puedes configurar diferentes workflows para diferentes repositorios o reutilizar plantillas entre ellos.

---

## 🎯 Conclusión

✅ Usar organizaciones en GitHub mejora la **colaboración, seguridad y escalabilidad** del desarrollo  
✅ Configurar equipos y roles permite una **gestión clara y controlada de accesos**  
✅ Implementar seguridad (2FA, auditoría, escaneo de código) **protege contra amenazas y errores humanos**  
✅ Automatizar con Actions **ahorra tiempo** y mejora la calidad del software

---

## 📚 Recursos Recomendados

- [Crear y gestionar organizaciones en GitHub](https://docs.github.com/en/organizations/collaborating-with-groups-in-your-organization/about-organizations)  
- [Configurar equipos y permisos](https://docs.github.com/en/organizations/managing-membership-in-your-organization/adding-people-to-your-organization)  
- [Protección de ramas](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)  
- [GitHub Actions Docs](https://docs.github.com/en/actions)

# 🔐 Mantenimiento de Repositorios Seguros en GitHub

Mantener un repositorio seguro en GitHub es fundamental para proteger el código, las credenciales y la integridad del proyecto. Esta guía cubre buenas prácticas, configuraciones y herramientas nativas de GitHub para fortalecer la seguridad de tus repositorios.

---

## 📚 Índice

- [1️⃣ Configuración de Seguridad en GitHub](#1️⃣-configuración-de-seguridad-en-github)
  - [📌 Autenticación en dos pasos (2FA)](#📌-autenticación-en-dos-pasos-2fa)
  - [📌 Uso de claves SSH y Tokens](#📌-uso-de-claves-ssh-y-tokens)
- [2️⃣ Control de Accesos y Permisos](#2️⃣-control-de-accesos-y-permisos)
  - [👥 Roles de los colaboradores](#👥-roles-de-los-colaboradores)
  - [🔒 Reglas de protección de ramas](#🔒-reglas-de-protección-de-ramas)
- [3️⃣ Protección contra Vulnerabilidades](#3️⃣-protección-contra-vulnerabilidades)
  - [⚠ Dependabot](#⚠-dependabot)
  - [🔎 Análisis de código y escaneo de secretos](#🔎-análisis-de-código-y-escaneo-de-secretos)
- [4️⃣ Buenas Prácticas para la Seguridad del Código](#4️⃣-buenas-prácticas-para-la-seguridad-del-código)
- [5️⃣ Monitoreo y Auditoría](#5️⃣-monitoreo-y-auditoría)
- [6️⃣ Recomendaciones adicionales de protección](#6️⃣-recomendaciones-adicionales-de-protección)
- [🎯 Conclusión](#🎯-conclusión)
- [📚 Recursos Recomendados](#📚-recursos-recomendados)

---

## 1️⃣ Configuración de Seguridad en GitHub

### 📌 Autenticación en dos pasos (2FA)

Activa el segundo factor para proteger tu cuenta contra accesos no autorizados:

1. Ve a `Settings → Password and authentication → Two-factor authentication`.
2. Configura tu método (app de autenticación o SMS).
3. Guarda tus códigos de recuperación en un lugar seguro.

---

### 📌 Uso de claves SSH y Tokens

**Claves SSH:** Para acceso seguro desde terminal.

```bash
ssh-keygen -t ed25519 -C "tu-email@example.com"
```

Luego agrega la clave pública en GitHub:  
`Settings → SSH and GPG Keys → New SSH key`

**Personal Access Tokens (PAT):** Para autenticarte con HTTPS o la API.

1. Ve a `Settings → Developer Settings → Personal Access Tokens`.
2. Genera un token con scopes mínimos necesarios (`repo`, `workflow`, etc.).
3. Úsalo como contraseña en Git o en tu CLI.

---

## 2️⃣ Control de Accesos y Permisos

### 👥 Roles de los colaboradores

Asigna permisos correctos según rol:

| Rol        | Permisos principales                      |
|------------|--------------------------------------------|
| Admin      | Control total sobre el repositorio         |
| Maintainer | Gestión de Issues y Pull Requests          |
| Developer  | Puede hacer commits y forks                |
| Read-only  | Solo lectura del código                    |

Configura desde: `Settings → Collaborators` o equipos en organizaciones.

---

### 🔒 Reglas de protección de ramas

1. Ve a `Settings → Branches → Add rule`.
2. Selecciona rama (`main`, `develop`, etc.).
3. Activa:
   - `Require pull request before merging`
   - `Require review from at least 1 reviewer`
   - `Require status checks to pass`
   - `Include administrators` (opcional)
   - `Restrict who can push`

✅ Esto evita cambios accidentales o sin revisión.

---

## 3️⃣ Protección contra Vulnerabilidades

### ⚠ Dependabot

Dependabot actualiza automáticamente dependencias vulnerables.

1. Activa `Dependabot alerts` y `security updates` en `Settings → Security & analysis`.
2. Crea `.github/dependabot.yml`.

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
```

---

### 🔎 Análisis de código y escaneo de secretos

**Secret scanning** detecta claves y tokens expuestos.  
**Code scanning** (como CodeQL) encuentra vulnerabilidades en el código fuente.

Actívalos desde:  
`Settings → Security & analysis → Enable secret/code scanning`

Ejemplo básico con CodeQL:

```yaml
name: CodeQL
on: [push, pull_request]
jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: github/codeql-action/init@v2
        with:
          languages: javascript
      - uses: github/codeql-action/analyze@v2
```

---

## 4️⃣ Buenas Prácticas para la Seguridad del Código

### 🚫 Usa archivos `.gitignore`

Evita subir archivos sensibles. Ejemplo:

```
.env
config/secrets.yml
*.key
node_modules/
```

---

### 🔑 Usa variables de entorno en lugar de credenciales en código

Crea un archivo `.env`:

```
API_KEY=123456789
```

Ejemplo en Node.js:

```js
require('dotenv').config();
const apiKey = process.env.API_KEY;
```

---

### 📜 Firma tus commits con GPG

```bash
git config --global user.signingkey <GPG_KEY>
git commit -S -m "Commit firmado"
```

Agrega tu clave GPG en GitHub: `Settings → SSH and GPG Keys → New GPG Key`

---

## 5️⃣ Monitoreo y Auditoría

### 📊 Verifica el historial y accesos

1. En organizaciones: `Settings → Audit Log`.
2. Revisa la pestaña `Security` en cada repo para alertas activas.
3. Habilita notificaciones por email o GitHub Mobile.

---

### 🛠 Automatiza revisiones con GitHub Actions

Ejecuta análisis en cada push o PR.

Ejemplo de workflow:

```yaml
name: Security Check

on: [push, pull_request]

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm audit --audit-level=moderate
```

---

## 6️⃣ Recomendaciones adicionales de protección

✅ **Usa archivos `.gitignore`:** Excluye archivos sensibles o de configuración privada.  
✅ **Haz repositorios privados:** Para proyectos confidenciales, selecciona la visibilidad *private* al crear el repo o cambia en `Settings → Danger Zone`.  
✅ **Activa 2FA:** Protege el acceso a tu cuenta contra robos de contraseña.  
✅ **Configura alertas de seguridad:** Habilita `Code scanning`, `Secret scanning` y `Dependabot`.  
✅ **Revoca claves expuestas:** Si subiste accidentalmente un token o clave:
1. Elimínalo del repositorio.
2. Rótalo (cámbialo en el proveedor, API o sistema).
3. Revoca el acceso desde GitHub (Developer Settings → Tokens).

---

## 🎯 Conclusión

✅ Usa autenticación segura con 2FA y claves SSH  
✅ Controla permisos y protege ramas críticas  
✅ Mantén dependencias seguras con Dependabot  
✅ Evita exponer información sensible en los commits  
✅ Monitorea accesos y cambios en el repositorio  
✅ Automatiza escaneos y pruebas para prevenir vulnerabilidades  
✅ Revoca cualquier secreto expuesto inmediatamente

---

## 📚 Recursos Recomendados

- [GitHub Security Best Practices](https://docs.github.com/en/code-security/getting-started/github-security-best-practices)  
- [Securing your repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/enabling-security-features)  
- [Using GPG to sign commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)  
- [Secret Scanning](https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning)  
- [Audit Log](https://docs.github.com/en/enterprise-cloud@latest/admin/policies/auditing/reviewing-audit-logs)

# 🔒 Gestión de Dependencias y Seguridad con Dependabot en GitHub

**Dependabot** es una herramienta integrada en GitHub que ayuda a mantener actualizadas las dependencias de tu proyecto y a detectar vulnerabilidades de seguridad en paquetes desactualizados.

---

## 📚 Índice

- [1️⃣ ¿Qué es Dependabot y cómo funciona?](#1️⃣-qué-es-dependabot-y-cómo-funciona)
  - [🛠️ Funciones principales](#️-funciones-principales)
- [2️⃣ Activar Dependabot en un Repositorio](#2️⃣-activar-dependabot-en-un-repositorio)
- [3️⃣ Configurar Actualizaciones Automáticas de Dependencias](#3️⃣-configurar-actualizaciones-automáticas-de-dependencias)
  - [📄 Ejemplo para npm](#📄-ejemplo-para-npm)
  - [🔁 Ejemplo para pip](#🔁-ejemplo-para-pip)
  - [📦 Opciones disponibles](#📦-opciones-disponibles)
- [4️⃣ Seguridad: Alertas y Parches Automáticos](#4️⃣-seguridad-alertas-y-parches-automáticos)
  - [✅ Ejemplo de Pull Request generado](#✅-ejemplo-de-pull-request-generado)
- [🧪 Ejemplo real de workflow con Dependabot + CI](#🧪-ejemplo-real-de-workflow-con-dependabot--ci)
- [✅ Buenas Prácticas](#✅-buenas-prácticas)
- [🎯 Conclusión](#🎯-conclusión)
- [📚 Recursos oficiales](#📚-recursos-oficiales)

---

## 1️⃣ ¿Qué es Dependabot y cómo funciona?

Dependabot automatiza la actualización de dependencias en proyectos gestionados con:

- `npm` (JavaScript / TypeScript)
- `pip` (Python)
- `maven` / `gradle` (Java)
- `composer` (PHP)
- `cargo` (Rust)
- `nuget` (.NET)
- ... y más.

### 🛠️ Funciones principales

✔ Detecta vulnerabilidades conocidas en dependencias  
✔ Genera automáticamente Pull Requests con versiones actualizadas  
✔ Ofrece parches de seguridad basados en CVEs  
✔ Se puede configurar por ecosistema y frecuencia

---

## 2️⃣ Activar Dependabot en un Repositorio

1. Ve a tu repositorio en GitHub  
2. Haz clic en **Settings → Security & analysis**  
3. Asegúrate de activar:
   - ✅ **Dependabot alerts**
   - ✅ **Dependabot security updates**

---

## 3️⃣ Configurar Actualizaciones Automáticas de Dependencias

Para que Dependabot genere automáticamente Pull Requests de actualización, agrega el archivo:

📁 `.github/dependabot.yml`

### 📄 Ejemplo para npm

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/" # raíz del proyecto
    schedule:
      interval: "daily" # Opciones: daily, weekly, monthly
    open-pull-requests-limit: 5
    labels:
      - "dependencies"
    ignore:
      - dependency-name: "lodash"
        versions: ["*"] # Ignorar todas las versiones
```

### 🔁 Ejemplo para pip

```yaml
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    allow:
      - dependency-type: "direct"
```

### 📦 Opciones disponibles

- `package-ecosystem`: Gestor de dependencias (`npm`, `pip`, `maven`, etc.)
- `directory`: Ubicación del archivo (`/`, `/backend`, `/frontend`)
- `schedule`: Frecuencia (`daily`, `weekly`, `monthly`)
- `ignore`: Lista de paquetes a ignorar
- `labels`: Etiquetas automáticas para los PRs

---

## 4️⃣ Seguridad: Alertas y Parches Automáticos

Cuando GitHub detecta una vulnerabilidad en alguna dependencia:

1. 🔴 Se muestra una alerta en la pestaña `Security → Dependabot alerts`
2. 🟢 Dependabot genera automáticamente un Pull Request con la versión recomendada del paquete

### ✅ Ejemplo de Pull Request generado

```
Bumps lodash from 4.17.19 to 4.17.21

Security vulnerabilities fixed:
 - CVE-2021-23337
```

GitHub añade comentarios explicando el cambio y proporciona enlaces a los CVEs y changelogs.

---

## 🧪 Ejemplo real de workflow con Dependabot + CI

```yaml
name: CI on Dependabot PR

on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]

jobs:
  test:
    if: github.actor == 'dependabot[bot]'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Instalar dependencias
        run: npm install
      - name: Ejecutar pruebas
        run: npm test
```

Este flujo de trabajo asegura que **toda actualización propuesta por Dependabot pase las pruebas automáticas antes de ser mergeada.**

---

## ✅ Buenas Prácticas

- Combina Dependabot con GitHub Actions para probar automáticamente los PRs de actualización
- Usa `labels` y `assignees` para que el equipo de desarrollo valide cambios
- No ignores paquetes vulnerables sin justificación técnica
- Usa `"allow"` para limitar actualizaciones a dependencias directas si deseas más control
- Documenta en tu `README.md` que usas Dependabot como parte de tu estrategia de seguridad

---

## 🎯 Conclusión

- 🔐 **Mejora la seguridad** al detectar y corregir vulnerabilidades automáticamente  
- 🔄 **Mantiene las dependencias actualizadas** con mínimo esfuerzo  
- 🧠 **Facilita la gestión de proyectos** al automatizar actualizaciones en múltiples entornos  

---

## 📚 Recursos oficiales

- [GitHub Docs - Dependabot](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically)  
- [Dependabot YAML Config Reference](https://docs.github.com/en/code-security/supply-chain-security/configuration-options-for-dependency-updates)

# 🔐 Cómo Usar Tokens en GitHub para Acceso Seguro a Repositorios Privados

GitHub permite autenticarse de forma segura usando tokens en lugar de contraseñas. Estos tokens son necesarios especialmente cuando accedes a **repositorios privados** o usas la **GitHub API**.

GitHub admite dos tipos principales de tokens:

- **Tokens clásicos (PATs - Personal Access Tokens)**
- **Tokens de acceso refinado (Fine-grained personal access tokens)**

---

## 🧾 1. Crear un Token Personal Clásico (PAT)

1. Ir a https://github.com/settings/tokens  
2. Selecciona **“Tokens (classic)” > Generate new token**  
3. Elige el **scope (alcance)**, por ejemplo:  
   - `repo` para acceso a repos privados  
   - `workflow` para GitHub Actions  
   - `read:org` si necesitas datos de la organización  
4. Define una expiración (recomendado por seguridad)  
5. Haz clic en **Generate token** y cópialo inmediatamente.

---

### ✅ Ejemplo: Clonar un repositorio privado

```bash
git clone https://<TOKEN>@github.com/tu-usuario/nombre-repo.git
```

⚠️ No recomendado para producción, mejor usar `.netrc`, `git credential manager` o variables de entorno.

---

### ✅ Usar con GitHub CLI

```bash
gh auth login
```

Selecciona “HTTPS” y luego ingresa tu token cuando te lo solicite.

---

### ✅ Usar en scripts o CI/CD

```bash
export GITHUB_TOKEN=<your_token>
git clone https://github.com/tu-org/mi-repo-privado.git
```

---

## 🔒 2. Crear un Token de Acceso Refinado (Fine-grained token)

1. Ir a https://github.com/settings/personal-access-tokens  
2. Selecciona **Generate new token (fine-grained)**  
3. Define:
   - **Repositorios específicos** donde el token tendrá acceso  
   - **Permisos detallados**: lectura/escritura a código, acciones, secretos, etc.  
   - **Expiración** segura  
4. Copia el token una vez generado

---

### ✅ Ventajas

- Acceso mucho más limitado  
- Más seguro para automatización o CI/CD  
- Ideal para trabajar en organizaciones  

---

### ✅ Usar en Git Remote (guardado seguro)

```bash
git remote set-url origin https://<TOKEN>@github.com/tu-org/mi-repo.git
```

---

### ✅ Alternativa segura: Git Credential Helper

```bash
git config --global credential.helper store
# luego ejecutas cualquier comando (como git pull) y cuando te pida usuario/token, se guardará
```

---

## 🧪 Ejemplo: Clonar un repositorio privado con usuario + token

Supongamos que tu usuario de GitHub es `misadev` y quieres clonar el repo privado `repo-privado`.

---

### 🔧 1. Abre tu terminal

```bash
git clone https://github.com/misadev/repo-privado.git
```

---

### 🔐 2. Git te pedirá credenciales:

```bash
Username for 'https://github.com': misadev
Password for 'https://misadev@github.com': <aquí pegas tu token>
```

✅ Aquí **no se pone la contraseña real**, sino el **Personal Access Token (PAT)** o el **Fine-grained Token**.

---

## 📌 Recomendaciones

- Si no quieres que te lo pida siempre, puedes guardar el token con:

```bash
git config --global credential.helper cache  # temporal
# o
git config --global credential.helper store  # persistente (¡cuidado en entornos compartidos!)
```

- Para asegurarte que no estás usando SSH por error, valida tu remote con:

```bash
git remote -v
```

Debe decir algo como:

```bash
origin  https://github.com/misadev/repo-privado.git (fetch)
origin  https://github.com/misadev/repo-privado.git (push)
```

---

## 🛠️ Buenas Prácticas de Seguridad

- Nunca subas un token al código fuente  
- Usa variables de entorno en GitHub Actions o en tu sistema local  
- Prefiere tokens **fine-grained** para tareas específicas  
- Revoca tokens no utilizados  
- Usa `gh auth status` para validar sesión activa con GitHub CLI

---

## 📦 Ejemplo en GitHub Actions (con Fine-grained Token)

```yaml
name: CI Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Clonar repositorio privado
        run: |
          git clone https://github.com/tu-org/privado.git
        env:
          GIT_ASKPASS: echo
          GITHUB_TOKEN: ${{ secrets.MI_FINE_TOKEN }}
```

⚠️ Guarda tu token como secreto en GitHub Actions (Settings > Secrets and variables > Actions)
