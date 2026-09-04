
---
### Verificar EtherChannel

Como siempre, al configurar dispositivos en su red, debe verificar su configuración. Si hay problemas, también deberá poder solucionarlos. En este tema se proporcionan los comandos que se deben verificar, así como algunos problemas comunes de red EtherChannel y sus soluciones.

Los ejemplos de comandos de verificación utilizarán la topología mostrada en la figura.

![](../CCNA2IMG/IMGM6/Verificar%20EtherChannel.png)

El **show interfaces port-channel** commando muestra el estado general de la interfaz de canal de puertos. En la figura, la interfaz de canal de puertos 1 está activa.

```
S1# show interfaces port-channel 1
Port-channel1 is up, line protocol is up (connected)
  Hardware is EtherChannel, address is c07b.bcc4.a981 (bia c07b.bcc4.a981)
  MTU 1500 bytes, BW 200000 Kbit/sec, DLY 100 usec,
     reliability 255/255, txload 1/255, rxload 1/255
(resultado omitido)
```

Cuando se configuren varias interfaces de canal de puertos en el mismo dispositivo, puede usar el comando **show etherchannel summary** para mostrar una única línea de información por canal de puertos. En el resultado, el switch tiene un EtherChannel configurado; el grupo 1 utiliza el LACP.

El grupo de interfaces consta de las interfaces FastEthernet0/1 y FastEthernet0/2. El grupo es un EtherChannel de capa 2 y está en uso, según lo indican las letras SU junto al número de canal de puertos.

```
S1# show etherchannel summary
Flags: D - down P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3 S - Layer2
        U - in use N - not in use, no aggregation
        f - failed to allocate aggregator
        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port
        A - formed by Auto LAG
Number of channel-groups in use: 1
Number of aggregators: 1
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) LACP Fa0/1(P) Fa0/2(P)
```

Use el comando **show etherchannel port-channel** para mostrar información sobre la interfaz del canal de puertos específica, como se muestra en el resultado. En el ejemplo, la interfaz de canal de puertos 1 consta de dos interfaces físicas, FastEthernet0/1 y FastEthernet0/2. Esta usa LACP en modo activo. Está correctamente conectada a otro switch con una configuración compatible, razón por la cual se dice que el canal de puertos está en uso.

```
S1# show etherchannel port-channel
                Channel-group listing:
                —
Group: 1
----------
                Canales de puerto en el grupo:
                ---------------------------
Canal de puerto: Po1 (agregador principal)
------------
Age of the Port-channel = 0d:01h:02m:10s
Logical slot/port = 2/1 Number of ports = 2
HotStandBy port = null
Port state = Port-channel Ag-Inuse
Protocol = LACP
Port security = Disabled
Load share deferral = Disabled
Ports in the Port-channel:
Index Load Port EC state No of bits
------+------+------+------------------+-----------
  0 00 Fa0/1 Active 0
  0 00 Fa0/2 Active 0
Time since last port bundled: 0d:00h:09m:30s Fa0/2
```

En cualquier miembro de una interfaz física de un grupo EtherChannel, el **show interfaces etherchannel** comando puede proporcionar información sobre la función de la interfaz en el EtherChannel, como se muestra en el resultado. La interfaz FastEthernet0/1 forma parte del grupo EtherChannel 1. El protocolo para este EtherChannel es LACP.

```
S1# show interfaces f0/1 etherchannel
Port state = Up Mstr Assoc In-Bndl
Channel group = 1 Mode = Active Gcchange = -
Port-channel = Po1 GC = - Pseudo port-channel = Po1
Port index = 0 Load = 0x00 Protocol = LACP
Flags: S - Device is sending Slow LACPDUs F - Device is sending fast LACPDUs.
        A - Device is in active mode.        P - Device is in passive mode.
Local information:
                            LACP port Admin Oper Port       
Port Flags State Priority Key Number State
Fa0/1 SA bndl 32768 0x1 0x1 0x102 0x3D
Partner's information:
                  CP port Admin Oper Port Port
Port Flags Priority Dev ID Age key Key Number State
Fa0/1 SA 32768 c025.5cd7.ef00 12s 0x0 0x1 0x102 0x3Dof the port in the current state: 0d:00h:11m:51sllowed vlan 1,2,20
```

---
### Problemas comunes con las configuraciones de EtherChannel

Todas las interfaces dentro de un EtherChannel deben tener exactamente la misma configuración de velocidad, modo dúplex, VLAN nativa, VLANs permitidas en enlaces troncales y VLAN de acceso en puertos de acceso, lo cual reduce significativamente los problemas de red.

