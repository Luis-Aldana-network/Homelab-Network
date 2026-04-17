Day 01 – Home Lab Initialization
# 🛠️ Red de Laboratorio Doméstico - Valledupar

Este repositorio documenta la configuración y el despliegue de mi HomeLab personal, centrado en la integración de servidores Linux con infraestructura MikroTik.

## 📡 Topología de Red
La red está segmentada para garantizar seguridad y orden en el tráfico de datos:
- **Router (Gateway):** MikroTik (IP: `172.16.10.1`)
- **Servidor:** HP Laptop (Ubuntu 24.04 LTS)
- **Segmento Servidores:** `172.16.10.0/24`

## 📔 Bitácora de Configuración (Día 01)

### Desafíos de Conectividad Resueltos:
Durante el despliegue inicial, enfrentamos problemas de enrutamiento y resolución de nombres. Aquí los aprendizajes clave:

1. **Gestión de Interfaces Dinámicas:**
   Ubuntu identificaba la tarjeta de red como `enp8s0` y `eno1` de forma intermitente. Se estandarizó la configuración mediante **Netplan** usando el `dhcp-identifier: mac` para asegurar una identidad única ante el DHCP del MikroTik.

2. **Resolución de Conflictos ARP:**
   Se detectaron IPs duplicadas en la tabla ARP del router. 
   - *Solución:* Limpieza manual de entradas en Winbox (`/ip arp remove`) y forzado de renovación de lease.

3. **Configuración de DNS (YAML):**
   Aprendizaje sobre la sintaxis estricta de Netplan para listas de servidores DNS, evitando errores de indentación.

### Configuración de Red Final (`/etc/netplan/01-netcfg.yaml`):
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp8s0: # O interfaz detectada como activa
      dhcp4: true
      dhcp-identifier: mac
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
