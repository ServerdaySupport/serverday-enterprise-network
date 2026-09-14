# Troubleshooting — Escenarios Documentados

## Metodología

Cada escenario sigue el formato profesional estándar:
PROBLEMA → SÍNTOMA → DIAGNÓSTICO → CAUSA RAÍZ → SOLUCIÓN → VERIFICACIÓN


## Escenario 1 — Puerto de acceso en VLAN incorrecta

**Problema:** PC-Ventas sin conectividad de red.

**Síntoma:** Ping al gateway 192.168.30.1 falla. El cable está 
conectado y el puerto aparece activo.

**Diagnóstico:**

show vlan brief

FastEthernet0/1 aparece en VLAN 99 en lugar de VLAN 30.

**Causa raíz:** Puerto asignado a VLAN de Management en lugar 
de VLAN de Ventas.

**Solución:**

interface FastEthernet0/1
switchport access vlan 30


**Verificación:** Ping al gateway 192.168.30.1 responde.


## Escenario 2 — DHCP Relay eliminado

**Problema:** PCs de VLAN 30 no obtienen IP automáticamente.

**Síntoma:** "DHCP request failed" en PC-Ventas.

**Diagnóstico:**

show running-config | section GigabitEthernet0/1.30

El comando ip helper-address no aparece en la subinterfaz.

**Causa raíz:** El DHCP Relay fue eliminado de la subinterfaz 
durante una reconfiguración.

**Solución:**

interface GigabitEthernet0/1.30
ip helper-address 192.168.50.10


**Verificación:** PC-Ventas obtiene IP automáticamente por DHCP.



## Escenario 3 — OSPF desactivado en sucursal

**Problema:** Sucursal Valencia sin conectividad con HQ.

**Síntoma:** PC-Valencia no alcanza ninguna red de Caracas.

**Diagnóstico:**

show ip ospf neighbor

R3 no aparece en la tabla de vecinos OSPF de R1.

show ip route

La red 192.168.100.0 no aparece en la tabla de rutas de R1.

**Causa raíz:** El proceso OSPF fue eliminado en R3.

**Solución:**

router ospf 1
network 10.0.12.0 0.0.0.3 area 0
network 192.168.100.0 0.0.0.255 area 0


**Verificación:** R3 aparece en show ip ospf neighbor con 
estado FULL. Ping de PC-Valencia a SRV-Interno responde.



## Escenario 4 — ACL bloqueando tráfico legítimo

**Problema:** VLAN Ventas sin conectividad hacia ningún destino.

**Síntoma:** Ping desde PC-Ventas falla hacia cualquier destino.

**Diagnóstico:**

show ip access-lists

POLITICA-VENTAS no tiene la línea permit ip any any al final. 
Los contadores muestran todo el tráfico siendo bloqueado.

**Causa raíz:** La regla permit ip any any fue eliminada 
accidentalmente dejando un deny implícito total.

**Solución:**

ip access-list extended POLITICA-VENTAS
deny ip 192.168.30.0 0.0.0.255 192.168.50.0 0.0.0.255
permit ip any any


**Verificación:** PC-Ventas alcanza otros destinos. 
Ping a 192.168.50.0 sigue bloqueado — comportamiento correcto.
