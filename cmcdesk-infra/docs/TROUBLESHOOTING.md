# Troubleshooting CmcDesk

## "No está listo" / cliente no confirma con el servidor (Reserved IP rompe el UDP)

### Síntoma
- El cliente queda en **"no listo" / "not ready"** indefinidamente.
- En el log del cliente (`~/.local/share/logs/RustDesk/rustdesk_rCURRENT.log`):
  ```
  register_pk of <IP>:21116 due to key not confirmed   (se repite sin parar)
  ```
- En el servidor, hbbs **sí** loguea `update_pk <ID>` (el registro llega), pero el cliente
  nunca lo confirma.
- Las claves del cliente y del servidor **coinciden** (no es mismatch de key).

### Causa raíz
La **Reserved IP de DigitalOcean** (`24.144.66.47`) es una IP con **NAT 1:1**, no está
en la interfaz del droplet. Funciona para TCP (SSH anda), pero **rompe el UDP** del
rendezvous de RustDesk:

```
Cliente  ──UDP──►  24.144.66.47:21116   (Reserved IP)
Servidor ──UDP──►  responde DESDE 143.198.170.154   (IP real / ancla)
Cliente: "recibí UDP de una IP que no esperaba" → lo DESCARTA → nunca confirma
```

Se diagnostica con `tcpdump` en el servidor:
```bash
tcpdump -i any -nn "host <IP_DEL_CLIENTE>"
# Se ve: In  -> dst Reserved IP   |   Out -> src IP REAL  (asimetría = bug)
```

### Solución
Apuntar los clientes a la **IP real del droplet** (`143.198.170.154`, la de `eth0`),
NO a la Reserved IP. Con la IP real, entrada y salida usan la misma dirección y el UDP
funciona.

- Config Linux (`~/.config/rustdesk/RustDesk2.toml`): `custom-rendezvous-server = '143.198.170.154'`
- Windows: nombre del `.exe` con `host=143.198.170.154`.

La Reserved IP queda solo para administración SSH (TCP), donde el NAT no molesta.

> Para la Fase 2B (build propio), conviene usar un **dominio** (ej. `relay.cmcdesk.ar`)
> apuntando a la IP real, así si el droplet cambia, no hay que recompilar clientes.

---

## Recuperar acceso SSH si te quedás afuera

La consola web de DigitalOcean (Droplet → "Console") entra sin SSH. Si pide password y
no lo tenés, usá "Reset Root Password". Último recurso: **Rebuild** del droplet (reinstala
el SO; conserva IP y llave SSH; se vuelve a montar el server con `server/docker-compose.yml`).
