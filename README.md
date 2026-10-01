# Repositorio-prueba

Repositorio de práctica para aprender a usar **Git** y **GitHub**. Esta guía cubre desde los conceptos básicos hasta el flujo de trabajo con ramas y Pull Requests.

---

## Índice

1. [Conceptos básicos](#1-conceptos-básicos)
2. [Instalación y configuración inicial](#2-instalación-y-configuración-inicial)
3. [Obtener un repositorio](#3-obtener-un-repositorio)
4. [El ciclo básico: modificar, preparar, confirmar, subir](#4-el-ciclo-básico-modificar-preparar-confirmar-subir)
5. [Ver el estado y el historial](#5-ver-el-estado-y-el-historial)
6. [Traer cambios del remoto](#6-traer-cambios-del-remoto)
7. [Ramas (branches)](#7-ramas-branches)
8. [Pull Requests en GitHub](#8-pull-requests-en-github)
9. [Resolver conflictos](#9-resolver-conflictos)
10. [Deshacer cosas](#10-deshacer-cosas)
11. [El archivo .gitignore](#11-el-archivo-gitignore)
12. [Otras funciones de GitHub](#12-otras-funciones-de-github)
13. [Buenas prácticas](#13-buenas-prácticas)
14. [Resumen de comandos](#14-resumen-de-comandos)
15. [Recursos para seguir aprendiendo](#15-recursos-para-seguir-aprendiendo)

---

## 1. Conceptos básicos

| Concepto | Qué es |
|---|---|
| **Git** | Programa que se instala en tu computadora y guarda el historial de cambios de tus archivos. Funciona sin internet. |
| **GitHub** | Sitio web que aloja repositorios de Git en la nube para compartirlos y colaborar. |
| **Repositorio (repo)** | Carpeta de un proyecto cuyo historial controla Git. El historial vive en la subcarpeta oculta `.git`. |
| **Commit** | Una "foto" del proyecto en un momento dado, con un mensaje que describe qué cambió. |
| **Rama (branch)** | Una línea de trabajo independiente. La principal suele llamarse `main`. |
| **Remoto (remote)** | La copia del repositorio en GitHub. Por convención se llama `origin`. |
| **Clonar** | Descargar un repositorio de GitHub a tu computadora. |
| **Push** | Subir tus commits locales a GitHub. |
| **Pull** | Bajar los commits nuevos de GitHub a tu computadora. |
| **Pull Request (PR)** | Propuesta en GitHub para integrar los cambios de una rama en otra, con revisión de por medio. |

### Las tres zonas de Git

```
 Directorio de trabajo  --git add-->  Área de preparación  --git commit-->  Repositorio local  --git push-->  GitHub
 (tus archivos)                        (staging)                             (.git)                            (origin)
```

1. **Directorio de trabajo**: los archivos tal como los ves y editás.
2. **Staging**: los cambios que elegiste incluir en el próximo commit.
3. **Repositorio local**: los commits ya guardados en tu máquina.

Después, `git push` los sube a GitHub.

---

## 2. Instalación y configuración inicial

### Instalar Git (Windows)

```powershell
winget install --id Git.Git -e --source winget
```

Después de instalar, **cerrá y volvé a abrir la terminal** para que reconozca el comando `git`. Verificá con:

```bash
git --version
```

### Configurar tu identidad (una sola vez)

Cada commit lleva tu nombre y email. Usá el mismo email que tu cuenta de GitHub:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-email@ejemplo.com"
git config --global init.defaultBranch main
```

Para ver tu configuración: `git config --list`

### Autenticación con GitHub

Git para Windows trae **Git Credential Manager**. La primera vez que hagas `git push`, se abre una ventana del navegador para iniciar sesión en GitHub. Las credenciales quedan guardadas y no te las vuelve a pedir.

> GitHub ya no acepta la contraseña de la cuenta en la terminal. Si alguna vez te la pide por texto, usá un **Personal Access Token** (GitHub → Settings → Developer settings → Personal access tokens).

---

## 3. Obtener un repositorio

### Opción A: clonar uno que ya existe en GitHub

```bash
git clone https://github.com/sernst-molinos/Repositorio-prueba.git
cd Repositorio-prueba
```

### Opción B: crear uno nuevo desde una carpeta local

```bash
cd mi-proyecto
git init
git add .
git commit -m "Primer commit"
```

Después creás un repo vacío en GitHub (botón **New** en github.com, sin README) y lo conectás:

```bash
git remote add origin https://github.com/TU-USUARIO/mi-proyecto.git
git push -u origin main
```

El `-u` deja asociada tu rama local con la remota. A partir de ahí alcanza con `git push`.

---

## 4. El ciclo básico: modificar, preparar, confirmar, subir

Este es el flujo que vas a repetir todo el tiempo:

```bash
# 1. Editá tus archivos normalmente

# 2. Mirá qué cambió
git status

# 3. Prepará los cambios (staging)
git add archivo.txt        # un archivo puntual
git add .                  # todos los cambios de la carpeta actual

# 4. Guardá un commit con un mensaje descriptivo
git commit -m "Agrega sección de instalación al README"

# 5. Subilo a GitHub
git push
```

### Cómo escribir buenos mensajes de commit

- Corto (idealmente menos de 50 caracteres) y en modo imperativo: *"Agrega..."*, *"Corrige..."*, *"Elimina..."*.
- Que explique **qué** cambió y, si no es obvio, **por qué**.
- Un commit = un cambio lógico. Evitá commits gigantes que mezclan cosas distintas.

| ❌ Malo | ✅ Bueno |
|---|---|
| `cambios` | `Corrige cálculo de totales en reporte mensual` |
| `asdf` | `Agrega validación de email en formulario` |

---

## 5. Ver el estado y el historial

```bash
git status                 # qué archivos cambiaron y cuáles están en staging
git diff                   # cambios que todavía NO están en staging
git diff --staged          # cambios que SÍ están en staging
git log                    # historial completo de commits
git log --oneline --graph  # historial compacto, con dibujo de ramas
git show <id-commit>       # detalle de un commit específico
```

En `git log`, apretá `q` para salir.

---

## 6. Traer cambios del remoto

Si otra persona (o vos desde otra computadora) subió cambios a GitHub:

```bash
git pull
```

`git pull` es en realidad dos pasos:

```bash
git fetch     # descarga los cambios sin aplicarlos
git merge     # los integra a tu rama actual
```

**Hábito recomendado:** hacé `git pull` antes de empezar a trabajar y antes de hacer `git push`.

---

## 7. Ramas (branches)

Las ramas te permiten trabajar en algo nuevo sin tocar `main`. Si sale mal, borrás la rama y listo.

```bash
git branch                     # lista las ramas locales (la actual tiene *)
git branch -a                  # incluye las ramas remotas
git switch -c nueva-funcion    # crea una rama nueva y se cambia a ella
git switch main                # vuelve a main
git branch -d nueva-funcion    # borra una rama ya integrada
```

> En tutoriales viejos vas a ver `git checkout -b` en lugar de `git switch -c`. Hacen lo mismo.

### Flujo típico con ramas

```bash
git switch main
git pull                               # partí de main actualizado
git switch -c corrige-titulo           # rama para tu cambio
# ... editás archivos ...
git add .
git commit -m "Corrige el título del README"
git push -u origin corrige-titulo      # sube la rama a GitHub
```

Después abrís un **Pull Request** en GitHub (ver la siguiente sección).

### Integrar una rama localmente (sin PR)

```bash
git switch main
git merge corrige-titulo
git push
```

---

## 8. Pull Requests en GitHub

Un Pull Request (PR) es la forma estándar de proponer cambios y que alguien los revise antes de integrarlos a `main`.

1. Subí tu rama: `git push -u origin mi-rama`.
2. Entrá al repo en github.com. Va a aparecer un aviso amarillo **"Compare & pull request"**. Hacé clic.
3. Escribí un título y una descripción de qué cambiaste y por qué.
4. Asigná revisores (*Reviewers*) si corresponde y hacé clic en **Create pull request**.
5. Los revisores pueden comentar línea por línea, aprobar o pedir cambios.
6. Si te piden cambios, hacé nuevos commits en la misma rama y `git push`. El PR se actualiza solo.
7. Cuando está aprobado, hacé clic en **Merge pull request**.
8. Borrá la rama (GitHub ofrece el botón) y actualizá tu copia local:

```bash
git switch main
git pull
git branch -d mi-rama
```

---

## 9. Resolver conflictos

Un conflicto ocurre cuando dos personas modificaron **las mismas líneas** de un archivo. Git no sabe cuál versión elegir y te pregunta a vos.

Al hacer `git pull` o `git merge` vas a ver algo como:

```
CONFLICT (content): Merge conflict in README.md
```

Dentro del archivo, Git marca la zona en conflicto:

```
<<<<<<< HEAD
Tu versión del texto
=======
La versión que viene de la otra rama
>>>>>>> otra-rama
```

**Para resolverlo:**

1. Abrí el archivo (VS Code muestra botones como *Accept Current*, *Accept Incoming* y *Accept Both*).
2. Dejá el contenido final que querés y borrá las marcas `<<<<<<<`, `=======` y `>>>>>>>`.
3. Guardá y ejecutá:

```bash
git add README.md
git commit
```

Si te arrepentís en medio del proceso: `git merge --abort`.

---

## 10. Deshacer cosas

| Situación | Comando |
|---|---|
| Descartar cambios de un archivo que **no** está en staging | `git restore archivo.txt` |
| Sacar un archivo del staging (sin perder los cambios) | `git restore --staged archivo.txt` |
| Corregir el mensaje del **último** commit (si todavía no hiciste push) | `git commit --amend -m "Nuevo mensaje"` |
| Deshacer el último commit pero conservar los cambios | `git reset --soft HEAD~1` |
| Revertir un commit que **ya** está en GitHub (crea un commit inverso) | `git revert <id-commit>` |
| Guardar cambios temporalmente para cambiar de rama | `git stash` y después `git stash pop` |

> ⚠️ **Cuidado:** `git reset --hard` y `git push --force` **borran trabajo** de forma difícil de recuperar. Evitalos en ramas compartidas. Si un commit ya está en GitHub, usá `git revert`.

---

## 11. El archivo .gitignore

`.gitignore` es un archivo de texto en la raíz del repo con la lista de archivos que Git **no** debe versionar: temporales, contraseñas, dependencias descargadas, etc.

Ejemplo:

```gitignore
# Checkpoints de Jupyter
.ipynb_checkpoints/

# Entornos virtuales de Python
venv/
.venv/
__pycache__/

# Archivos con credenciales
.env

# Archivos del sistema
Thumbs.db
.DS_Store
```

Si un archivo ya estaba versionado antes de agregarlo al `.gitignore`, tenés que sacarlo del índice:

```bash
git rm -r --cached .ipynb_checkpoints
git commit -m "Deja de versionar .ipynb_checkpoints"
```

> 🔒 **Nunca subas contraseñas, tokens ni claves de API a GitHub**, aunque el repo sea privado. Si pasa, cambiá la credencial de inmediato: borrarla en un commit nuevo no alcanza, porque queda en el historial.

---

## 12. Otras funciones de GitHub

- **Issues**: lista de tareas, bugs o ideas del proyecto. Se pueden asignar, etiquetar y cerrar automáticamente desde un commit o PR escribiendo `Closes #12` en el mensaje.
- **Fork**: copia de un repo ajeno en tu cuenta. Sirve para proponer cambios a proyectos donde no tenés permiso de escritura (hacés un fork, cambiás tu copia y abrís un PR al original).
- **README.md**: este archivo. GitHub lo muestra en la página principal del repo. Se escribe en [Markdown](https://docs.github.com/es/get-started/writing-on-github).
- **Releases y tags**: marcan versiones del proyecto (`v1.0.0`).
- **GitHub Actions**: automatizaciones (tests, despliegues) que se ejecutan con cada push o PR.
- **Settings → Collaborators**: para invitar a otras personas a tu repositorio.
- **Branch protection** (Settings → Branches): obliga a usar PRs y revisiones para modificar `main`.

### Markdown en 30 segundos

```markdown
# Título grande
## Subtítulo
**negrita**, *cursiva*, `código`
- lista
1. lista numerada
[texto del link](https://github.com)
```

---

## 13. Buenas prácticas

1. **Commits chicos y frecuentes**, con mensajes claros.
2. **`git pull` antes de empezar** a trabajar.
3. **No trabajes directo en `main`**: usá una rama por tarea y un PR para integrarla.
4. **Revisá `git status` y `git diff`** antes de cada commit.
5. **Usá `.gitignore`** desde el principio.
6. **Nunca subas secretos** (contraseñas, tokens, `.env`).
7. **Evitá guardar repos en carpetas sincronizadas** (OneDrive, Dropbox): la sincronización puede corromper archivos dentro de `.git`. Si aparecen errores raros, mové el repo a una carpeta local como `C:\repos\`.

---

## 14. Resumen de comandos

| Comando | Para qué sirve |
|---|---|
| `git clone <url>` | Descargar un repo |
| `git init` | Convertir una carpeta en repo |
| `git status` | Ver el estado actual |
| `git add <archivo>` / `git add .` | Preparar cambios |
| `git commit -m "mensaje"` | Guardar un commit |
| `git push` | Subir commits a GitHub |
| `git pull` | Bajar cambios de GitHub |
| `git log --oneline` | Ver el historial |
| `git diff` | Ver diferencias |
| `git switch -c <rama>` | Crear una rama y cambiarse a ella |
| `git switch <rama>` | Cambiar de rama |
| `git merge <rama>` | Integrar una rama en la actual |
| `git restore <archivo>` | Descartar cambios locales |
| `git stash` / `git stash pop` | Guardar y recuperar cambios temporales |
| `git remote -v` | Ver a qué repo remoto está conectado |

---

## 15. Recursos para seguir aprendiendo

- [Documentación oficial de GitHub (en español)](https://docs.github.com/es)
- [Libro Pro Git (gratis, en español)](https://git-scm.com/book/es/v2)
- [Learn Git Branching](https://learngitbranching.js.org/?locale=es_AR): ejercicios interactivos para entender las ramas
- [GitHub Skills](https://skills.github.com/): cursos prácticos dentro de GitHub
- [Cheat sheet de Git (PDF)](https://education.github.com/git-cheat-sheet-education.pdf)
