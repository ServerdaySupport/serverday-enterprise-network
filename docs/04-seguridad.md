# Políticas de Seguridad

## ACLs Extendidas — R2

### POLITICA-VENTAS
Aplicada en GigabitEthernet0/1.30 dirección in.

| Regla | Acción | Origen | Destino |
|---|---|---|---|
| 1 | deny | 192.168.30.0/24 | 192.168.50.0/24 |
| 2 | permit | any | any |

Ventas no puede acceder a la VLAN de Servidores.

### POLITICA-RRHH
Aplicada en GigabitEthernet0/1.40 dirección in.

| Regla | Acción | Origen | Destino |
|---|---|---|---|
| 1 | deny | 192.168.40.0/24 | 192.168.20.0/24 |
| 2 | permit | any | any |

RRHH no puede acceder a la VLAN de IT.

### POLITICA-SUCURSALES
Aplicada en GigabitEthernet0/0 dirección in.

| Regla | Acción | Origen | Destino |
|---|---|---|---|
| 1 | permit | 192.168.100.0/24 | 192.168.50.0/24 |
| 2 | permit | 192.168.200.0/24 | 192.168.50.0/24 |
| 3 | deny | 192.168.100.0/24 | 192.168.10.0/24 |
| 4 | deny | 192.168.100.0/24 | 192.168.20.0/24 |
| 5 | deny | 192.168.200.0/24 | 192.168.10.0/24 |
| 6 | deny | 192.168.200.0/24 | 192.168.20.0/24 |
| 7 | permit | any | any |

Sucursales acceden solo a Servidores. No acceden a Gerencia ni IT.

### POLITICA-DMZ
Aplicada en GigabitEthernet0/1 dirección in — R1.

| Regla | Acción | Tipo | Origen | Destino |
|---|---|---|---|---|
| 1 | permit | icmp echo | 192.168.0.0/16 | 172.16.100.0/24 |
| 2 | permit | icmp echo-reply | 172.16.100.0/24 | 192.168.0.0/16 |
| 3 | deny | ip | 172.16.100.0/24 | 192.168.0.0/16 |
| 4 | permit | ip | any | any |

La DMZ no puede iniciar conexiones hacia la red interna.

## SSH v2

Configurado en R1, R2, R3, R4, SW1, SW2.

- Dominio: garcia.local
- RSA: 1024 bits
- Versión: SSH v2
- Usuario: admin / privilege 15
- VTY: transport input ssh únicamente

Telnet está deshabilitado en todos los dispositivos.

## Port Security

Configurado en puertos de acceso de SW1 y SW2.

- Maximum: 1 MAC address por puerto
- Modo violación: restrict
- Aprendizaje: sticky (automático)
