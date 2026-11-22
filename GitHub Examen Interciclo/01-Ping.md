# 1. Verificación de Red, Ping y Respaldos

Comprobación de interfaz de red, salida a Internet y creación/verificación del directorio de copias de seguridad.

## Comandos

**1. Verificar IP asignada y salida a Internet:**
```bash
ip -4 addr show
ping -c 10 8.8.8.8
```
* `ip -4 addr show`: Muestra la dirección IPv4 activa (`192.168.0.107`).
* `ping -c 10 8.8.8.8`: Envía 10 paquetes ICMP a Google para confirmar conectividad externa.

**2. Crear directorio de respaldos y verificar archivos `.bak` generados:**
```bash
sudo mkdir -p /root/backups_interciclo
sudo find / -name "*.bak" 2>/dev/null
```
* `mkdir -p`: Crea la ruta donde se almacenan las copias de seguridad de los archivos originales.
* `find / -name "*.bak"`: Confirma que los respaldos de DNS (`named.conf`) y Apache (`httpd.conf`, `welcome.conf`, `index.html`) se guardaron correctamente con marca de tiempo.

## Evidencia

![Verificación de IP y Ping](img/01-red-ping.png)

![Verificación de archivos de respaldo](img/01-backups.png)

---
Siguiente: [2. Configuración DNS (BIND)](02-dns.md)
