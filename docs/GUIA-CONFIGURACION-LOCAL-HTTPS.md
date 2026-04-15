# Guía de Configuración LOCAL por Proyecto con Autenticación GitHub (HTTPS)

## Dirigida a estudiantes de la materia POO – 4to Software

> **¿Cuándo usar esta guía?**
> Cuando trabajas en una **computadora compartida** (laboratorio, biblioteca, etc.) donde **varios estudiantes** usan la misma máquina. Cada estudiante configura su identidad **solo en su carpeta de proyecto**, sin afectar al resto.

---

## Tabla de Contenidos

1. [Requisitos previos](#1-requisitos-previos)
2. [Paso 1: Verificar si Git está instalado](#2-paso-1-verificar-si-git-está-instalado)
3. [Paso 2: Instalar Git (si no lo tienes)](#3-paso-2-instalar-git-si-no-lo-tienes)
4. [Paso 3: Verificar y limpiar configuración global (importante en labs)](#4-paso-3-verificar-y-limpiar-configuración-global-importante-en-labs)
5. [Paso 4: Crear tu repositorio en GitHub](#5-paso-4-crear-tu-repositorio-en-github)
6. [Paso 5: Crear un Token de Acceso Personal (PAT)](#6-paso-5-crear-un-token-de-acceso-personal-pat)
7. [Paso 6: Inicializar tu repositorio local](#7-paso-6-inicializar-tu-repositorio-local)
8. [Paso 7: Configurar tu identidad LOCAL](#8-paso-7-configurar-tu-identidad-local)
9. [Paso 8: Desactivar el almacenamiento de credenciales](#9-paso-8-desactivar-el-almacenamiento-de-credenciales)
10. [Paso 9: Conectar con GitHub y subir tu código](#10-paso-9-conectar-con-github-y-subir-tu-código)
11. [Paso 10: Flujo de trabajo diario en el laboratorio](#11-paso-10-flujo-de-trabajo-diario-en-el-laboratorio)
12. [Protocolo de seguridad: Al terminar tu sesión](#12-protocolo-de-seguridad-al-terminar-tu-sesión)
13. [Solución de problemas frecuentes](#13-solución-de-problemas-frecuentes)
14. [Resumen rápido (cheat sheet)](#14-resumen-rápido-cheat-sheet)
15. [Glosario de términos](#15-glosario-de-términos)

---

## 1. Requisitos previos

- Computadora con Windows 10/11 (esta guía está orientada a Windows).
- Una cuenta de GitHub activa. Si no tienes una, créala en [github.com/signup](https://github.com/signup).
- Conexión a internet.
- Acceso a una terminal (PowerShell, CMD o Git Bash).

---

## 2. Paso 1: Verificar si Git está instalado

Abre una terminal y ejecuta:

```bash
git --version
```

### ¿Qué esperar?

| Resultado | Significado | Acción |
|-----------|------------|--------|
| `git version 2.x.x` | Git ya está instalado | Continúa al **Paso 3** |
| `'git' is not recognized...` | Git no está instalado | Ve al **Paso 2** |

---

## 3. Paso 2: Instalar Git (si no lo tienes)

> **Nota:** En los laboratorios de la universidad, Git normalmente ya está instalado. Si no lo está, solicita al administrador del laboratorio que lo instale.

1. Ve a [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. Descarga el instalador para Windows.
3. Ejecuta el instalador con las opciones por defecto.
4. **Cierra y vuelve a abrir la terminal**.
5. Verifica:

```bash
git --version
```

---

## 4. Paso 3: Verificar y limpiar configuración global (importante en labs)

> **⚠️ Este paso es CRÍTICO en computadoras compartidas.** Debes asegurarte de que no haya una configuración global de otro estudiante.

### 4.1. Ver si hay una configuración global existente

```bash
git config --global user.name
git config --global user.email
```

### 4.2. Ver toda la configuración global

```bash
git config --global --list
```

### 4.3. ¿Qué hacer con los resultados?

| Resultado | Acción |
|-----------|--------|
| No muestra nada | ✅ Perfecto, no hay configuración global. Continúa al **Paso 4** |
| Muestra datos de otra persona | ⚠️ No los borres; es una computadora compartida. Continúa al **Paso 4**, usarás configuración LOCAL |
| Muestra tus datos | Continúa al **Paso 4** |

### 4.4. Verificar si se almacenan credenciales (credential helper)

```bash
git config --global credential.helper
```

| Resultado | Significado |
|-----------|------------|
| `manager` o `manager-core` | Las credenciales se guardan automáticamente. **Riesgo de seguridad en labs** |
| `store` | Las credenciales se guardan en texto plano. **Riesgo alto** |
| (vacío) | No se almacenan. ✅ Seguro |

> Si muestra `manager` o `store`, ve el **Paso 8** para desactivarlo.

---

## 5. Paso 4: Crear tu repositorio en GitHub

1. Ve a [github.com/new](https://github.com/new) e inicia sesión.
2. Llena los campos:
   - **Repository name**: el nombre de tu proyecto (ejemplo: `POO-4TO-SOFTWARE-2026-TuNombre`)
   - **Description**: (opcional) Breve descripción
   - **Visibility**: `Public` o `Private` según indique el profesor
3. **NO marques** ninguna de estas opciones:
   - ❌ Add a README file
   - ❌ Add .gitignore
   - ❌ Choose a license
4. Haz clic en **Create repository**.
5. **No cierres esta página.** Necesitarás la URL del repositorio (se ve como `https://github.com/tu-usuario/nombre-del-repo.git`).

---

## 6. Paso 5: Crear un Token de Acceso Personal (PAT)

> **¿Por qué necesito esto?**
> GitHub ya no acepta contraseñas para autenticarse desde la terminal. Necesitas un token que funciona como una contraseña especial.

### 6.1. Generar el token

1. Ve a [github.com/settings/tokens](https://github.com/settings/tokens) (inicia sesión si te lo pide).
2. Haz clic en **"Tokens (classic)"** en el menú lateral.
3. Haz clic en **"Generate new token"** → **"Generate new token (classic)"**.
4. Completa:
   - **Note**: `Lab UNEMI POO` (un nombre para identificar el token)
   - **Expiration**: Selecciona `90 days` o `Custom` con fecha de fin del semestre
   - **Scopes (permisos)**: Marca **solo** ✅ `repo` (el primero de la lista)
5. Haz clic en **Generate token**.

### 6.2. Copiar y guardar el token

> **⚠️ MUY IMPORTANTE:** El token **solo se muestra UNA VEZ**. Si lo pierdes, tendrás que crear uno nuevo.

El token se ve similar a:

```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### ¿Dónde guardarlo?

- **Opción recomendada:** Guárdalo en un **gestor de contraseñas** (LastPass, Bitwarden, etc.)
- **Opción alternativa:** Guárdalo en una **nota privada en tu celular**
- **❌ NUNCA lo guardes** en archivos dentro del repositorio, notas adhesivas en la computadora, o documentos compartidos.

---

## 7. Paso 6: Inicializar tu repositorio local

Abre una terminal y navega a la carpeta de tu proyecto:

```bash
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"
```

### 7.1. Verificar si ya es un repositorio Git

```bash
git status
```

| Resultado | Significado | Acción |
|-----------|------------|--------|
| Muestra información de rama y archivos | Ya es un repositorio Git | Salta al **Paso 7** |
| `fatal: not a git repository` | No está inicializado | Continúa abajo |

### 7.2. Inicializar el repositorio

```bash
git init
```

Resultado esperado:

```
Initialized empty Git repository in D:/UNEMI/2026/.../POO-4TO-SOFTWARE-2026/.git/
```

### 7.3. Crear un archivo .gitignore

Crea un archivo `.gitignore` para excluir archivos innecesarios:

```bash
echo node_modules/ > .gitignore
echo .env >> .gitignore
echo *.class >> .gitignore
echo .idea/ >> .gitignore
echo .vscode/ >> .gitignore
echo __pycache__/ >> .gitignore
```

> Esto evita subir archivos temporales o de configuración local a GitHub.

---

## 8. Paso 7: Configurar tu identidad LOCAL

> **⚠️ CLAVE:** Usamos configuración **LOCAL** (sin `--global`) para que solo aplique a **este proyecto**, sin afectar la configuración de otros estudiantes.

### 8.1. Configurar nombre y correo

```bash
git config user.name "Tu Nombre Completo"
git config user.email "tu-correo@ejemplo.com"
```

> **Usa el mismo correo que tienes registrado en GitHub.**

### Ejemplo concreto:

```bash
git config user.name "Carlos Pérez"
git config user.email "carlos.perez@gmail.com"
```

### 8.2. Verificar que se guardó correctamente

```bash
git config user.name
git config user.email
```

Debe mostrar **tu nombre y correo**.

### 8.3. Entender la diferencia entre LOCAL y GLOBAL

| Configuración | Comando | Alcance | Uso recomendado |
|--------------|---------|---------|-----------------|
| **Global** | `git config --global` | Todos los repositorios de la máquina | Computadora personal |
| **Local** | `git config` (sin --global) | Solo el repositorio actual | Computadoras compartidas (labs) |

> La configuración **local** tiene **prioridad** sobre la global. Aunque otro estudiante haya configurado algo de forma global, tu configuración local prevalecerá en tu carpeta.

---

## 9. Paso 8: Desactivar el almacenamiento de credenciales

> **¿Por qué?** En una computadora compartida, si Git guarda tus credenciales, el siguiente estudiante podría hacer push a **tu repositorio**. Esto lo evita.

### 9.1. Desactivar a nivel del repositorio

```bash
git config credential.helper ""
```

### 9.2. Verificar

```bash
git config credential.helper
```

Debe mostrar una **línea vacía** (sin nada).

### 9.3. Limpiar credenciales previas del almacén de Windows

Si la computadora tiene el Credential Manager de Windows activado, limpia credenciales previas:

**En CMD:**

```cmd
cmdkey /delete:git:https://github.com
```

**En PowerShell:**

```powershell
cmdkey /delete:git:https://github.com
```

> Si dice "Element not found", significa que no había credenciales guardadas. Eso es bueno ✅.

---

## 10. Paso 9: Conectar con GitHub y subir tu código

### 10.1. Agregar archivos al staging

```bash
git add .
```

### 10.2. Verificar qué archivos se van a subir

```bash
git status
```

Deberías ver los archivos en verde bajo "Changes to be committed".

### 10.3. Crear el primer commit

```bash
git commit -m "Commit inicial del proyecto POO"
```

### 10.4. Renombrar la rama principal

```bash
git branch -M main
```

### 10.5. Conectar con el repositorio remoto

```bash
git remote add origin https://github.com/tu-usuario/nombre-del-repo.git
```

> Reemplaza la URL con la de **tu repositorio** (la que copiaste en el Paso 4).

### 10.6. Subir el código

```bash
git push -u origin main
```

### 10.7. Autenticación

Git te pedirá credenciales:

```
Username for 'https://github.com': tu-usuario-de-github
Password for 'https://tu-usuario@github.com': (pega tu token PAT aquí)
```

> **⚠️ IMPORTANTE:**
> - En el campo **Username**: escribe tu **usuario de GitHub** (no tu correo).
> - En el campo **Password**: pega tu **Token de Acceso Personal (PAT)** del Paso 5.
> - Al pegar el token, **no se mostrará nada en pantalla**. Esto es normal por seguridad. Solo pégalo y presiona Enter.

### 10.8. Verificar que todo subió

```bash
git status
```

Debe decir: `nothing to commit, working tree clean`

Ahora ve a tu repositorio en GitHub y verifica que los archivos aparezcan.

---

## 11. Paso 10: Flujo de trabajo diario en el laboratorio

Cada vez que llegues al laboratorio y quieras trabajar:

### 11.1. Al iniciar tu sesión

```bash
# 1. Navegar a tu carpeta
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"

# 2. Verificar tu identidad configurada
git config user.name
git config user.email

# 3. Si NO muestra tus datos, configúralos:
git config user.name "Tu Nombre"
git config user.email "tu-correo@gmail.com"

# 4. Asegurarte de que las credenciales no se guardan
git config credential.helper ""

# 5. Traer los últimos cambios del repositorio
git pull
```

### 11.2. Mientras trabajas

```bash
# Ver el estado de tus archivos
git status

# Agregar cambios
git add .

# Crear un commit descriptivo
git commit -m "Agregar clase Vehículo con herencia"
```

### 11.3. Al terminar de trabajar

```bash
# Subir tus cambios
git push

# Git te pedirá usuario y token cada vez (eso es correcto y seguro)
```

---

## 12. Protocolo de seguridad: Al terminar tu sesión

> **⚠️ OBLIGATORIO antes de dejar la computadora del laboratorio.**

Ejecuta estos comandos para limpiar tu información:

```bash
# 1. Eliminar tu configuración local de identidad
git config --unset user.name
git config --unset user.email

# 2. Limpiar credenciales del almacén de Windows (por si se guardaron)
cmdkey /delete:git:https://github.com

# 3. Verificar que se limpió
git config user.name
git config user.email
```

Ambos comandos de verificación deben devolver **líneas vacías**.

### Lista de verificación antes de irte

- [ ] ¿Hiciste `git push` para subir tus cambios?
- [ ] ¿Ejecutaste `git config --unset user.name`?
- [ ] ¿Ejecutaste `git config --unset user.email`?
- [ ] ¿Limpiaste las credenciales con `cmdkey /delete`?
- [ ] ¿Cerraste la terminal?

---

## 13. Solución de problemas frecuentes

### Problema: `remote: Support for password authentication was removed`

**Causa:** Estás usando tu contraseña de GitHub en lugar del token.

**Solución:** Usa el **Token de Acceso Personal (PAT)** en el campo Password (ver Paso 5).

### Problema: El commit aparece con el nombre de otro estudiante

**Causa:** No configuraste tu identidad local antes de hacer el commit.

**Solución:**
1. Configura tu identidad:
   ```bash
   git config user.name "Tu Nombre"
   git config user.email "tu-correo@gmail.com"
   ```
2. Corrige el último commit:
   ```bash
   git commit --amend --reset-author --no-edit
   ```

### Problema: `fatal: not a git repository`

**Causa:** No estás en la carpeta correcta o no se ha inicializado Git.

**Solución:**
```bash
# Verificar en qué carpeta estás
cd
# Navegar a la carpeta correcta
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"
# Si aún da error, inicializar
git init
```

### Problema: `error: failed to push some refs to...`

**Causa:** El repositorio remoto tiene cambios que tú no tienes localmente.

**Solución:**
```bash
git pull --rebase origin main
git push
```

### Problema: `fatal: remote origin already exists`

**Causa:** Ya se configuró un remoto previamente.

**Solución:**
```bash
# Ver el remoto actual
git remote -v

# Si la URL es incorrecta, actualizarla:
git remote set-url origin https://github.com/tu-usuario/nombre-repo.git
```

### Problema: Token expirado

**Síntoma:** Error 401 o "Bad credentials" al hacer push.

**Solución:** Genera un nuevo token en GitHub (repite el Paso 5).

### Problema: Subiste archivos que no debías (node_modules, .env, etc.)

**Solución:**
```bash
# Crear o actualizar .gitignore con las exclusiones
# Luego remover del tracking (sin borrar el archivo local)
git rm -r --cached node_modules/
git commit -m "Remover node_modules del tracking"
git push
```

---

## 14. Resumen rápido (cheat sheet)

### Al llegar al laboratorio

```bash
cd "ruta/a/tu/proyecto"
git config user.name "Tu Nombre"
git config user.email "tu@email.com"
git config credential.helper ""
git pull
```

### Durante la clase

```bash
git add .
git commit -m "Descripción del cambio"
```

### Antes de irte

```bash
git push
git config --unset user.name
git config --unset user.email
cmdkey /delete:git:https://github.com
```

### Comandos de verificación útiles

| Comando | Qué hace |
|---------|----------|
| `git status` | Ver estado de archivos |
| `git log --oneline -5` | Ver los últimos 5 commits |
| `git config user.name` | Ver nombre configurado |
| `git config user.email` | Ver correo configurado |
| `git remote -v` | Ver URL del repositorio remoto |
| `git config --list --local` | Ver toda la configuración local |

---

## 15. Glosario de términos

| Término | Definición |
|---------|-----------|
| **Git** | Sistema de control de versiones que registra los cambios en tus archivos |
| **GitHub** | Plataforma en la nube para almacenar repositorios Git |
| **HTTPS** | Protocolo de conexión segura basado en usuario/token |
| **PAT (Personal Access Token)** | Token especial que reemplaza a la contraseña para autenticarse en GitHub |
| **Repositorio (repo)** | Carpeta de tu proyecto gestionada por Git |
| **Commit** | Captura/fotografía del estado de tus archivos en un momento dado |
| **Push** | Enviar tus commits locales al repositorio remoto (GitHub) |
| **Pull** | Traer los cambios del repositorio remoto a tu máquina |
| **Remote (origin)** | La dirección del repositorio en GitHub conectada a tu repo local |
| **Branch (rama)** | Línea independiente de desarrollo. `main` es la rama principal |
| **Staging (add)** | Área temporal donde preparas los archivos antes de hacer un commit |
| **Credential Helper** | Mecanismo de Git para almacenar credenciales. Debe estar desactivado en labs |
| **Configuración local** | Configuración que aplica solo a un repositorio específico |
| **Configuración global** | Configuración que aplica a todos los repositorios de la máquina |
| **.gitignore** | Archivo que indica a Git qué archivos o carpetas debe ignorar |

---

> 📝 **Documento creado para la materia POO – 4to Software – UNEMI – Período Abril-Junio 2026**
