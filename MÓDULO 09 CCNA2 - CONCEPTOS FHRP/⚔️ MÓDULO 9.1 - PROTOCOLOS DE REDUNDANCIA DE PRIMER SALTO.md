
---
## Protocolos de Redundancia de Primer Salto (FHRP)

Los protocolos de redundancia de primer salto (First Hop Redundancy Protocols) están diseñados para proteger contra fallos en el _Default Gateway_ (puerta de enlace predeterminada), garantizando que los dispositivos finales no pierdan la conectividad hacia otras redes si el router principal falla.

### Limitaciones del Gateway Predeterminado

**El problema del punto único de falla:** Los dispositivos finales (PCs, servidores) generalmente se configuran con una única dirección IP de Gateway Predeterminado. Esta IP corresponde a la interfaz del router local.

**Impacto del fallo:** Si ese router físico falla, o su interfaz se desconecta, los hosts locales quedan aislados y no pueden comunicarse con redes externas (como Internet u otras VLANs), incluso si existe otro router de respaldo en la misma red física. Los hosts no tienen la capacidad de cambiar dinámicamente a un nuevo Gateway por sí solos.

### Redundancia del Router

Para solucionar la limitación anterior, se implementa la redundancia de router utilizando un **Router Virtual**.

**Concepto de Router Virtual:** Varios routers físicos se agrupan para presentar la ilusión de un único router a los hosts de la LAN.

**IP y MAC Virtuales:** A este grupo se le asigna una **IP virtual (VIP)** y una **Dirección MAC virtual**.

**Configuración del Host:** Los hosts se configuran apuntando su Gateway Predeterminado hacia la **IP virtual**, no hacia las direcciones físicas de los routers.

**Roles:** Dentro del grupo, un router físico asume el rol de **Activo** (retransmite el tráfico) y al menos otro asume el rol de **En espera / Standby** (monitorea al activo).

### Pasos para la Conmutación por Falla del Router

La transición o _failover_ ocurre de manera transparente para los usuarios. Cuando el router Activo sufre una falla, el proceso es el siguiente:

1. **Pérdida de comunicación:** El router en espera (_Standby_) deja de recibir mensajes de saludo (_Hello packets_) del router Activo.

2. **Asunción del rol:** El router en espera asume automáticamente el rol de router Activo.

3. **Control de la identidad virtual:** El nuevo router Activo asume la responsabilidad de la **IP virtual** y la **MAC virtual** del grupo.

4. **Continuidad del tráfico:** Como los hosts siguen enviando su tráfico a la misma IP y MAC virtuales, no notan el cambio físico del equipo y la red sigue funcionando sin interrupciones.

### Opciones de FHRP

Existen varios protocolos para implementar esta redundancia. Aquí están los principales y ejemplos de sus comandos básicos (tomando HSRP como referencia principal en entornos Cisco):

---

#### HSRP (Hot Standby Router Protocol)

**Descripción:** Protocolo propietario de Cisco. Un router está Activo y el otro en Standby.

**Comandos de configuración básicos (en la interfaz LAN del router):**

 ```
 Router(config)# interface GigabitEthernet0/1
  
 ! Asignar la IP física de la interfaz
 
 Router(config-if)# ip address 192.168.1.2 255.255.255.0

! Crear el grupo HSRP (ej. grupo 1) y asignar la IP Virtual (Gateway)

Router(config-if)# standby 1 ip 192.168.1.1

! Establecer prioridad (el valor más alto gana el rol de Activo, por defecto es 100)

Router(config-if)# standby 1 priority 150

! Habilitar "preemption" para que recupere su rol si se reinicia
   
Router(config-if)# standby 1 preempt
 ```

**Verificación:** `show standby` o `show standby brief`

---
#### VRRP (Virtual Router Redundancy Protocol)

**Descripción:** Es el protocolo estándar abierto (IEEE). Funciona de manera casi idéntica a HSRP, pero la terminología cambia: el router activo se llama **Master** y el de respaldo se llama **Backup**.

**Comandos de configuración básicos:** Se utiliza el comando `vrrp` en lugar de `standby`.


```
Router(config-if)# vrrp 1 ip 192.168.1.1

Router(config-if)# vrrp 1 priority 150

```

---

#### GLBP (Gateway Load Balancing Protocol)

**Descripción:** Protocolo propietario de Cisco. A diferencia de HSRP y VRRP (que tienen routers inactivos esperando), GLBP **permite el balanceo de carga**. Todos los routers en el grupo retransmiten tráfico simultáneamente (Activo/Activo).

**Funcionamiento:** Utiliza una única IP virtual, pero responde a las solicitudes ARP de los hosts con diferentes MAC virtuales, distribuyendo así el tráfico entre los routers físicos.

**Comandos de configuración básicos:**

```
Router(config-if)# glbp 1 ip 192.168.1.1

Router(config-if)# glbp 1 priority 150
```

-----------

