# Master Guide: Git y GitHub 🐙

## 📑 Índice
1. [Introducción a Git y GitHub](#1-introducción-a-git-y-github)
2. [Conceptos Básicos](#2-conceptos-básicos)
3. [Los 3 Estados de un Archivo](#3-los-3-estados-de-un-archivo)
4. [Comandos Esenciales](#4-comandos-esenciales)
5. [Ramas (Branches)](#5-ramas-branches)
6. [Conflictos y Resolución](#6-conflictos-y-resolución)
7. [El .gitignore](#7-el-gitignore)
8. [GitHub: Colaboración](#8-github-colaboración)
9. [Workflows Comunes](#9-workflows-comunes)
10. [Buenas Prácticas](#10-buenas-prácticas)

---

## 1. Introducción a Git y GitHub

**Git** es el sistema de control de versiones que te permite "viajar en el tiempo" en tu código. **GitHub** es la red social donde guardamos esos proyectos.

### ¿Qué es Git?

**Git** es un sistema de control de versiones distribuido que permite:
- ✅ **Historial completo**: Ver todos los cambios realizados
- ✅ **Revertir cambios**: Volver a versiones anteriores
- ✅ **Trabajo en equipo**: Múltiples personas trabajando en el mismo proyecto
- ✅ **Ramas**: Trabajar en features sin afectar el código principal
- ✅ **Backup**: Tu código está guardado en múltiples lugares

### ¿Qué es GitHub?

**GitHub** es una plataforma de hosting para repositorios Git que ofrece:
- ✅ **Almacenamiento en la nube**: Tus repositorios en la nube
- ✅ **Colaboración**: Pull requests, issues, discusiones
- ✅ **CI/CD**: Integración continua y despliegue
- ✅ **Documentación**: README, wikis, páginas
- ✅ **Red social**: Seguir proyectos, estrellas, forks

### Analogía Simple

Imagina Git como un **sistema de guardado de videojuegos**:
- Cada **commit** es un punto de guardado
- Puedes volver a cualquier punto de guardado
- Puedes tener múltiples "partidas" (ramas) diferentes
- GitHub es como la "nube" donde guardas tus partidas para compartirlas

---

## 2. Conceptos Básicos

### Repositorio Local

Tu carpeta privada con Git iniciado. Contiene todo el historial de cambios.

```bash
# Inicializar repositorio
git init
```

### Repositorio Remoto (`origin`)

Tu código en la nube (GitHub). Es una copia del repositorio local que puedes compartir.

```bash
# Conectar con repositorio remoto
git remote add origin https://github.com/usuario/proyecto.git
```

### Commit

Un **commit** es un "punto de guardado" en el historial. Contiene:
- Los cambios realizados
- Un mensaje descriptivo
- Autor y fecha
- Hash único (ID)

### Rama (Branch)

Un **branch** es un desvío del código principal para trabajar sin romper nada. Permite:
- Trabajar en features independientes
- Experimentar sin afectar el código principal
- Colaborar sin conflictos

### HEAD

**HEAD** es un puntero que indica en qué commit estás actualmente. Es como tu "posición actual" en el historial.

---

## 3. Los 3 Estados de un Archivo

Git tiene 3 estados principales para cada archivo:

### 1. Modified (Modificado)

Cambios realizados pero **no guardados** en Git. Los archivos están modificados pero Git aún no los ha registrado.

**Ejemplo**:
```bash
# Editas un archivo
# El archivo está "modified" pero no "staged"
```

### 2. Staged (Preparado)

Cambios preparados para el "paquete" (Commit). Los archivos están listos para ser incluidos en el próximo commit.

**Ejemplo**:
```bash
git add archivo.js
# El archivo ahora está "staged"
```

### 3. Committed (Confirmado)

Cambios guardados de forma segura en el historial. Los archivos están guardados en un commit.

**Ejemplo**:
```bash
git commit -m "Mensaje"
# Los cambios están "committed"
```

### Flujo de Estados

```
Working Directory (Modified)
        ↓
    git add
        ↓
Staging Area (Staged)
        ↓
    git commit
        ↓
Repository (Committed)
```

### Ver Estado

```bash
# Ver estado de todos los archivos
git status

# Ver cambios detallados
git diff              # Cambios no staged
git diff --staged     # Cambios staged
```

---

## 4. Comandos Esenciales

### Iniciar Proyecto

```bash
# Inicializar repositorio (solo la primera vez)
git init

# Conectar con GitHub
git remote add origin https://github.com/usuario/proyecto.git

# Ver remotos configurados
git remote -v
```

### Guardar y Subir

```bash
# Ver qué archivos han cambiado
git status

# Preparar todos los archivos
git add .

# Preparar archivos específicos
git add archivo1.js archivo2.js

# Crear punto de guardado (commit)
git commit -m "Mensaje claro y descriptivo"

# Subir a la nube (primera vez)
git push -u origin main

# Subir cambios posteriores
git push origin main
```

### Actualizar desde Remoto

```bash
# Bajar cambios del equipo
git pull origin main

# Solo bajar (sin fusionar)
git fetch origin

# Ver diferencias antes de hacer pull
git fetch origin
git diff main origin/main
```

### Ver Historial

```bash
# Ver historial de commits
git log

# Ver historial simplificado
git log --oneline

# Ver historial con gráfico
git log --oneline --graph --all
```

---

## 5. Ramas (Branches)

Un **branch** es un desvío del código principal para trabajar sin romper nada.

### Conceptos

- **main/master**: La rama principal del proyecto
- **feature branch**: Rama para desarrollar una funcionalidad
- **merge**: Fusionar cambios de una rama a otra

### Comandos de Ramas

#### Crear y Cambiar de Rama

```bash
# Crear nueva rama
git branch nueva-rama

# Crear y saltar a nueva rama
git checkout -b nueva-rama

# Cambiar de rama
git checkout main

# Ver todas las ramas
git branch

# Ver ramas remotas
git branch -r
```

#### Fusionar Ramas

```bash
# Cambiar a la rama destino (ej: main)
git checkout main

# Traer cambios de otra rama
git merge rama-a-unir

# Eliminar rama después de merge
git branch -d rama-a-unir
```

### Workflow con Ramas

```bash
# 1. Crear rama para feature
git checkout -b feature/login

# 2. Trabajar en la feature
# ... hacer cambios ...
git add .
git commit -m "Agregar funcionalidad de login"

# 3. Volver a main
git checkout main

# 4. Fusionar cambios
git merge feature/login

# 5. Subir a GitHub
git push origin main

# 6. Eliminar rama local
git branch -d feature/login
```

### Ramas Remotas

```bash
# Subir rama a GitHub
git push -u origin nombre-rama

# Bajar rama remota
git checkout -b nombre-rama origin/nombre-rama

# Ver ramas remotas
git branch -r
```

---

## 6. Conflictos y Resolución

Ocurren cuando dos personas editan la misma línea del mismo archivo.

### ¿Cuándo Ocurren Conflictos?

- Dos personas modifican la misma línea
- Haces `git pull` y hay cambios conflictivos
- Haces `git merge` y hay cambios conflictivos

### Proceso de Resolución

#### 1. Git Detiene el Merge

Git te dirá "Automatic merge failed" y te mostrará qué archivos tienen conflictos.

```bash
# Ver archivos con conflictos
git status
```

#### 2. Abrir el Archivo

Verás marcas como `<<<<<<<`, `=======`, y `>>>>>>>`:

```javascript
<<<<<<< HEAD
const nombre = "Juan";
=======
const nombre = "María";
>>>>>>> branch-feature
```

#### 3. Limpiar el Código

Elige qué versión queda (o mezcla ambas):

**Opción 1: Mantener tu versión (HEAD)**
```javascript
const nombre = "Juan";
```

**Opción 2: Mantener la otra versión**
```javascript
const nombre = "María";
```

**Opción 3: Combinar ambas**
```javascript
const nombres = ["Juan", "María"];
```

#### 4. Finalizar

```bash
# Agregar archivos resueltos
git add archivo-resuelto.js

# Completar el merge
git commit -m "Resolver conflictos en archivo.js"
```

### Herramientas de Resolución

- **Editor de texto**: Resolver manualmente
- **GitHub Desktop**: Interfaz visual
- **VS Code**: Editor integrado con resolución de conflictos
- **Meld/KDiff3**: Herramientas especializadas

### Prevenir Conflictos

- ✅ **Comunicación**: Coordina con tu equipo
- ✅ **Pull frecuente**: `git pull` antes de trabajar
- ✅ **Ramas pequeñas**: Trabaja en features pequeñas
- ✅ **Commits frecuentes**: Commits pequeños y frecuentes

---

## 7. El .gitignore

Archivo vital donde pones lo que Git **debe ignorar**. Los archivos listados aquí no serán rastreados por Git.

### ¿Por qué es Importante?

- ✅ **Seguridad**: No subir contraseñas o API keys
- ✅ **Rendimiento**: No subir archivos pesados
- ✅ **Limpieza**: No subir archivos generados automáticamente

### Crear .gitignore

Crea un archivo llamado `.gitignore` en la raíz del proyecto:

```bash
# Crear archivo
touch .gitignore
```

### Contenido Común

```gitignore
# Dependencias
node_modules/
package-lock.json
yarn.lock

# Variables de entorno
.env
.env.local
.env.production

# Archivos generados
dist/
build/
*.log

# Sistema operativo
.DS_Store
Thumbs.db

# IDEs
.vscode/
.idea/
*.swp

# Testing
coverage/
.nyc_output/

# Archivos temporales
*.tmp
*.temp
```

### Patrones

```gitignore
# Ignorar archivo específico
archivo.js

# Ignorar todos los archivos con extensión
*.log

# Ignorar carpeta completa
node_modules/

# Ignorar pero con excepción
*.js
!archivo-importante.js

# Ignorar en subcarpetas
**/temp/

# Ignorar archivos que empiezan con punto
.*
!.gitignore
```

### Verificar qué se Ignora

```bash
# Ver qué archivos serían ignorados
git status --ignored
```

---

## 8. GitHub: Colaboración

### Crear Repositorio en GitHub

1. Ve a [github.com](https://github.com)
2. Click en **New Repository**
3. Nombre: `mi-proyecto`
4. Descripción (opcional)
5. Público o Privado
6. **NO** inicializar con README (si ya tienes código local)
7. Click en **Create repository**

### Conectar Repositorio Local

```bash
# Si ya tienes código local
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/usuario/mi-proyecto.git
git push -u origin main
```

### Clonar Repositorio

```bash
# Clonar repositorio existente
git clone https://github.com/usuario/mi-proyecto.git

# Clonar en carpeta específica
git clone https://github.com/usuario/mi-proyecto.git mi-carpeta
```

### Pull Requests (PR)

**Pull Request** es una forma de proponer cambios y revisarlos antes de fusionarlos.

**Proceso**:
1. Crear rama: `git checkout -b feature/nueva-funcionalidad`
2. Hacer cambios y commits
3. Subir rama: `git push origin feature/nueva-funcionalidad`
4. En GitHub, crear Pull Request
5. Revisar cambios
6. Aprobar y fusionar

### Issues

**Issues** son como "tickets" para:
- Reportar bugs
- Proponer features
- Hacer preguntas
- Documentar tareas

### Fork y Contribuir

1. **Fork**: Hacer una copia del repositorio a tu cuenta
2. **Clone**: Clonar tu fork
3. **Cambios**: Hacer cambios
4. **Push**: Subir a tu fork
5. **Pull Request**: Proponer cambios al repositorio original

---

## 9. Workflows Comunes

### Workflow Básico Diario

```bash
# 1. Ver qué cambió
git status

# 2. Preparar cambios
git add .

# 3. Guardar
git commit -m "Descripción clara"

# 4. Actualizar desde remoto
git pull origin main

# 5. Subir cambios
git push origin main
```

### Workflow con Feature Branch

```bash
# 1. Actualizar main
git checkout main
git pull origin main

# 2. Crear rama para feature
git checkout -b feature/login

# 3. Trabajar en la feature
# ... hacer cambios ...
git add .
git commit -m "Agregar login"

# 4. Subir rama
git push -u origin feature/login

# 5. Crear Pull Request en GitHub

# 6. Después de aprobar, fusionar en GitHub

# 7. Actualizar main local
git checkout main
git pull origin main

# 8. Eliminar rama local
git branch -d feature/login
```

### Workflow de Hotfix (Corrección Urgente)

```bash
# 1. Crear rama desde main
git checkout main
git pull origin main
git checkout -b hotfix/bug-critico

# 2. Arreglar bug
# ... hacer cambios ...
git add .
git commit -m "Fix: Corregir bug crítico"

# 3. Subir y fusionar rápidamente
git push origin hotfix/bug-critico
# Fusionar en GitHub

# 4. Actualizar main
git checkout main
git pull origin main
```

---

## 10. Buenas Prácticas

### Mensajes de Commit

- ✅ **Claros y descriptivos**: "Agregar funcionalidad de login" en lugar de "cambios"
- ✅ **Imperativo**: "Agregar" no "Agregué" o "Agregando"
- ✅ **Específicos**: "Corregir bug en validación de email" en lugar de "fix"
- ✅ **Convenciones**: Usa prefijos como `feat:`, `fix:`, `docs:`, `refactor:`

**Ejemplos**:
```
✅ feat: Agregar autenticación con JWT
✅ fix: Corregir validación de email
✅ docs: Actualizar README con instrucciones
✅ refactor: Reorganizar estructura de carpetas
✅ test: Agregar tests para login
```

### Frecuencia de Commits

- ✅ **Commits pequeños**: Un commit por cambio lógico
- ✅ **Commits frecuentes**: No esperes días para hacer commit
- ✅ **Un cambio por commit**: No mezcles múltiples features en un commit

### Estructura de Commits

```
feat: Agregar funcionalidad de login

- Agregar endpoint POST /api/auth/login
- Agregar validación de credenciales
- Agregar generación de JWT token
- Agregar tests para login
```

### .gitignore Completo

Asegúrate de incluir:
- ✅ `node_modules/`
- ✅ `.env`
- ✅ `dist/` o `build/`
- ✅ Archivos de sistema (`.DS_Store`)
- ✅ Archivos de IDE (`.vscode/`)

### Ramas

- ✅ **Nombres descriptivos**: `feature/login`, `bugfix/email-validation`
- ✅ **Ramas pequeñas**: Una feature por rama
- ✅ **Eliminar después de merge**: Limpia ramas fusionadas
- ✅ **Sincronizar**: Mantén tus ramas actualizadas con `main`

### Colaboración

- ✅ **Pull antes de push**: Siempre `git pull` antes de `git push`
- ✅ **Comunicación**: Coordina con tu equipo
- ✅ **Pull Requests**: Usa PRs para revisar código
- ✅ **Code Review**: Revisa el código de otros

### Seguridad

- ✅ **Nunca subir `.env`**: Usa `.gitignore`
- ✅ **Nunca subir contraseñas**: Usa variables de entorno
- ✅ **Nunca subir `node_modules`**: Ya está en `.gitignore` por defecto
- ✅ **Tokens y API Keys**: Nunca en el código

---

## 📚 Comandos de Referencia Rápida

### Básicos
```bash
git init                    # Inicializar repositorio
git status                  # Ver estado
git add .                   # Preparar todos
git commit -m "mensaje"     # Guardar
git push origin main        # Subir
git pull origin main        # Bajar
```

### Ramas
```bash
git branch                  # Ver ramas
git checkout -b nueva      # Crear y cambiar
git checkout main          # Cambiar rama
git merge rama             # Fusionar
git branch -d rama         # Eliminar
```

### Historial
```bash
git log                     # Ver historial
git log --oneline          # Historial corto
git log --graph            # Con gráfico
```

### Remoto
```bash
git remote -v              # Ver remotos
git remote add origin URL  # Agregar remoto
git clone URL              # Clonar
```

### Deshacer
```bash
git restore archivo         # Descartar cambios
git restore --staged archivo # Quitar de staged
git reset HEAD~1           # Deshacer último commit
```

---

