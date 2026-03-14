---
title: Tips para servidores Hetzner con Linux
date: 2026-03-14
tags: [linux, hetzner, ssh, vnc, chrome, swap]
status: active
---

# Tips para servidores Hetzner con Linux

Colección de recetas útiles para administrar servidores dedicados Hetzner.

---

## Túneles SSH

### Túnel local (forward)

Redirigir un puerto remoto al equipo local:

```bash
ssh -L 8080:localhost:8080 user@servidor
```

### Túnel reverso

Exponer un servicio local a través del servidor remoto:

```bash
ssh -R 9090:localhost:9090 user@servidor
```

### Túnel SOCKS (proxy dinámico)

```bash
ssh -D 1080 user@servidor
```

### Mantener la conexión viva

Agregar en `~/.ssh/config`:

```
Host hetzner
    HostName <IP_DEL_SERVIDOR>
    User root
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

---

## VNC: acceso como root vs. usuario agent

### Instalar servidor VNC

```bash
apt update && apt install -y tigervnc-standalone-server tigervnc-common
```

### Iniciar VNC como root

```bash
vncserver :1 -geometry 1920x1080 -depth 24
```

### Iniciar VNC como usuario agent

```bash
su - agent -c "vncserver :2 -geometry 1920x1080 -depth 24"
```

### Conectar vía túnel SSH (recomendado)

```bash
# Desde tu máquina local:
ssh -L 5901:localhost:5901 root@servidor   # para :1
ssh -L 5902:localhost:5902 agent@servidor  # para :2
```

Luego conectar el cliente VNC a `localhost:5901` o `localhost:5902`.

### Detener sesión VNC

```bash
vncserver -kill :1
```

---

## Ejecutar OpenClaw Gateway

### Configurar variables de entorno

```bash
export OPENAI_API_KEY="<OPENAI_API_KEY>"
export GATEWAY_PORT=8080
```

### Iniciar el gateway

```bash
cd /opt/openclaw-gateway
./openclaw-gateway --port "$GATEWAY_PORT"
```

### Ejecutar con systemd (persistente)

Crear `/etc/systemd/system/openclaw-gateway.service`:

```ini
[Unit]
Description=OpenClaw Gateway
After=network.target

[Service]
Type=simple
User=agent
EnvironmentFile=/etc/openclaw-gateway/env
ExecStart=/opt/openclaw-gateway/openclaw-gateway --port 8080
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable --now openclaw-gateway
```

El archivo `/etc/openclaw-gateway/env` debe contener:

```
OPENAI_API_KEY=<OPENAI_API_KEY>
```

---

## Automatización de navegador / depuración remota con Chrome

### Instalar Chrome headless

```bash
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/google-chrome.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list
apt update && apt install -y google-chrome-stable
```

### Lanzar Chrome con depuración remota

```bash
google-chrome-stable \
  --headless=new \
  --no-sandbox \
  --disable-gpu \
  --remote-debugging-port=9222 \
  --remote-debugging-address=0.0.0.0
```

### Conectar desde máquina local

```bash
ssh -L 9222:localhost:9222 user@servidor
```

Luego abrir `chrome://inspect` en el Chrome local y configurar el target `localhost:9222`.

---

## Perfil persistente de Chrome

Para mantener sesiones, cookies y extensiones entre reinicios:

```bash
google-chrome-stable \
  --headless=new \
  --no-sandbox \
  --disable-gpu \
  --remote-debugging-port=9222 \
  --user-data-dir=/home/agent/.chrome-profile
```

### Respaldar el perfil

```bash
tar czf chrome-profile-backup.tar.gz -C /home/agent .chrome-profile
```

### Restaurar el perfil

```bash
tar xzf chrome-profile-backup.tar.gz -C /home/agent
```

---

## Archivo de swap

### Crear swap (ejemplo: 4 GB)

```bash
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### Hacerlo permanente

Agregar al final de `/etc/fstab`:

```
/swapfile none swap sw 0 0
```

### Verificar swap activo

```bash
swapon --show
free -h
```

### Ajustar swappiness (opcional)

```bash
# Valor bajo = preferir RAM; valor alto = usar swap más agresivamente
sysctl vm.swappiness=10

# Hacerlo permanente:
echo "vm.swappiness=10" >> /etc/sysctl.conf
```

---

## Higiene de secretos

**Nunca almacenes credenciales en el repositorio git.** Esto incluye contraseñas, tokens de API, claves SSH privadas y cualquier dato sensible.

Buenas prácticas:

- Usa **variables de entorno** para inyectar secretos en tiempo de ejecución.
- Emplea un **gestor de secretos** como HashiCorp Vault, AWS Secrets Manager, `pass`, o `age`/`sops`.
- Agrega archivos sensibles a `.gitignore` (por ejemplo, `.env`, `*.pem`, `credentials.json`).
- Usa `EnvironmentFile=` en unidades systemd para cargar secretos desde archivos fuera del repo.
- Si accidentalmente haces commit de un secreto, **rota la credencial de inmediato** y limpia el historial con `git filter-repo`.
