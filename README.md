

## 🖥️ RustDesk – Selbstgehostete Remote-Desktop-Lösung mit Docker

[RustDesk](https://github.com/rustdesk/rustdesk) ist eine Open-Source-Alternative zu TeamViewer und AnyDesk. Mit einem eigenen Relay- und Rendezvous-Server behältst du die volle Kontrolle über deine Daten – sicher, schnell und plattformübergreifend.

---

### ⚙️ Voraussetzungen

- Docker (v20+ empfohlen)
- Docker Compose (v2+)
- Offene Ports: `21115`, `21116`, `21117`, `21118`

---

### 🚀 Schnellstart mit Docker Compose

1. **Erstelle die Datei `docker-compose.yml`:**

```yaml
version: '3.8'

services:
  hbbs:
    image: rustdesk/rustdesk-server:latest
    container_name: rustdesk-hbbs
    ports:
      - "21115:21115"   # TCP
      - "21116:21116"   # UDP
      - "21118:21118"   # TCP/HTTP
    command: hbbs -r YOUR.SERVER.IP
    restart: unless-stopped
    networks:
      - rustdesk-net

  hbbr:
    image: rustdesk/rustdesk-server:latest
    container_name: rustdesk-hbbr
    ports:
      - "21117:21117"   # TCP
    command: hbbr
    restart: unless-stopped
    networks:
      - rustdesk-net

networks:
  rustdesk-net:
    driver: bridge
```
🔐 Hinweis: Ersetze YOUR.SERVER.IP durch die öffentliche IP-Adresse oder Domain deines Servers.

## 📁 Serverdienste

| Dienst | Port | Beschreibung |
|--------|--------------|-------|
| **hbbs** | `21115` | Relay-Server (TCP) |
|  | `21116` | NAT-Punch (UDP) |
