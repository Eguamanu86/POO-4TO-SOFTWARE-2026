# Guía de Configuración Global con SSH para GitHub

## Dirigida a estudiantes de la materia POO – 4to Software

> **¿Cuándo usar esta guía?**
> Cuando trabajas en **tu computadora personal** y deseas una conexión segura y permanente con GitHub sin necesidad de ingresar credenciales en cada operación.

---

## Tabla de Contenidos

1. [Requisitos previos](#1-requisitos-previos)
2. [Paso 1: Verificar si Git está instalado](#2-paso-1-verificar-si-git-está-instalado)
3. [Paso 2: Instalar Git (si no lo tienes)](#3-paso-2-instalar-git-si-no-lo-tienes)
4. [Paso 3: Verificar configuración existente de Git](#4-paso-3-verificar-configuración-existente-de-git)
5. [Paso 4: Configurar identidad global de Git](#5-paso-4-configurar-identidad-global-de-git)
6. [Paso 5: Verificar si ya tienes claves SSH](#6-paso-5-verificar-si-ya-tienes-claves-ssh)
7. [Paso 6: Generar una nueva clave SSH](#7-paso-6-generar-una-nueva-clave-ssh)
8. [Paso 7: Agregar la clave SSH al agente SSH](#8-paso-7-agregar-la-clave-ssh-al-agente-ssh)
9. [Paso 8: Agregar la clave pública a GitHub](#9-paso-8-agregar-la-clave-pública-a-github)
10. [Paso 9: Verificar la conexión SSH con GitHub](#10-paso-9-verificar-la-conexión-ssh-con-github)
11. [Paso 10: Clonar o configurar un repositorio con SSH](#11-paso-10-clonar-o-configurar-un-repositorio-con-ssh)
12. [Paso 11: Flujo de trabajo diario](#12-paso-11-flujo-de-trabajo-diario)
13. [Solución de problemas frecuentes](#13-solución-de-problemas-frecuentes)
14. [Glosario de términos](#14-glosario-de-términos)

---

## 1. Requisitos previos

- Una computadora con Windows 10/11 (esta guía está orientada a Windows).
- Una cuenta de GitHub activa. Si no tienes una, créala en [github.com/signup](https://github.com/signup).
- Conexión a internet.

---

## 2. Paso 1: Verificar si Git está instalado

Abre una terminal (PowerShell, CMD o Git Bash) y ejecuta:

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

1. Ve a [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. Descarga el instalador para Windows.
3. Ejecuta el instalador y sigue estos pasos:
   - **Select Components**: deja las opciones por defecto.
   - **Default editor**: selecciona el editor que prefieras (se recomienda **Visual Studio Code**).
   - **Adjusting PATH**: selecciona **"Git from the command line and also from 3rd-party software"**.
   - **SSH executable**: selecciona **"Use bundled OpenSSH"**.
   - El resto de opciones déjalas por defecto.
4. Finaliza la instalación.
5. **Cierra y vuelve a abrir la terminal**.
6. Verifica nuevamente:

```bash
git --version
```

Deberías ver algo como: `git version 2.47.1.windows.1`

---

## 4. Paso 3: Verificar configuración existente de Git

Antes de configurar, revisa si ya tienes una configuración previa:

```bash
git config --global user.name
git config --global user.email
```

### ¿Qué esperar?

| Resultado | Significado |
|-----------|------------|
| Muestra un nombre y un correo | Ya tienes configuración. Verifica que sean **tus datos correctos**. Si son correctos, salta al **Paso 5** |
| No muestra nada (línea vacía) | No hay configuración. Continúa al **Paso 4** |
| Muestra datos de otra persona | Debes reconfigurar. Continúa al **Paso 4** |

Para ver **toda** la configuración global de un vistazo:

```bash
git config --global --list
```

---

## 5. Paso 4: Configurar identidad global de Git

Esta información aparecerá en cada commit que hagas. **Usa el mismo correo electrónico que registraste en GitHub.**

```bash
git config --global user.name "Tu Nombre Completo"
git config --global user.email "tu-correo@ejemplo.com"
```

### Ejemplo:

```bash
git config --global user.name "María López"
git config --global user.email "maria.lopez@gmail.com"
```

### Verificar que se guardó correctamente:

```bash
git config --global user.name
git config --global user.email
```

> **⚠️ Importante:** Si usas `--global`, esta configuración aplica a **todos los repositorios** de tu máquina. Esto es ideal para tu computadora personal.

---

## 6. Paso 5: Verificar si ya tienes claves SSH

Antes de generar una clave nueva, revisa si ya existe una:

```bash
ls ~/.ssh/
```

Si usas **CMD** de Windows:

```cmd
dir %USERPROFILE%\.ssh
```

### ¿Qué esperar?

| Resultado | Significado | Acción |
|-----------|------------|--------|
| Ves archivos como `id_ed25519` y `id_ed25519.pub` | Ya tienes claves SSH | Puedes reutilizarlas. Salta al **Paso 8** |
| Ves archivos como `id_rsa` y `id_rsa.pub` | Tienes claves antiguas (RSA) | Funcionan, pero se recomienda generar nuevas con Ed25519. Ve al **Paso 6** |
| Error o carpeta vacía | No tienes claves SSH | Continúa al **Paso 6** |

---

## 7. Paso 6: Generar una nueva clave SSH

Abre **Git Bash** (se instaló junto con Git) y ejecuta:

```bash
ssh-keygen -t ed25519 -C "tu-correo@ejemplo.com"
```

> Reemplaza `tu-correo@ejemplo.com` con el correo de tu cuenta de GitHub.

### Durante la ejecución te preguntará:

**Pregunta 1:** `Enter file in which to save the key (/c/Users/TuUsuario/.ssh/id_ed25519):`
- **Acción:** Presiona **Enter** para aceptar la ubicación por defecto.

**Pregunta 2:** `Enter passphrase (empty for no passphrase):`
- **Acción:** Puedes escribir una contraseña para mayor seguridad o presionar **Enter** para dejarla vacía (más cómodo pero menos seguro).

**Pregunta 3:** `Enter same passphrase again:`
- **Acción:** Repite la contraseña o presiona **Enter** si la dejaste vacía.

### Resultado esperado:

```
Your identification has been saved in /c/Users/TuUsuario/.ssh/id_ed25519
Your public key has been saved in /c/Users/TuUsuario/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx tu-correo@ejemplo.com
```

### Verificar que se crearon los archivos:

```bash
ls ~/.ssh/
```

Debes ver:
- `id_ed25519` → tu clave **privada** (🔒 NUNCA la compartas)
- `id_ed25519.pub` → tu clave **pública** (esta es la que subirás a GitHub)

---

## 8. Paso 7: Agregar la clave SSH al agente SSH

El agente SSH administra tus claves para que no tengas que escribir la passphrase cada vez.

### En Git Bash:

1. **Iniciar el agente SSH:**

```bash
eval "$(ssh-agent -s)"
```

Resultado esperado: `Agent pid 12345` (el número puede variar)

2. **Agregar tu clave privada al agente:**

```bash
ssh-add ~/.ssh/id_ed25519
```

Resultado esperado: `Identity added: /c/Users/TuUsuario/.ssh/id_ed25519 (tu-correo@ejemplo.com)`

### En Windows (PowerShell como Administrador):

Si prefieres usar PowerShell, primero asegúrate de que el servicio del agente esté activo:

```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

---

## 9. Paso 8: Agregar la clave pública a GitHub

### 9.1. Copiar la clave pública

En **Git Bash**:

```bash
cat ~/.ssh/id_ed25519.pub
```

En **CMD**:

```cmd
type %USERPROFILE%\.ssh\id_ed25519.pub
```

**Selecciona y copia TODO el texto** que aparece. Se ve similar a:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG... tu-correo@ejemplo.com
```

> ⚠️ Copia **todo**, desde `ssh-ed25519` hasta el final del correo, sin espacios extra.

### 9.2. Agregarla en GitHub

1. Ve a [github.com](https://github.com) e inicia sesión.
2. Haz clic en tu **foto de perfil** (esquina superior derecha) → **Settings**.
3. En el menú lateral izquierdo, haz clic en **SSH and GPG keys**.
4. Haz clic en el botón verde **New SSH key**.
5. Completa los campos:
   - **Title**: Un nombre descriptivo, por ejemplo: `Laptop Personal - Windows`
   - **Key type**: Déjalo en `Authentication Key`
   - **Key**: Pega la clave pública que copiaste
6. Haz clic en **Add SSH key**.
7. GitHub te pedirá confirmar tu contraseña.

---

## 10. Paso 9: Verificar la conexión SSH con GitHub

```bash
ssh -T git@github.com
```

### ¿Qué esperar?

**Primera vez**, te preguntará:

```
The authenticity of host 'github.com (IP)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

- Escribe **`yes`** y presiona Enter.

**Resultado exitoso:**

```
Hi tu-usuario! You've been successfully authenticated, but GitHub does not provide shell access.
```

✅ **¡Si ves este mensaje, la configuración SSH está completa!**

### Si ves un error:

| Error | Causa probable | Solución |
|-------|---------------|----------|
| `Permission denied (publickey)` | La clave no está en GitHub o el agente no la tiene | Repite los pasos 7 y 8 |
| `Connection timed out` | Firewall o proxy bloqueando SSH | Usa el puerto 443: ver sección de problemas |

---

## 11. Paso 10: Clonar o configurar un repositorio con SSH

### Opción A: Clonar un repositorio existente

```bash
git clone git@github.com:tu-usuario/nombre-del-repo.git
```

### Opción B: Conectar un repositorio local existente

Si ya tienes un repositorio local y quieres cambiarlo a SSH:

```bash
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"
git remote set-url origin git@github.com:tu-usuario/POO-4TO-SOFTWARE-2026.git
```

### Opción C: Crear un repositorio nuevo desde cero

```bash
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:tu-usuario/POO-4TO-SOFTWARE-2026.git
git push -u origin main
```

### Verificar la URL remota:

```bash
git remote -v
```

Debe mostrar URLs con formato `git@github.com:...` (no `https://`):

```
origin  git@github.com:tu-usuario/POO-4TO-SOFTWARE-2026.git (fetch)
origin  git@github.com:tu-usuario/POO-4TO-SOFTWARE-2026.git (push)
```

---

## 12. Paso 11: Flujo de trabajo diario

Una vez configurado todo, tu flujo de trabajo diario será:

```bash
# 1. Ir a tu carpeta del proyecto
cd "D:\UNEMI\2026\PERIODO-ABRIL-JUNIO\POO\POO-4TO-SOFTWARE-2026"

# 2. Verificar el estado de tus archivos
git status

# 3. Agregar los archivos modificados
git add .

# 4. Crear un commit con un mensaje descriptivo
git commit -m "Descripción clara de lo que hiciste"

# 5. Subir los cambios a GitHub
git push
```

### Traer cambios del repositorio remoto:

```bash
git pull
```

> Con SSH configurado, los comandos `git push` y `git pull` **no pedirán credenciales**.

---

## 13. Solución de problemas frecuentes

### Problema: `Permission denied (publickey)`

**Causa:** GitHub no reconoce tu clave SSH.

**Solución:**
1. Verifica que el agente tenga tu clave:
   ```bash
   ssh-add -l
   ```
2. Si no aparece, agrégala de nuevo:
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```
3. Verifica que la clave pública en GitHub coincide:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   Compara con la que está en GitHub → Settings → SSH keys.

### Problema: `Connection timed out` (redes con firewall)

Algunas redes bloquean el puerto 22. Usa SSH por el puerto 443:

Crea o edita el archivo `~/.ssh/config`:

```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```

Luego verifica:

```bash
ssh -T git@github.com
```

### Problema: `fatal: not a git repository`

**Causa:** No ejecutaste `git init` o no estás en la carpeta correcta.

**Solución:**
```bash
pwd
git init
```

### Problema: El commit aparece con otro nombre

**Causa:** La configuración de `user.name` o `user.email` no es correcta.

**Solución:**
```bash
git config user.name
git config user.email
```

Si los datos son incorrectos, reconfigúralos (ver Paso 4).

---

## 14. Glosario de términos

| Término | Definición |
|---------|-----------|
| **Git** | Sistema de control de versiones que registra los cambios en tus archivos |
| **GitHub** | Plataforma en la nube para almacenar repositorios Git |
| **SSH** | Protocolo de comunicación segura (Secure Shell) |
| **Clave privada** | Archivo secreto que identifica tu máquina. Nunca la compartas |
| **Clave pública** | Archivo que se comparte con servicios como GitHub para autenticarte |
| **Repositorio (repo)** | Carpeta de tu proyecto gestionada por Git |
| **Commit** | Captura/fotografía del estado de tus archivos en un momento dado |
| **Push** | Enviar tus commits locales al repositorio remoto (GitHub) |
| **Pull** | Traer los cambios del repositorio remoto a tu máquina |
| **Clone** | Descargar una copia completa de un repositorio remoto |
| **Remote (origin)** | La dirección del repositorio en GitHub conectada a tu repo local |
| **Branch (rama)** | Línea independiente de desarrollo. `main` es la rama principal |

---

> 📝 **Documento creado para la materia POO – 4to Software – UNEMI – Período Abril-Junio 2026**
