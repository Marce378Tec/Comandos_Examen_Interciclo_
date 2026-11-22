# 4. Acceso Remoto (SSH) y Transferencia de Archivos (SCP)

Creación de usuario en el servidor, validación del servicio `sshd` en el puerto `2222`, conexión remota y transferencia segura de archivos (`scp`) desde Windows.

## Comandos en el Servidor (Linux)

**1. Crear usuario y asignar contraseña:**
```bash
sudo useradd jerves_espinoza
sudo passwd jerves_espinoza
```

**2. Verificar estado de SSH y reglas del firewall:**
```bash
sudo systemctl status sshd
sudo firewall-cmd --list-all
```
* `systemctl status sshd`: Confirma que el servicio OpenSSH está activo y escuchando en el puerto `2222`.
* `firewall-cmd --list-all`: Verifica que el puerto `2222/tcp` esté abierto.

## Comandos en Cliente Windows (CMD)

**1. Conectarse por SSH al puerto 2222:**
```cmd
ssh -p 2222 jerves_espinoza@ssh.jerves-espinoza.lab
```
* Inicia sesión remota en el servidor usando el subdominio configurado en DNS. *(Escribe `exit` para cerrar la sesión y regresar a Windows).*

**2. Transferencia de archivos con SCP (Subida y Descarga):**
```cmd
echo Archivo de prueba Jerves y Espinoza > prueba_subida.txt
scp -P 2222 prueba_subida.txt jerves_espinoza@ssh.jerves-espinoza.lab:/home/jerves_espinoza/
scp -P 2222 jerves_espinoza@ssh.jerves-espinoza.lab:/home/jerves_espinoza/prueba_subida.txt ./prueba_descargada.txt
dir prueba*
```
* `echo ... > prueba_subida.txt`: Crea un archivo de texto local en Windows para la prueba.
* `scp -P 2222 prueba_subida.txt ...:/home/jerves_espinoza/`: **Sube** el archivo local hacia la carpeta personal del usuario en el servidor Linux.
* `scp -P 2222 ...:/home/jerves_espinoza/prueba_subida.txt ./prueba_descargada.txt`: **Descarga** el archivo desde el servidor hacia Windows con un nuevo nombre.
* `dir prueba*`: Lista los archivos en Windows para comprobar que tanto el original como el descargado existen.

**3. Limpiar huella SSH (en caso de error de clave al reconectar):**
```cmd
ssh-keygen -R "[ssh.jerves-espinoza.lab]:2222"
```
* Elimina la clave antigua almacenada en `known_hosts` si el servidor cambió de IP o fue reinstalado.

**4. Restaurar DNS automático en PowerShell (SOLO al terminar la práctica):**
```powershell
netsh interface ip set dns name="Wi-Fi 3" dhcp
```

## Evidencia

![Creación de usuario SSH](img/04-ssh-usuario.png)

![Estado de sshd y firewall](img/04-ssh-status.png)

![Conexión SSH y transferencia SCP](img/04-ssh-scp.png)

## Posibles errores y soluciones

* **`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!` (Host key verification failed):** Ocurre cuando clonas la VM o cambias la IP del servidor y Windows conserva la huella anterior. Ejecuta `ssh-keygen -R "[ssh.jerves-espinoza.lab]:2222"` y vuelve a conectarte.
* **`Connection refused` al conectar por SSH o SCP:** El servicio `sshd` no está escuchando en el puerto `2222` (puede seguir en el `22` por defecto) o está detenido. Revisa `/etc/ssh/sshd_config` (`Port 2222`) y reinicia con `sudo systemctl restart sshd`.
* **`Connection timed out`:** El puerto `2222/tcp` no está permitido en el firewall. Ejecuta `sudo firewall-cmd --permanent --add-port=2222/tcp && sudo firewall-cmd --reload`.
* **Error en `scp` (`unknown option -- p` o `cp: cannot stat`):** Se usó `-p` minúscula en lugar de `-P` mayúscula. En `ssh` el puerto va con `-p` minúscula, pero en `scp` siempre va con `-P` mayúscula.
* **`Permission denied` al subir archivos con SCP:** Intentaste subir el archivo a una ruta sin permisos (como `/root` o `/etc`). Asegúrate de enviarlo a `/home/jerves_espinoza/`.

---
Anterior: [3. Servicios HTTP](03-http.md) | [Volver al inicio](01-red-y-ping.md)

