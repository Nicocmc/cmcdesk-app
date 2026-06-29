# Servidor CmcDesk — Despliegue y operación

## Infraestructura

- **Proveedor:** DigitalOcean (Droplet `CmcDesk`, Ubuntu 24.04, 1 vCPU / 1 GB)
- **IP real del droplet (eth0):** `143.198.170.154` ← **la que usan los clientes**
- **Reserved IP:** `24.144.66.47` ← solo sirve para SSH (TCP); **NO usar para los clientes**
- **Clave pública del servidor:** `ChbJmIG2DBN2TLjzSTSQH1afp2w7SJpwXE4C+zBn1tU=`

> ⚠️ **Importante:** los clientes de RustDesk deben apuntar a la **IP real**
> (`143.198.170.154`), no a la Reserved IP. La Reserved IP de DigitalOcean hace NAT y
> el servidor responde el UDP del rendezvous desde la IP real → el cliente descarta esas
> respuestas y queda en "no listo". Detalle completo en `docs/TROUBLESHOOTING.md`.

## Acceso SSH

```bash
ssh -i ~/.ssh/cmcdesk_droplet -p 4040 root@143.198.170.154
```

- SSH solo por **puerto 4040** (el 22 está cerrado por firewall y sshd).
- Login por contraseña **deshabilitado**; root solo por llave.

## Puertos (firewall ufw activo en el host)

| Puerto | Protocolo | Uso |
|--------|-----------|-----|
| 4040 | TCP | SSH administración |
| 21115 | TCP | hbbs — test de NAT |
| 21116 | TCP + UDP | hbbs — registro/heartbeat + hole punching |
| 21117 | TCP | hbbr — relay |
| 21118 | TCP | hbbs — websocket (web client) |
| 21119 | TCP | hbbr — websocket |

> Si además usás el **Cloud Firewall** de DigitalOcean, abrí esos mismos puertos ahí.

## Operación

```bash
cd /opt/cmcdesk
docker compose ps           # estado
docker compose logs -f hbbs # ver clientes registrandose
docker compose restart      # reiniciar
docker compose pull && docker compose up -d   # actualizar
```

Los datos (clave del servidor, base de IDs) viven en `/opt/cmcdesk/data/`.
**Hacer backup de esa carpeta** antes de recrear el droplet.
