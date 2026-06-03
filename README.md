# Guía para instalar Git y configurar GitHub en Windows

Esta guía es para dejar listo Git en Windows, poder subir proyectos a GitHub y también saber qué hacer cuando ya tienes una llave SSH, pero necesitas agregar otra cuenta.

La idea es no borrar nada que ya exista y configurar todo con calma para evitar problemas después.

---

# 1. Primero: qué es Git y qué es GitHub

Git es el programa que se instala en la computadora. Sirve para guardar cambios del código, crear commits, manejar ramas y subir o bajar cambios.

GitHub es la página donde se guardan los repositorios en internet. Ahí puedes tener tus proyectos, compartirlos, trabajar con más personas o usarlos para subir código a producción.

---

# 2. Instalar Git en Windows

Para instalar Git en Windows, entra a la página oficial:

```powershell
https://git-scm.com/download/win
```

Descarga el instalador y ejecútalo y Durante la instalación puedes dejar casi todo como viene por defecto.
Algunas opciones recomendadas son:

1.- Editor por defecto: Visual Studio Code, si lo tienes instalado.
2.- PATH: Git from the command line and also from 3rd-party software.
3.- HTTPS transport backend: Use the OpenSSL library.
4.- Line endings: Checkout Windows-style, commit Unix-style.
5.- Terminal emulator: Use MinTTY.

Cuando termine la instalación, abre Git Bash.

---

# 3. Verificar que Git quedó instalado

En Git Bash o PowerShell escribe:
```powershell
git --version
```

Si aparece la versión, Git ya quedó instalado.

---

# 4. Configurar nombre y correo en Git

Esto es importante porque cada commit queda guardado con tu nombre y correo.
Comando para configurar el nombre:
```powershell
git config --global user.name "TU NOMBRE"
```
Comando para configurar el correo:
```powershell
git config --global user.email "TU CORREO"
```

Para revisar que sí se guardó:
```powershell
git config --global --list
```
---

# 5. Revisar si ya existen llaves SSH

Antes de crear una llave nueva, primero hay que revisar si ya tienes llaves en la computadora.

En Git Bash escribe:
```powershell
ls -al ~/.ssh
```
Si ya tienes llaves (Si no tienes ve al paso 6), puedes ver archivos como estos:

- id_ed25519
- id_ed25519.pub
- id_rsa
- id_rsa.pub

 Los archivos que terminan en .pub son las llaves públicas. Esas sí se pueden copiar y pegar en GitHub a otras cuentas (Como reutilziar).

Ejemplo:

- id_ed25519.pub

**Aquí hay que tener cuidado:** Los archivos que no tienen .pub son llaves privadas. **Esas no se comparten con nadie.**

Ejemplo:

- id_ed25519

---

# 6. Crear una llave SSH nueva

Si no tienes llave o quieres crear una nueva para una cuenta específica, usa este comando:

Si es nueva 
```powershell
ssh-keygen -t rsa -b 4096 -C "tu_correo_github@gmail.com"
```

Si sera otra
```powershell
ssh-keygen -t ed25519 -C "tu_correo_github@gmail.com" -f ~/.ssh/como_gustes_que_se_llame
```

Cuando pregunte dónde guardar la llave, puedes presionar Enter para usar el nombre por defecto.

La ruta normalmente queda así:
```powershell
/c/Users/TU_USUARIO/.ssh/tu_llaves
```

Después puede pedir una contraseña para la llave.
Puedes dejarla vacía con Enter o **ponerle una contraseña si quieres más seguridad.**

---
# 7. Copiar la llave pública

Para copiar la llave pública escribe:
```powershell
cat ~/.ssh/tu_llave.pub
```
Te va a mostrar algo parecido a esto:

- ssh-ed25519 AQUI_VA_LA_LLAVE TU_CORREO

Eso es lo que se copia y se pega en GitHub.

No copies la llave privada.
Solo copia la que termina en .pub.

---

# 8. Agregar la llave en GitHub

Entra a GitHub.

Luego ve a:
1.- Settings
2.- SSH and GPG keys
3.- New SSH key
4.- En Title puedes poner algo como:
5.- Laptop Windows
6.- En Key pegas la llave pública.
7.- Después guardas.

---

# 10. Probar la conexión con GitHub

En Git Bash escribe:
```powershell
ssh -T git@github.com
```
Si todo está bien, GitHub debe responder con un mensaje parecido a:
Hi usuario! You've successfully authenticated...
Eso significa que tu computadora ya se puede conectar con GitHub usando SSH.

---

# 11. Subir un proyecto nuevo a GitHub

1. Entra a GitHub.
2. Da clic en el botón **New repository**.
3. Escribe el nombre del repositorio.

Ejemplo:

```text
mi-proyecto
```

4. Selecciona si el repositorio será público o privado.

```text
Public  = cualquiera puede verlo
Private = solo tú o las personas invitadas pueden verlo
```

