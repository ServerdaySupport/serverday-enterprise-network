# Diseño de VLANs y Trunking

## ¿Por qué VLANs?

Sin VLANs todos los dispositivos de la empresa están en la misma red 
plana. Un equipo infectado puede ver el tráfico de todos los demás. 
Las VLANs crean segmentos lógicos separados dentro del mismo 
hardware físico.

## VLANs Configuradas

| VLAN ID | Nombre | Justificación |
|---|---|---|
| 10 | Gerencia | Aísla el tráfico ejecutivo |
| 20 | IT | Protege la administración de sistemas |
| 30 | Ventas | Separa el tráfico comercial |
| 40 | RRHH | Protege datos de personal |
| 50 | Servidores | Zona de mayor protección |
| 99 | Management | Administración de switches |

## Asignación de Puertos

### SW1-HQ
| Puerto | VLAN | Dispositivo |
|---|---|---|
| FastEthernet0/1 | 10 | PC-Gerencia |
| FastEthernet0/2 | 20 | PC-IT |
| GigabitEthernet0/1 | Trunk | R2-HQ-Internal |
| FastEthernet0/24 | Trunk | SW2-HQ |

### SW2-HQ
| Puerto | VLAN | Dispositivo |
|---|---|---|
| FastEthernet0/1 | 30 | PC-Ventas |
| FastEthernet0/2 | 40 | PC-RRHH |
| FastEthernet0/3 | 50 | SRV-Interno |
| GigabitEthernet0/1 | Trunk | SW1-HQ |

## Trunking 802.1Q

Los enlaces trunk transportan múltiples VLANs etiquetadas entre 
dispositivos. La etiqueta 802.1Q identifica a qué VLAN pertenece 
cada trama.

VLANs permitidas en todos los trunks: 10, 20, 30, 40, 50, 99

## Router-on-a-Stick

R2 usa subinterfaces sobre GigabitEthernet0/1 para enrutar entre VLANs:

- GigabitEthernet0/1.10 → Gateway VLAN 10
- GigabitEthernet0/1.20 → Gateway VLAN 20
- GigabitEthernet0/1.30 → Gateway VLAN 30
- GigabitEthernet0/1.40 → Gateway VLAN 40
- GigabitEthernet0/1.50 → Gateway VLAN 50
- GigabitEthernet0/1.99 → Gateway VLAN 99
