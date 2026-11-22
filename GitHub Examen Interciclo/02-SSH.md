# 4. Acceso Remoto (SSH) y Transferencia de Archivos (SCP)

Creación de usuario en el servidor, validación del servicio `sshd` en el puerto `2222` y pruebas de conexión y transferencia desde Windows.

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

## Comandos en Cliente Windows (CMD)

**1. Conectarse por SSH al puerto 2222:**
```cmd
ssh -p 2222 jerves_espinoza@ssh.jerves-espinoza.lab
```
*(Escribe `exit` para cerrar la sesión remota y volver a Windows).*

**2. Probar subida y descarga de archivos con SCP:**
```cmd
echo Archivo de prueba Jerves y Espinoza > prueba_subida.txt
scp -P 2222 prueba_subida.txt jerves_espinoza@ssh.jerves-espinoza.lab:/home/jerves_espinoza/
scp -P 2222 jerves_espinoza@ssh.jerves-espinoza.lab:/home/jerves_espinoza/prueba_subida.txt ./prueba_descargada.txt
dir prueba*
```
* Nota: En `ssh` se usa `-p` minúscula para el puerto, mientras que en `scp` se usa `-P` mayúscula.

**3. Limpiar huella SSH si se reinstala/cambia el servidor y da error al reconectar:**
```cmd
ssh-keygen -R "[ssh.jerves-espinoza.lab]:2222"
```

**4. Restaurar DNS automático en PowerShell (SOLO al terminar el examen/práctica):**
```powershell
netsh interface ip set dns name="Wi-Fi 3" dhcp
```

## Evidencia

![Creación de usuario SSH](img/04-ssh-usuario.png)
*(Coloca aquí la quinta captura: comandos `useradd` y `passwd`)*

![Estado de sshd y firewall](img/04-ssh-status.png)
*(Coloca aquí la octava captura: `firewall-cmd --list-all` y `systemctl status sshd`)*

## Posibles errores y soluciones

* **`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!` (Host key verification failed):** Ocurre cuando clonas la VM o cambias la IP del servidor y Windows guarda la clave antigua. Ejecuta `ssh-keygen -R "[ssh.jerves-espinoza.lab]:2222"` para borrar el registro viejo y vuelve a conectar.
* **`Connection refused` al intentar conectar por SSH:** El servicio `sshd` no está escuchando en el puerto `2222` (puede seguir en el `22` por defecto) o está detenido. Revisa `/etc/ssh/sshd_config` (`Port 2222`) y reinicia con `sudo systemctl restart sshd`.
* **`Connection timed out`:** El puerto `2222/tcp` no está abierto en el firewall. Ejecuta `sudo firewall-cmd --permanent --add-port=2222/tcp && sudo firewall-cmd --reload`.
* **Error en `scp` (`unknown option -- p`):** Se usó `-p` minúscula en lugar de `-P` mayúscula para indicar el puerto en `scp`.

---
Anterior: [3. Servicios HTTP](03-http.md) | [Volver al inicio](01-red-y-ping.md)
