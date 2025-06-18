

## 🖥️ RustDesk – Selbstgehostete Remote-Desktop-Lösung mit Docker

[RustDesk](https://github.com/rustdesk/rustdesk) ist eine Open-Source-Alternative zu TeamViewer und AnyDesk. Mit einem eigenen Relay- und Rendezvous-Server behältst du die volle Kontrolle über deine Daten – sicher, schnell und plattformübergreifend.

---

### ⚙️ Voraussetzungen

- Docker (v20+ empfohlen)
- Docker Compose (v2+)
- Offene Ports: `21115`, `21116`, `21117`, `21118`, `21119`

---

### 🚀 Schnellstart mit Docker Compose

1. **Erstelle die Datei `docker-compose.yml`:**

```yaml
services:
  rustdesk_eREW:
    image: rustdesk/rustdesk-server-s6:${VERSION}
    #    container_name: ${CONTAINER_NAME}
    deploy:
      resources:
        limits:
          cpus: ${CPUS}
          memory: ${MEMORY_LIMIT}
    environment:
        - RELAY=${RUSTDESK_HOST_ADDR}:${RUSTDESK_PORT_HBBR}
        - ENCRYPTED_ONLY=1
    ports:
        - ${HOST_IP}:${RUSTDESK_PORT_NAT}:21115
        - ${HOST_IP}:${RUSTDESK_PORT_HBBS}:21116
        - ${HOST_IP}:${RUSTDESK_PORT_HBBS}:21116/udp
        - ${HOST_IP}:${RUSTDESK_PORT_HBBR}:21117
        - ${HOST_IP}:${RUSTDESK_PORT_WEB_CLIENT_1}:21118
        - ${HOST_IP}:${RUSTDESK_PORT_WEB_CLIENT_2}:21119
    restart: always
    volumes:
        - ${APP_PATH}/data:/data
    labels:
      createdBy: "bt_apps"
    networks:
      - baota_net

networks:
  baota_net:
    external: true
```
2. **Erstelle die Datei `.env`:**
```env
VERSION=1.1.11
CONTAINER_NAME=CONTAINER_NAME
RUSTDESK_HOST_ADDR=
RUSTDESK_PORT_NAT=21115
RUSTDESK_PORT_HBBS=21116
RUSTDESK_PORT_HBBR=21117
RUSTDESK_PORT_WEB_CLIENT_1=21118
RUSTDESK_PORT_WEB_CLIENT_2=21119
HOST_IP=YOUR.SERVER.IP
CPUS=0
MEMORY_LIMIT=0MB
APP_PATH=/data/rustdesk/rustdesk_eREW
```

🔐 Hinweis: Ersetze YOUR.SERVER.IP durch die öffentliche IP-Adresse oder Domain deines Servers.
🔐 Hinweis: CPUS=0, MEMORY_LIMIT=0MB > Standart Unlimited Resources.

## 📁 Serverdienste

| Dienst | Port | Beschreibung |
|--------|--------------|-------|
| **hbbs** | `21115` | Relay-Server (TCP) |
|  | `21116` | NAT-Punch (UDP) |
|  | `21118` | HTTP Tunnel/Signal |
|  | `21119` | Webinterface / WebSocket Dienste ⚠️ |
| **hbbr** | `21117` | Rendezvous-Server (TCP) |



## 🧪 Client-Einstellungen
Starte den RustDesk-Client.

Gehe zu Settings → ID/Relay Server.

Trage deine IP oder Domain unter beiden Feldern ein.

Optional: E2EE konfigurieren über Public/Private Key.

## 🔧 Systembefehle
```
# Container starten
docker compose up -d

# Logs anzeigen
docker compose logs -f

# Container neustarten
docker compose restart
```

## 🔐 Sicherheit & Weiteres
Aktiviere SSH und Firewall für Serverhärtung.

Setze Public-Key-Verschlüsselung für sichere Kommunikation.

Optional: NGINX Reverse Proxy für Webzugriff auf 21118.
