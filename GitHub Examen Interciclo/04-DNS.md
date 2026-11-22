# 2. Configuración de Servidor DNS (BIND)

Instalación de BIND, creación de zona directa para `jerves-espinoza.lab` y pruebas de resolución desde Windows.

## Comandos en el Servidor (Linux)

**1. Instalar BIND:**
```bash
sudo dnf install -y bind bind-utils
```

**2. Respaldar y editar configuración principal:**
```bash
sudo cp -p /etc/named.conf /root/backups_interciclo/named.conf.$(date +%F_%H%M).bak
sudo nano /etc/named.conf
```

Contenido clave en `/etc/named.conf`:
```text
listen-on port 53 { 127.0.0.1; 192.168.0.107; };
allow-query     { localhost; 192.168.0.0/24; };

zone "." IN {
    type hint;
    file "named.ca";
};

zone "jerves-espinoza.lab" IN {
    type master;
    file "jerves-espinoza.lab.zone";
    allow-update { none; };
};
```

**3. Crear archivo de zona directa (`192.168.0.107`):**
```bash
sudo tee /var/named/jerves-espinoza.lab.zone << 'EOF' > /dev/null
\$TTL 86400
@   IN  SOA     ns1.jerves-espinoza.lab. admin.jerves-espinoza.lab. (
                2026112002  ; Serial
                3600        ; Refresh
                1800        ; Retry
                604800      ; Expire
                86400 )     ; Minimum TTL

; Registros NS
@       IN  NS      ns1.jerves-espinoza.lab.

; Registros A
@       IN  A       192.168.0.107
ns1     IN  A       192.168.0.107
srv     IN  A       192.168.0.107
web     IN  A       192.168.0.107
www     IN  A       192.168.0.107
ssh     IN  A       192.168.0.107
admin   IN  A       192.168.0.107
EOF
```

**4. Permisos, SELinux y validación de sintaxis:**
```bash
sudo chown root:named /var/named/jerves-espinoza.lab.zone
sudo chmod 640 /var/named/jerves-espinoza.lab.zone
sudo restorecon -Rv /var/named/

sudo named-checkconf /etc/named.conf
sudo named-checkzone jerves-espinoza.lab /var/named/jerves-espinoza.lab.zone
```

**5. Arrancar servicio y abrir Firewall:**
```bash
sudo systemctl enable --now named
sudo systemctl restart named
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
```

## Comandos en Cliente Windows (PowerShell como Administrador)

**1. Apuntar DNS de Wi-Fi al servidor y limpiar caché:**
```powershell
netsh interface ip set dns name="Wi-Fi 3" static 192.168.0.107
ipconfig /flushdns
```

**2. Verificar resolución de nombres:**
```powershell
nslookup web.jerves-espinoza.lab 192.168.0.107
nslookup ssh.jerves-espinoza.lab 192.168.0.107
```

**3. Restaurar DNS automático (SOLO al terminar la práctica):**
```powershell
netsh interface ip set dns name="Wi-Fi 3" dhcp
```

## Evidencia

<img width="1645" height="886" alt="Captura de pantalla 2026-11-21 112720" src="https://github.com/user-attachments/assets/deaee655-c359-4e33-9778-02b61837009f" />

<img width="1102" height="633" alt="Captura de pantalla 2026-11-21 112825" src="https://github.com/user-attachments/assets/f4c1f0d0-8ac8-4c4f-af3c-77af2f5b4aa0" />

## Posibles errores y soluciones

* **Error al reiniciar `named` (`Job for named.service failed`):** Error de sintaxis en `/etc/named.conf` o en el archivo `.zone` (falta un punto y coma `;` o un punto final `.` en el dominio). Ejecuta `named-checkconf` o `named-checkzone` para ver la línea exacta del fallo.
* **`Permission denied` al leer la zona:** El dueño del archivo no es el grupo `named` o SELinux bloquea la lectura. Solución: volver a ejecutar `sudo chown root:named` y `sudo restorecon -Rv /var/named/`.
* **`nslookup` da `Request timed out` desde Windows:** El firewall de Linux no tiene abierto el puerto 53 (`firewall-cmd --add-service=dns`) o la máquina virtual no está en modo **Adaptador Puente (Bridged)** en la misma subred `192.168.0.x`.
* **Error en `netsh` (`El nombre de archivo, el nombre de directorio... no es válido`):** El nombre de tu tarjeta de red no es `"Wi-Fi 3"`. Verifica el nombre exacto con `netsh interface show interface`.

---
Anterior: [1. Red y Ping](01-red-y-ping.md) | Siguiente: [3. Servicios HTTP (Apache)](03-http.md)
