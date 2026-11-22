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

<img width="1912" height="208" alt="Captura de pantalla 2026-11-21 113503" src="https://github.com/user-attachments/assets/4d563d3f-9f23-45e7-b62c-2c311ada4ae9" />

<img width="1283" height="613" alt="Captura de pantalla 2026-11-22 002046" src="https://github.com/user-attachments/assets/7f3f123e-b918-4be8-b11a-d8c77b94b4f8" />

<img width="1477" height="646" alt="Captura de pantalla 2026-11-20 214932" src="https://github.com/user-attachments/assets/5c8b89db-b6ee-4a48-ae1d-05b6349e0440" />

## Posibles errores y soluciones

* **`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!` (Host key verification failed):** Ocurre cuando clonas la VM o cambias la IP del servidor y Windows conserva la huella anterior. Ejecuta `ssh-keygen -R "[ssh.jerves-espinoza.lab]:2222"` y vuelve a conectarte.
* **`Connection refused` al conectar por SSH o SCP:** El servicio `sshd` no está escuchando en el puerto `2222` (puede seguir en el `22` por defecto) o está detenido. Revisa `/etc/ssh/sshd_config` (`Port 2222`) y reinicia con `sudo systemctl restart sshd`.
* **`Connection timed out`:** El puerto `2222/tcp` no está permitido en el firewall. Ejecuta `sudo firewall-cmd --permanent --add-port=2222/tcp && sudo firewall-cmd --reload`.
* **Error en `scp` (`unknown option -- p` o `cp: cannot stat`):** Se usó `-p` minúscula en lugar de `-P` mayúscula. En `ssh` el puerto va con `-p` minúscula, pero en `scp` siempre va con `-P` mayúscula.
* **`Permission denied` al subir archivos con SCP:** Intentaste subir el archivo a una ruta sin permisos (como `/root` o `/etc`). Asegúrate de enviarlo a `/home/jerves_espinoza/`.

---
Anterior: [3. Servicios HTTP](03-http.md) | [Volver al inicio](01-red-y-ping.md)

