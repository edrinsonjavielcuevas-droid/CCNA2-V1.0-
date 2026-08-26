---

---

---
### Enrutamiento Inter-VLAN en Switch de Capa 3

**El estándar empresarial:** Mientras que _router-on-a-stick_ es útil para redes pequeñas y medianas, las grandes empresas requieren switches de Capa 3 debido a su alta escalabilidad.

**Velocidad superior:** Los switches de Capa 3 realizan la conmutación (switching) basada en **hardware**, lo que les permite procesar y enrutar paquetes a velocidades mucho más altas que un router tradicional.

**Capacidades clave del hardware:**

**Enrutamiento mediante SVIs:** Pueden enrutar el tráfico entre distintas VLANs utilizando múltiples Interfaces Virtuales de Switch (SVI).

**Puertos enrutados:** Tienen la capacidad de convertir un puerto normal de switch (Capa 2) en una interfaz puramente de Capa 3, comportándose exactamente igual que la interfaz física de un router.

**Configuración obligatoria:** Para que exista el enrutamiento Inter-VLAN, es obligatorio crear una SVI específica para cada VLAN que se desee enrutar. Esto se logra ingresando el comando `interface vlan [vlan-id]` (el mismo que se usa para la SVI de administración en Capa 2).

---
### Escenario de switch de capa 3

En la figura, el switch de capa 3, D1, está conectado a dos hosts en diferentes VLAN. PC1 está en VLAN 10 y PC2 está en VLAN 20, como se muestra. El switch de capa 3 proporcionará servicios inter-VLAN routing a los dos hosts.

![](../CCNA2IMG/IMGM4/Sw%20de%20capa%203.png)

La tabla muestra las direcciones IP de cada VLAN.

### D1 VLAN IP Addresses

![](../CCNA2IMG/IMGM4/D1%20VLAN%20IP%20Add.png)

---
### Configuracion de switch de capa 3

Complete los siguientes pasos para configurar S1 con VLAN y trunking :

**Paso 1**. Crear las VLAN.

**Paso 2**. Crear las interfaces VLAN SVI.

**Paso 3**. Configurar puertos de acceso.

**Paso 4**. Habilitar IP routing.

---

**1. Crear las VLANs.**

Primero, cree las dos VLAN como se muestra en el ejemplo

```
D1(config)# vlan 10
D1(config-vlan)# name LAN10
D1(config-vlan)# vlan 20
D1(config-vlan)# name LAN20
D1(config-vlan)# exit
D1(config)#
```

---

**2. Crear las interfaces VLAN SVI.**

Configurar el SVI para VLANs 10 y 20 Las direcciones IP configuradas servirán como default gateways para los hosts de las VLAN respectivas. Observe que los mensajes informativos que muestran el protocolo de línea en ambos SVIs cambiaron a funcionales.

```
D1(config)# interface vlan 10
D1(config-if)# description Default Gateway SVI for 192.168.10.0/24
D1(config-if)# ip add 192.168.10.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
D1(config)# int vlan 20
D1(config-if)# description Default Gateway SVI for 192.168.20.0/24
D1(config-if)# ip add 192.168.20.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
*Sep 17 13:52:16.053: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
*Sep 17 13:52:16.160: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan20, changed state to up
```

---

**3. Configurar puertos de acceso.**

A continuación, configure los puertos de acceso que se conectan a los hosts y asígnelos a sus respectivas VLAN.

```
D1(config)# interface GigabitEthernet1/0/6
D1(config-if)# description Access port to PC1
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 10
D1(config-if)# exit
D1(config)#
D1(config)# interface GigabitEthernet1/0/18
D1(config-if)# description Access port to PC2
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 20
D1(config-if)# exit
```

---

**4. Habilitar IP routing.**

Por último, habilite el enrutamiento IPv4 con el comando de configuración **ip routing** global para permitir el intercambio de tráfico entre las VLAN 10 y 20. Este comando debe configurarse para habilitar el inter-VAN routing en un switch de capa 3 para IPv4.

```
D1(config)# ip routing
D1(config)#
```

---
### Verificación Inter-VLAN Routing del switch de capa 3

El Inter-VLAN Routing mediante un switch de capa 3 es más sencillo de configurar que el método router-on-a-stick. Una vez completada la configuración, la configuración se puede verificar probando la conectividad entre los hosts.

Desde un host, compruebe la conectividad con un host de otra VLAN mediante el **ping** comando. Es una buena idea verificar primero la configuración IP del host actual mediante el comando **ipconfig** Windows host El resultado confirma la dirección IPv4 y el default gateway de PC1.

![](../CCNA2IMG/IMGM4/ipconfig%201.png)

A continuación, verifique la conectividad con PC2 mediante el comando host de **ping** Windows, como se muestra en el ejemplo. El **ping** resultado confirma correctamente que el enrutamiento entre VLANs está funcionando.

![](../CCNA2IMG/IMGM4/ping%20192.168.20.10%201.png)

---
### Enrutamiento en un switch de capa 3

