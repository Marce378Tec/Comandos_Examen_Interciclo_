# 3. Configuración de Servidor Web HTTP (Apache)

Despliegue de servidor web `httpd` con página HTML personalizada accesible mediante `http://jerves-espinoza.lab`.

## Comandos en el Servidor (Linux)

**1. Instalar Apache y respaldar archivos originales:**
```bash
sudo dnf install -y httpd
sudo cp -p /etc/httpd/conf/httpd.conf /root/backups_interciclo/httpd.conf.\$(date +%F_%H%M).bak
sudo cp -p /etc/httpd/conf.d/welcome.conf /root/backups_interciclo/welcome.conf.\$(date +%F_%H%M).bak
sudo cp -p /usr/share/testpage/index.html /root/backups_interciclo/testpage_index.html.\$(date +%F_%H%M).bak
```

**2. Guardar código HTML en `/usr/share/testpage/index.html`:**
```bash
sudo tee /usr/share/testpage/index.html << 'EOF' > /dev/null
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Sign In - Práctica académica</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      background: #121212;
      color: #fff;
      font-family: "Segoe UI", Arial, sans-serif;
      display: flex;
      justify-content: center;
    }
    .window-controls { position: absolute; top: 8px; right: 14px; display: flex; gap: 22px; font-size: 16px; color: #fff; user-select: none; }
    .logo { position: absolute; top: 42px; left: 28px; width: 34px; height: 38px; background: #fff; color: #121212; font-size: 9px; font-weight: 800; line-height: 1; text-align: center; padding-top: 8px; clip-path: polygon(0 0, 100% 0, 100% 80%, 50% 100%, 0 80%); }
    .login { width: 100%; max-width: 498px; padding: 55px 16px 24px; display: flex; flex-direction: column; align-items: center; }
    h1 { font-size: 14px; font-weight: 600; letter-spacing: .5px; margin-bottom: 22px; }
    .social { display: flex; gap: 9px; width: 100%; }
    .social button { flex: 1; height: 48px; border: 2px solid transparent; color: #fff; font-size: 20px; font-weight: 700; cursor: pointer; }
    .ps { background: #00a9f0; } .xbox { background: #107c10; } .nin { background: #e60012; } .fb { background: #3b5998; } .goog { background: #fff; color: #4285f4; }
    .social button:hover { filter: brightness(1.1); }
    .divider { width: 100%; display: flex; align-items: center; gap: 18px; margin: 20px 0 46px; font-size: 13px; }
    .divider::before, .divider::after { content: ""; flex: 1; height: 1px; background: #a0a0a0; }
    form { width: 100%; }
    input[type="email"], input[type="password"] { width: 100%; height: 48px; background: #070707; border: 1px solid #5a5a5a; color: #fff; font-size: 15px; padding: 0 16px; margin-bottom: 40px; outline: none; }
    input::placeholder { color: #8a8a8a; text-transform: uppercase; }
    input[type="email"]:focus, input[type="password"]:focus { border-color: #fff; }
    .options { display: flex; justify-content: space-between; align-items: center; margin-top: -4px; margin-bottom: 26px; font-size: 14px; }
    .remember { display: flex; align-items: center; gap: 14px; cursor: pointer; }
    .remember input { appearance: none; width: 20px; height: 20px; border: 1px solid #8fd49a; background: #070707; display: grid; place-content: center; cursor: pointer; }
    .remember input:checked::after { content: "✓"; color: #fff; font-size: 14px; }
    a { color: #8fd49a; text-decoration: none; }
    a:hover { text-decoration: underline; }
    .btn-signin { width: 100%; height: 46px; background: #88d19a; border: none; color: #fff; font-size: 14px; font-weight: 700; letter-spacing: .5px; cursor: pointer; }
    .btn-signin:hover { background: #99dcaa; }
    .privacy { display: block; text-align: center; margin-top: 12px; font-size: 13px; }
    .signup { margin-top: 20px; font-size: 14px; text-align: center; }
    .signup span { margin: 0 6px; }
    .settings { position: absolute; bottom: 28px; left: 38px; font-size: 18px; color: #fff; }
  </style>
</head>
<body>
  <div class="window-controls"><span>—</span><span>☐</span><span>✕</span></div>
  <div class="logo">EPIC<br>GAMES</div>
  <main class="login">
    <h1>SIGN IN</h1>
    <div class="social">
      <button class="ps" type="button" aria-label="PlayStation">PS</button>
      <button class="xbox" type="button" aria-label="Xbox">X</button>
      <button class="nin" type="button" aria-label="Nintendo">N</button>
      <button class="fb" type="button" aria-label="Facebook">f</button>
      <button class="goog" type="button" aria-label="Google">G</button>
    </div>
    <div class="divider">or</div>
    <form onsubmit="event.preventDefault(); alert('Esto es solo una práctica de diseño.');">
      <input type="email" placeholder="Email" required>
      <input type="password" placeholder="Password" required>
      <div class="options">
        <label class="remember">
          <input type="checkbox" checked> Remember Me
        </label>
        <a href="#">Forgot Your Password?</a>
      </div>
      <button class="btn-signin" type="submit">SIGN IN</button>
    </form>
    <a class="privacy" href="#">Privacy Policy</a>
    <p class="signup">
      Don't have an Epic Games account? <a href="#">Sign Up</a>
      <span>or</span> <a href="#">Sign In Later</a>
    </p>
  </main>
  <div class="settings">⚙</div>
</body>
</html>
EOF
```

**3. Permisos, contexto SELinux, arrancar Apache y abrir Firewall:**
```bash
sudo chmod 644 /usr/share/testpage/index.html
sudo restorecon -Rv /usr/share/testpage/
sudo systemctl enable --now httpd
sudo systemctl restart httpd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

**4. Verificar en navegador web (Windows):**
```text
http://jerves-espinoza.lab
```

## Evidencia

![Despliegue Apache en terminal](img/03-http-terminal.png)
*(Coloca aquí la sexta captura: permisos, `restorecon`, inicio de `httpd` y apertura de firewall)*

![Página cargada en navegador](img/03-http-navegador.png)
*(Coloca aquí la séptima captura: página cargada en el navegador)*

## Posibles errores y soluciones

* **Error `403 Forbidden` al abrir la web:** Permisos incorrectos en `index.html` o contexto de SELinux bloqueado. Asegúrate de ejecutar `sudo chmod 644 /usr/share/testpage/index.html` y `sudo restorecon -Rv /usr/share/testpage/`.
* **El navegador muestra la página por defecto de CentOS/RHEL en lugar de tu diseño:** Caché del navegador o archivo de bienvenida activo. Presiona `Ctrl + F5` en el navegador para forzar recarga sin caché.
* **`ERR_CONNECTION_TIMED_OUT` o no carga el sitio:** Falta habilitar el servicio HTTP en el firewall (`sudo firewall-cmd --permanent --add-service=http && sudo firewall-cmd --reload`).
* **`DNS_PROBE_FINISHED_NXDOMAIN` en el navegador:** Windows aún no tiene configurado el DNS `192.168.0.107` en la tarjeta de red o el navegador está usando "DNS seguro" (DoH). Desactiva *Usar DNS seguro* en la configuración de privacidad del navegador.

---
Anterior: [2. Configuración DNS](02-dns.md) | Siguiente: [4. Acceso Remoto SSH y SCP](04-ssh.md)
