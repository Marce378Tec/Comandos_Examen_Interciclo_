# Práctica de Administración de Servicios en Red (DNS, HTTP y SSH)

Documentación paso a paso de la configuración y pruebas de servicios de red sobre un servidor Linux (`srv-jerves-espinoza.lab`) y un cliente Windows.

## Datos del Entorno

* **Servidor Linux:** `srv-jerves-espinoza.lab` (CentOS / RHEL)
* **Dirección IP del Servidor:** `192.168.0.107/24`
* **Dominio Local:** `jerves-espinoza.lab`
* **Servicios Desplegados:**
  * **DNS (BIND):** Puerto `53` (UDP/TCP)
  * **Web (Apache HTTPD):** Puerto `80` (TCP)
  * **Acceso Remoto (OpenSSH):** Puerto `2222` (TCP)
* **Usuario de Prueba:** `jerves_espinoza`

## Contenido del Repositorio

1. **[Verificación de Red, Ping y Respaldos](01-red-y-ping.md)** — Comprobación de interfaz IPv4, conectividad a Internet (`8.8.8.8`) y creación de carpeta de respaldos.
2. **[Configuración de Servidor DNS (BIND)](02-dns.md)** — Instalación, archivo de zona directa, permisos SELinux, pruebas `nslookup` desde Windows y solución de errores.
3. **[Configuración de Servidor Web HTTP (Apache)](03-http.md)** — Despliegue de página HTML personalizada, ajustes de firewall/SELinux y solución de errores.
4. **[Acceso Remoto (SSH) y Transferencia (SCP)](04-ssh.md)** — Creación de usuario, conexión por puerto `2222`, transferencia de archivos con `scp`, limpieza de huella y solución de errores.
