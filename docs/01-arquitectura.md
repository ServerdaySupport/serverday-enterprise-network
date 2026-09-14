# Arquitectura de Red — García Repuestos C.A.

## Descripción General

Red empresarial diseñada para una distribuidora venezolana con sede 
principal en Caracas y sucursales en Valencia y Maracaibo. La arquitectura 
sigue el modelo jerárquico de tres capas de Cisco: core, distribución y acceso.

## Zonas de Red

| Zona | Red | Descripción |
|---|---|---|
| Internet | 200.0.0.0/30 | Enlace hacia ISP simulado |
| DMZ | 172.16.100.0/24 | Servidor web público |
| WAN HQ-Valencia | 10.0.12.0/30 | Enlace serial punto a punto |
| WAN HQ-Maracaibo | 10.0.13.0/30 | Enlace serial punto a punto |
| WAN HQ-R2 | 10.0.11.0/30 | Enlace interno Caracas |
| LAN Caracas | 192.168.10-50.0/24 | Red interna segmentada |
| LAN Valencia | 192.168.100.0/24 | Red sucursal Valencia |
| LAN Maracaibo | 192.168.200.0/24 | Red sucursal Maracaibo |

## Dispositivos

| Dispositivo | Modelo | Rol |
|---|---|---|
| R1-HQ-Border | Cisco 2911 | Router de borde, NAT, DMZ |
| R2-HQ-Internal | Cisco 2911 | Router interno, inter-VLAN |
| R3-Branch-Valencia | Cisco 2911 | Router sucursal Valencia |
| R4-Branch-Maracaibo | Cisco 2911 | Router sucursal Maracaibo |
| SW1-HQ | Cisco 2960 | Switch principal HQ |
| SW2-HQ | Cisco 2960 | Switch secundario HQ |
| SW3-Valencia | Cisco 2960 | Switch sucursal Valencia |
| SW4-Maracaibo | Cisco 2960 | Switch sucursal Maracaibo |
| SRV-Interno | Server-PT | DHCP + DNS internos |
| SRV-DMZ | Server-PT | Servidor web público |
| SRV-Internet | Server-PT | Servidor externo simulado |

## Decisiones de Diseño

**¿Por qué Router-on-a-Stick en R2?**
R2 maneja el inter-VLAN routing usando subinterfaces sobre una sola 
interfaz física hacia SW1. Esto reduce el número de cables y puertos 
necesarios manteniendo la segmentación completa entre VLANs.

**¿Por qué topología en cascada SW1 → SW2?**
Los routers tienen puertos limitados. En lugar de conectar cada switch 
directamente a R2, SW2 se conecta a SW1 mediante un enlace trunk. 
SW1 actúa como switch de distribución y SW2 como switch de acceso.

**¿Por qué OSPF y no rutas estáticas?**
Con tres sedes y múltiples VLANs, las rutas estáticas serían difíciles 
de mantener. OSPF recalcula automáticamente ante fallos de enlace y 
propaga nuevas rutas sin intervención manual.

**¿Por qué DMZ separada?**
El servidor web público no debe estar en la red interna. Si es comprometido, 
el atacante queda contenido en la DMZ sin acceso a los sistemas internos.
