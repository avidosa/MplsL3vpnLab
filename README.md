# MplsL3vpnLab

Red de proveedor completa montada en laboratorio: un núcleo **MPLS con L3VPN** que da
servicio a dos clientes separados por VRF, con los accesos protegidos por **802.1X** y un
**FortiGate** en el perímetro. Diez equipos configurados y verificados.

Enero – mayo de 2026.

## La topología

| Equipos | Papel |
|---|---|
| `P1`, `P2` | núcleo MPLS: solo conmutación de etiquetas, sin rutas de cliente |
| `PE1`, `PE2` | *provider edge*: terminan las VRF y hablan BGP VPNv4 entre ellos |
| `CE-A1`, `CE-A2`, `CE-B1`, `CE-B3` | *customer edge* de los dos clientes |
| `SW-A`, `SW-B` | acceso: VLAN y autenticación 802.1X contra RADIUS |

Todo en GNS3, con Cisco IOSv 15.2 en el núcleo y FortiGate 7.0.9 sobre QEMU, corriendo
encima de Proxmox VE.

## Qué demuestra

- **OSPF + MPLS/LDP** en el núcleo, con las etiquetas distribuyéndose por LDP.
- **BGP VPNv4 con VRF**: `ip vrf CLIENTE` en los PE, `send-community extended` para
  transportar las *route targets*, y los PE anunciándose por `Loopback0`.
- **Separación real de clientes**: dos clientes con direccionamiento propio que no se ven
  entre ellos, compartiendo el mismo núcleo.
- **802.1X en el acceso**: `aaa new-model`, autenticación `dot1x` contra RADIUS y
  `dot1x system-auth-control` en los switches.
- **Montar el laboratorio de cero**: plantillas QEMU, importación de imágenes a GNS3,
  dependencias y drivers, y la topología levantada en cinco fases. Está todo documentado
  en capturas.

## Contenido

| Carpeta | Qué hay |
|---|---|
| [`configs/`](configs/) | la configuración en ejecución de los 10 equipos, exportada del laboratorio |
| [`docs/montaje/`](docs/montaje/) | 63 capturas: Proxmox, plantillas, importación a GNS3, las cinco fases de la topología y las verificaciones (`ping`, `show`) |

## Nota sobre las configuraciones

Son exportaciones literales del laboratorio, con una sola excepción: **la clave compartida de
RADIUS de `SW-A` está sustituida por `<RADIUS_SHARED_SECRET>`**. Era un valor de
laboratorio, pero una clave no se publica ni de broma.

El direccionamiento es privado y de laboratorio.

## Autoría

Autoría: mmoreno | avidosa

---

*Nota: dos capturas del montaje se han retirado porque mostraban el acceso por SSH al
entorno de laboratorio de un compañero.*