5. Si ya tienes archivos en tu computadora, puedes dejar sin marcar estas opciones:

```text
Add a README file
Add .gitignore
Choose a license
```

6. Da clic en:

```text
Create repository
```


7. Primero entra a la carpeta del proyecto.

Ejemplo:

cd /c/Users/TU_USUARIO/github/mi-proyecto

Inicializa Git:
```powershell
git init
```
Agrega los archivos:
```powershell
git add .
```
Crea el primer commit:
```powershell
git commit -m "Primer commit"
```
Cambia la rama principal a main:
```powershell
git branch -M main
```
Agrega el repositorio remoto (git hub en sus opciones te da para copear y pegar):
```powershell
git remote add origin git@github.com:LegoC/mi-proyecto.git
```

Sube el proyecto:
```powershell
git push -u origin main
```

Si acado te habre una pantalla de login, simplemente es para asociar y autorizar los cambios

---

# 11. Configurar varias llaves SSH cuando tienes más de una cuenta

Este paso solo aplica cuando tienes más de una llave SSH o más de una cuenta de GitHub en la misma computadora.

Por ejemplo, puede pasar que tengas:

- Una cuenta personal.
- Una cuenta de clases.
- Una cuenta de trabajo.
- Varias llaves SSH dentro de la carpeta .ssh.

El problema es que si usas directamente:

git@github.com:USUARIO/REPOSITORIO.git

Git puede tomar una llave que no corresponde a esa cuenta.
Por eso a veces aparece un error como:

**Permission denied publickey**

O también:

**Repository not found**

Aunque la llave sí exista.

Para evitar eso, se usa un archivo llamado config dentro de la carpeta .ssh.

---

## 11.1 Revisar las llaves que existen

Primero se revisan las llaves que ya tienes:

ls -al ~/.ssh

Puedes ver archivos como:

* id_ed25519
* id_ed25519.pub
* id_rsa
* id_rsa.pub
* clases
* clases.pub
* trabajo
* trabajo.pub

Los archivos que terminan en .pub son las llaves públicas.

Los archivos que no terminan en .pub son las llaves privadas.

Para configurar SSH se usan las llaves privadas, o sea las que no tienen .pub.

---

## 11.2 Crear o abrir el archivo config

El archivo debe llamarse exactamente:

config

No debe llamarse:

config.txt

Para abrirlo puedes usar:
```powershell
notepad ~/.ssh/config
```
Si no existe, se va a crear.

---

## 11.3 Agregar una configuración por cada cuenta

Dentro del archivo config puedes agregar una configuración por cada cuenta o por cada llave.

Ejemplo:

### Cuenta personal

Host github-personal
HostName github.com
User git
IdentityFile ~/.ssh/id_ed25519
IdentitiesOnly yes

### Cuenta de clases

Host github-clases
HostName github.com
User git
IdentityFile ~/.ssh/clases
IdentitiesOnly yes

### Cuenta de trabajo

Host github-trabajo
HostName github.com
User git
IdentityFile ~/.ssh/trabajo
IdentitiesOnly yes

Aquí lo importante es el nombre que va después de Host.

Por ejemplo:

- github-personal
- github-clases
- github-trabajo

Ese nombre se va a usar después en el remote del repositorio.

---

## 11.4 Probar cada configuración

Después de guardar el archivo config, se puede probar cada cuenta.

Para probar la cuenta personal:
```powershell
ssh -T git@github-personal
```
Para probar la cuenta de clases:
```powershell
ssh -T git@github-clases
```
Para probar la cuenta de trabajo:
```powershell
ssh -T git@github-trabajo
```
Si está bien configurado, GitHub debe responder algo parecido a:

Hi usuario! You've successfully authenticated, but GitHub does not provide shell access.

Eso significa que la llave sí está conectando con GitHub.

---

## 11.5 Cambiar el remote del proyecto

Cuando se usan varias llaves, no conviene dejar el remote así:

git@github.com:USUARIO/REPOSITORIO.git

Porque puede tomar la llave incorrecta.

Se debe cambiar por el Host que se configuró.

Ejemplo para la cuenta de clases:
```powershell
git remote set-url origin git@github-clases:USUARIO/REPOSITORIO.git
```
Ejemplo para la cuenta personal:
```powershell
git remote set-url origin git@github-personal:USUARIO/REPOSITORIO.git
```
Ejemplo para la cuenta de trabajo:
```powershell
git remote set-url origin git@github-trabajo:USUARIO/REPOSITORIO.git
```
---

## 11.6 Revisar que el remote quedó correcto

Para revisar el remote actual:
```powershell
git remote -v
```
Debe verse algo parecido a:
```powershell
origin git@github-clases:USUARIO/REPOSITORIO.git fetch

origin git@github-clases:USUARIO/REPOSITORIO.git push
```
---

