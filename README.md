# Examen Interciclo — Administración de Servicios en Red (DNS, HTTP y SSH)

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

## Conexión Remota al Servidor (CentOS vía MobaXterm)

En esta sección se evidencia la administración remota del servidor CentOS desde Windows utilizando el cliente SSH **MobaXterm** (conectado mediante el puerto `2222`), desde donde se ejecutaron las configuraciones y pruebas de los servicios.

# Examen Interciclo — Administración de Servicios en Red (DNS, HTTP y SSH)

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

## Conexión Remota al Servidor (CentOS vía MobaXterm)

En esta sección se evidencia la administración remota del servidor CentOS desde Windows utilizando el cliente SSH **MobaXterm** (conectado mediante el puerto `2222`), desde donde se ejecutaron las configuraciones y pruebas de los servicios.

<img width="1917" height="1016" alt="Captura de pantalla 2026-11-20 222356" src="https://github.com/user-attachments/assets/4abdc31f-2150-429c-8d1c-b882d3f89342" />

## Contenido del Repositorio

1. **[Verificación de Red, Ping y Respaldos](GitHub%20Examen%20Interciclo/01-Ping.md)** — Comprobación de interfaz IPv4, conectividad a Internet (`8.8.8.8`) y verificación de respaldos `.bak`.
2. **[Configuración de Servidor DNS (BIND)](GitHub%20Examen%20Interciclo/02-DNS.md)** — Instalación, archivo de zona directa, permisos SELinux, pruebas `nslookup` desde Windows y solución de errores.
3. **[Configuración de Servidor Web HTTP (Apache)](GitHub%20Examen%20Interciclo/03-HTTP.md)** — Despliegue de página HTML personalizada, ajustes de firewall/SELinux y solución de errores.
4. **[Acceso Remoto (SSH) y Transferencia (SCP)](GitHub%20Examen%20Interciclo/04-SSH_TCP.md)** — Creación de usuario, conexión por puerto `2222`, transferencia de archivos con `scp`, limpieza de huella y solución de errores.

## Contenido del Repositorio

1. **[Verificación de Red, Ping y Respaldos](GitHub%20Examen%20Interciclo/01-Ping.md)** — Comprobación de interfaz IPv4, conectividad a Internet (`8.8.8.8`) y verificación de respaldos `.bak`.
2. **[Configuración de Servidor DNS (BIND)](GitHub%20Examen%20Interciclo/02-DNS.md)** — Instalación, archivo de zona directa, permisos SELinux, pruebas `nslookup` desde Windows y solución de errores.
3. **[Configuración de Servidor Web HTTP (Apache)](GitHub%20Examen%20Interciclo/03-HTTP.md)** — Despliegue de página HTML personalizada, ajustes de firewall/SELinux y solución de errores.
4. **[Acceso Remoto (SSH) y Transferencia (SCP)](GitHub%20Examen%20Interciclo/04-SSH_TCP.md)** — Creación de usuario, conexión por puerto `2222`, transferencia de archivos con `scp`, limpieza de huella y solución de errores.
