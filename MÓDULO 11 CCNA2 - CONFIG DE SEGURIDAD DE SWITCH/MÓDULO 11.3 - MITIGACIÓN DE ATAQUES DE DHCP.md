
---

La mitigación de ataques de DHCP se logra principalmente mediante la función **DHCP Snooping** de los switches Cisco, la cual filtra los mensajes DHCP que no son de confianza y construye una tabla de enlaces (Binding Database) que se utiliza para validar protocolos adicionales de seguridad.

### Revisión de ataques DHCP

Para entender la mitigación, es crucial recordar los dos ataques principales a la infraestructura DHCP:

**Agotamiento de DHCP (DHCP Starvation):** Un atacante inunda la red con solicitudes DHCP de descubrimiento (DHCPDISCOVER) falsificando direcciones MAC, agotando el pool de direcciones IP del servidor legítimo para causar una denegación de servicio (DoS).

**Suplantación de DHCP (DHCP Spoofing):** Un atacante introduce un servidor DHCP falso (Rogue DHCP) en la red. Este responde a los clientes antes que el servidor legítimo, asignándoles direcciones IP y un Gateway predeterminado malicioso para interceptar el tráfico (Man-in-the-Middle).

### Indagación de DHCP (DHCP Snooping)

DHCP Snooping clasifica las interfaces del switch en dos categorías para bloquear servidores falsos:

**Puertos de confianza (Trusted Ports):** Interfaces conectadas a servidores DHCP legítimos o enlaces troncales hacia otros switches confiables. Se permite que pasen mensajes de servidor (DHCPOFFER, DHCPACK).

**Puertos no confiables (Untrusted Ports):** Interfaces conectadas a los usuarios finales. Se bloquean todos los mensajes de servidor entrantes; solo se permiten mensajes de cliente (DHCPDISCOVER, DHCPREQUEST).

**Base de datos de enlaces (Binding Database):** El switch registra la dirección MAC, IP asignada, tiempo de concesión, tipo de enlace, número de VLAN y el identificador de la interfaz de cada cliente en un puerto no confiable.

---

### Pasos para implementar DHCP Snooping

La implementación requiere una configuración global y luego configuraciones específicas por interfaz y VLAN.

1. **Habilitar DHCP Snooping globalmente:**

```
Switch(config)# ip dhcp snooping
```

2. **Configurar los puertos de confianza:** Ir a la interfaz que conecta al servidor DHCP (o al router) y configurarla como confiable (por defecto, todas son no confiables).

```
Switch(config)# interface GigabitEthernet0/1

Switch(config-if)# ip dhcp snooping trust
```

3. **Limitar la tasa de mensajes DHCP en puertos no confiables (Mitigación de agotamiento):** Ir a los puertos de los usuarios y limitar cuántos mensajes DHCP por segundo pueden enviar para evitar el ataque de _Starvation_.

```
Switch(config)# interface range FastEthernet0/1 - 24

Switch(config-if-range)# ip dhcp snooping limit rate 6
```

4. **Habilitar DHCP Snooping para VLANs específicas:** La función debe activarse para las VLANs donde residen los usuarios.

```
Switch(config)# ip dhcp snooping vlan 10,20
```

---
### Un ejemplo de configuración de detección de DHCP

Aquí tienes la configuración completa unificada para un switch que tiene el servidor en `Gi0/1`, usuarios en `Fa0/1-24` y la VLAN 10 operando:

```
! Habilitar globalmente
Switch(config)# ip dhcp snooping

! Habilitar para la VLAN 10
Switch(config)# ip dhcp snooping vlan 10

! Configurar el puerto hacia el servidor DHCP como confiable
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust

! Configurar los puertos de acceso de usuarios y limitar la tasa de paquetes
Switch(config)# interface range FastEthernet0/1 - 24
Switch(config-if-range)# ip dhcp snooping limit rate 10
Switch(config-if-range)# exit

! Verificar la configuración y la base de datos de enlaces
Switch# show ip dhcp snooping
Switch# show ip dhcp snooping binding
```

----

