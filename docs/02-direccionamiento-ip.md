# Tabla de Direccionamiento IP

## Sede Principal — Caracas HQ

### Interfaces de Routers

| Dispositivo | Interfaz | IP | Máscara | Descripción |
|---|---|---|---|---|
| R1-HQ-Border | GigabitEthernet0/0 | 200.0.0.1 | /30 | Hacia Internet |
| R1-HQ-Border | GigabitEthernet0/1 | 172.16.100.1 | /24 | Zona DMZ |
| R1-HQ-Border | GigabitEthernet0/2 | 10.0.11.1 | /30 | Hacia R2 |
| R1-HQ-Border | Serial0/0/0 | 10.0.12.1 | /30 | Hacia Valencia |
| R1-HQ-Border | Serial0/0/1 | 10.0.13.1 | /30 | Hacia Maracaibo |
| R2-HQ-Internal | GigabitEthernet0/0 | 10.0.11.2 | /30 | Hacia R1 |
| R2-HQ-Internal | GigabitEthernet0/1.10 | 192.168.10.1 | /24 | Gateway VLAN Gerencia |
| R2-HQ-Internal | GigabitEthernet0/1.20 | 192.168.20.1 | /24 | Gateway VLAN IT |
| R2-HQ-Internal | GigabitEthernet0/1.30 | 192.168.30.1 | /24 | Gateway VLAN Ventas |
| R2-HQ-Internal | GigabitEthernet0/1.40 | 192.168.40.1 | /24 | Gateway VLAN RRHH |
| R2-HQ-Internal | GigabitEthernet0/1.50 | 192.168.50.1 | /24 | Gateway VLAN Servidores |
| R2-HQ-Internal | GigabitEthernet0/1.99 | 192.168.99.1 | /24 | Gateway VLAN Management |

### VLANs — Sede Caracas

| VLAN | Nombre | Red | Gateway | DHCP Inicio | DHCP Fin |
|---|---|---|---|---|---|
| 10 | Gerencia | 192.168.10.0/24 | 192.168.10.1 | .10 | .50 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 | .10 | .50 |
| 30 | Ventas | 192.168.30.0/24 | 192.168.30.1 | .10 | .100 |
| 40 | RRHH | 192.168.40.0/24 | 192.168.40.1 | .10 | .50 |
| 50 | Servidores | 192.168.50.0/24 | 192.168.50.1 | Estática | Estática |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | Estática | Estática |

### Servidores

| Servidor | IP | Servicios | VLAN |
|---|---|---|---|
| SRV-Interno | 192.168.50.10 | DHCP, DNS | 50 |
| SRV-DMZ | 172.16.100.10 | HTTP | DMZ |
| SRV-Internet | 200.0.0.2 | HTTP | Externa |

## Sucursales

### Valencia

| Dispositivo | Interfaz | IP | Descripción |
|---|---|---|---|
| R3-Branch-Valencia | Serial0/0/0 | 10.0.12.2 | Hacia R1 HQ |
| R3-Branch-Valencia | GigabitEthernet0/0 | 192.168.100.1 | LAN Valencia |
| PC-Valencia | FastEthernet | 192.168.100.10 | Estación de trabajo |

### Maracaibo

| Dispositivo | Interfaz | IP | Descripción |
|---|---|---|---|
| R4-Branch-Maracaibo | Serial0/0/0 | 10.0.13.2 | Hacia R1 HQ |
| R4-Branch-Maracaibo | GigabitEthernet0/0 | 192.168.200.1 | LAN Maracaibo |
| PC-Maracaibo | FastEthernet | 192.168.200.10 | Estación de trabajo |

## Enlaces WAN punto a punto (/30)

| Enlace | Red | IP R1 | IP Remota |
|---|---|---|---|
| HQ ↔ R2 | 10.0.11.0/30 | 10.0.11.1 | 10.0.11.2 |
| HQ ↔ Valencia | 10.0.12.0/30 | 10.0.12.1 | 10.0.12.2 |
| HQ ↔ Maracaibo | 10.0.13.0/30 | 10.0.13.1 | 10.0.13.2 |

Se usan subredes /30 en los enlaces WAN porque solo se necesitan 
2 direcciones IP utilizables por enlace — una para cada extremo.