**Problemas comunes en EtherChannel:**

**VLANs no coincidentes:** Los puertos asignados en el EtherChannel no forman parte de la misma VLAN, no están configurados como enlace troncal, o tienen VLANs nativas diferentes.

**Troncalización inconsistente:** Se configuró el enlace troncal en algunos de los puertos del EtherChannel pero no en todos. _Recomendación:_ No se debe configurar el modo troncal en los puertos individuales; el modo troncal se debe configurar directamente en la interfaz del EtherChannel.

**Rango de VLANs diferente:** Si el rango permitido de VLANs no es el mismo en ambos extremos, los puertos no forman un EtherChannel, incluso si PAgP está configurado en modo _deseable_ o _auto_.

**Incompatibilidad de negociación:** Las opciones de negociación dinámica para PAgP y LACP no están configuradas de forma compatible en ambos extremos del EtherChannel.

**Nota importante (Confusión común con DTP):** Es fácil confundir PAgP o LACP con DTP, pero cumplen funciones distintas:

**PAgP y LACP:** Se utilizan para la agregación de enlaces (**EtherChannel**).

**DTP:** Se utiliza para automatizar la creación de enlaces troncales.

_Orden de configuración recomendado:_ Cuando se configura un enlace troncal en un EtherChannel, normalmente se configura primero el EtherChannel (usando PAgP o LACP) y después se configura DTP.

---
### Ejemplo de solucionar problemas de EtherChannel

En la figura, las interfaces F0/1 y F0/2 en los switches S1 y S2 se conectan con un EtherChannel. Sin embargo, el EtherChannel no está operativo.

![](../CCNA2IMG/IMGM6/S%20p%20d%20E.png)


**Paso 1. Ver la información de resumen de EtherChannel**

La salida del **show etherchannel summary** comando indica que el EtherChannel está caído.

```
S1# show etherchannel summary
Flags: D - down P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3 S - Layer2
        U - in use N - not in use, no aggregation
        f - failed to allocate aggregator
        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port
        A - formed by Auto LAG
Number of channel-groups in use: 1
Number of aggregators: 1
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 PO1 (SD) - Fa0/1 (D) Fa0/2 (D)
```


**Paso 2. Verifique la configuración de canalización de puerto**

En la **show run | begin interface port-channel** salida, una salida más detallada indica que existen modos PAgP incompatibles configurados en los switches S1 y S2.

```
S1# show run | begin interface port-channel
interface Port-channel1
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
!
interface FastEthernet0/1
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
 channel-group 1 mode on
!
interface FastEthernet0/2
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
 channel-group 1 mode on
! ======================================
S2# show run | begin interface port-channel
interface Port-channel1
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
!
interface FastEthernet0/1
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
 channel-group 1 mode desirable
!
interface FastEthernet0/2
 switchport trunk allowed vlan 1,2,20
 switchport mode trunk
 channel-group 1 mode desirable
```


**Paso 3. Corregir las configuraciones incorrectas**

Para corregir el problema, el modo PAgP en el EtherChannel se cambia a deseable.

**Nota:** EtherChannel y STP deben interoperar. Por este motivo, el orden en el que se introducen los comandos relacionados con EtherChannel es importante, y por ello se puede ver que se quitó el canal de puertos de interfaz1 y después se volvió a agregar con el **channel-group** comando, en vez de cambiarse directamente. Si se intenta cambiar la configuración directamente, los errores STP hacen que los puertos asociados entren en estado de bloqueo o errdisabled.

```
S1(config)# no interface port-channel 1
S1(config)# interface range fa0/1 - 2
S1(config-if-range)# channel-group 1 mode desirable
Creating a port-channel interface Port-channel 1
S1(config-if-range)# no shutdown
S1(config-if-range)# exit
S1(config)# interface range fa0/1 - 2
S1(config-if-range)# channel-group 1 mode desirable
S1(config-if-range)# no shutdown
S1(config-if-range)# interface port-channel 1
S1(config-if)# switchport mode trunk
S1(config-if)# finalizar
S1# 
```



**Paso 4. Verificar que EtherChannel este en funcionamiento**

El EtherChannel ahora está activo según lo verificado por la salida del **show etherchannel summary** comando.

```
S1# show etherchannel summary
Flags: D - down P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3 S - Layer2
        U - in use N - not in use, no aggregation
        f - failed to allocate aggregator
        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port
        A - formed by Auto LAG
Number of channel-groups in use: 1
Number of aggregators: 1
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) PAgP Fa0/1(P) Fa0/2(P)
```

----