Si se quiere que otros dispositivos de Capa 3 puedan acceder a las VLAN, deben anunciarse mediante enrutamiento estático o dinámico. Para habilitar el enrutamiento en un switch de capa 3, se debe configurar un puerto enrutado.

Un puerto enrutado se crea en un switch de Capa 3 deshabilitando la función switchport de un switch de Capa 2 que está conectado a otro dispositivo de Capa 3. Específicamente, al configurar el comando de configuración de **no switchport** interfaz en un puerto de Capa 2, se convierte en una interfaz de Capa 3. A continuación, la interfaz se puede configurar con una configuración IPv4 para conectarse a un router u otro switch de capa 3.

---
### Escenario de enrutamiento en un switch de capa 3

En la figura, el switch de capa 3 D1 previamente configurado ahora está conectado a R1. R1 y D1 están ambos en un dominio de protocolo de enrutamiento Open Shortest Path First (OSPF). Supongamos que Inter-VLAN se ha implementado correctamente en D1. La interfaz G0/0/1 de R1 también ha sido configurada y habilitada. Además, R1 está utilizando OSPF para anunciar sus dos redes, 10.10.10.0/24 y 10.20.20.0/24.

La configuración de enrutamiento **Note:** OSPF se cubre en otro curso. En este módulo, se le darán comandos de configuración OSPF en todas las actividades y evaluaciones. No es necesario que comprenda la configuración para habilitar el enrutamiento OSPF en el switch de capa 3.

![](../CCNA2IMG/IMGM4/Escenario%20de%20enrutamiento%20en%20un%20switch%20de%20capa%203.png)

---
### Configuración de enrutamiento en un switch de capa 3

Complete los siguientes pasos para configurar D1 para enrutar con R1:

**Paso 1**. Configure el puerto enrutado.

**Paso 2**. Activar el routing.

**Paso 3**. Configurar el enrutamiento

**Paso 4**. Verificar enrutamiento.

**Paso 5**. Verificar la conectividad

---
**1. Configure el puerto enrutado.**

Configure G1/0/1 para que sea un puerto enrutado, asígnele una dirección IPv4 y habilítelo.

```
D1(config)# interface GigabitEthernet1/0/1
D1(config-if)# description routed Port Link to R1
D1(config-if)# no switchport
D1(config-if)# ip address 10.10.10.2 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
```

---

**2. Activar el routing.**

Asegúrese de que el enrutamiento IPv4 esté habilitado con el comando de configuración **ip routing** global.

```
D1(config)# ip routing
D1(config)#
```

---

**3. Configurar el enrutamiento.**

Configure el protocolo de enrutamiento OSPF para anunciar las redes VLAN 10 y VLAN 20, junto con la red que está conectada a R1. Observe el mensaje informándole de que se ha establecido una adyacencia con R1.

```
D1(config)# router ospf 10
D1(config-router)# network 192.168.10.0 0.0.0.255 area 0
D1(config-router)# network 192.168.20.0 0.0.0.255 area 0
D1(config-router)# network 10.10.10.0 0.0.0.3 area 0
D1(config-router)# ^Z
D1#
*Sep 17 13:52:51.163: %OSPF-5-ADJCHG: Process 10, Nbr 10.20.20.1 on GigabitEthernet1/0/1 from LOADING to FULL, Loading Done
D1#
```

---

**4. Verificar enrutamiento.**

Verifique la tabla de enrutamiento en D1. Observe que D1 ahora tiene una ruta a la red 10.20.20.0/24.

```
D1# show ip route | begin Gateway
Gateway of last resort is not set
      10.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
C 10.10.10.0/30 is directly connected, GigabitEthernet1/0/1
L 10.10.10.2/32 is directly connected, GigabitEthernet1/0/1
O 10.20.20.0/24 [110/2] via 10.10.10.1, 00:00:06, GigabitEthernet1/0/1
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.10.0/24 is directly connected, Vlan10
L 192.168.10.1/32 is directly connected, Vlan10
      192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.20.0/24 is directly connected, Vlan20
L 192.168.20.1/32 is directly connected, Vlan20
D1#
```

---

**5. Verificar la conectividad.**

En este momento, PC1 y PC2 pueden hacer ping al servidor conectado a R1.

```
c:Userspc1> ping 10.20.20.254
Pinging 10.20.20.254 with 32 bytes of data: 
Request timed out.
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Estadísticas de ping para 10.20.20.254:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss).
Approximate round trip times in milli-seconds: 
    Minimum = 1ms, Maximum = 2ms, Average = 1ms 
C:\Users\PC1>
! ==================================================
C:Userspc2> ping 10.20.20.254
Pinging 10.20.20.254 with 32 bytes of data: 
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Reply from 10.20.20.254: bytes=32 time<1ms TTL=127
Estadísticas de ping para 10.20.20.254:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss).
Approximate round trip times in milli-seconds: 
    Minimum = 1ms, Maximum = 2ms, Average = 1ms
C:\Users\PC2 > 
```

----

