
---

La mitigación de ataques ARP (como el envenenamiento de caché o _ARP Spoofing_) se realiza mediante la **Inspección Dinámica de ARP (DAI)**. Esta característica de seguridad es una extensión natural de la seguridad de Capa 2 y depende directamente de DHCP Snooping, ya que utiliza su base de datos de enlaces (Binding Database) para distinguir a los usuarios legítimos de los atacantes.

### Inspección dinámica de ARP

En un ataque de suplantación de ARP, un ciberdelincuente envía respuestas ARP falsificadas (afirmando que su MAC corresponde a la IP del Gateway, por ejemplo) para interceptar el tráfico de red. La Inspección Dinámica de ARP previene esto mediante las siguientes acciones:

Intercepta todas las solicitudes y respuestas ARP que ingresan por puertos no confiables (_untrusted_).

Verifica cada paquete interceptado comprobando si existe un enlace válido de IP a MAC en la tabla de DHCP Snooping.

Descarta y registra (hace un log) cualquier paquete ARP no válido, impidiendo que el atacante envenene las cachés ARP de otros dispositivos.

Si hay dispositivos con IP estáticas (que no usan DHCP), DAI permite utilizar ACLs de ARP estáticas para validarlos.

----
### Pautas de implementación DAI

Para implementar DAI correctamente, debes seguir una secuencia lógica en el switch:

**Habilitar DHCP Snooping** globalmente y en las VLAN correspondientes (DAI no funciona dinámicamente sin la tabla de enlaces de DHCP).

**Habilitar DAI en las VLANs** seleccionadas (generalmente las de acceso de usuarios).

**Configurar puertos de confianza (Trusted Ports):** Por defecto, DAI considera todos los puertos como no confiables (bloqueando respuestas ARP). Debes configurar explícitamente como confiables los puertos que conectan con otros switches, el router o el firewall.

_(Opcional pero recomendado)_ Configurar validaciones adicionales para que el switch revise más a fondo el contenido del paquete ARP (MAC de origen, MAC de destino y direcciones IP).

---

### Ejemplo de configuración DAI

A continuación, tienes la sintaxis y los comandos necesarios para activar DAI, asumiendo que DHCP Snooping ya está activo, los usuarios están en la VLAN 10 y el router principal está conectado en `Gi0/1`.

```
! 1. Habilitar la inspección ARP en la VLAN objetivo

Switch(config)# ip arp inspection vlan 10

! 2. Configurar la interfaz del router/troncal como confiable

Switch(config)# interface GigabitEthernet0/1

Switch(config-if)# ip arp inspection trust

Switch(config-if)# exit

! 3. (Opcional) Habilitar verificaciones avanzadas del contenido del paquete ARP

Switch(config)# ip arp inspection validate src-mac dst-mac ip
```

**Comandos de verificación clave para tus notas:**

**Ver el estado general de DAI:*

```
Switch# show ip arp inspection vlan 10
```

**Comprobar qué interfaces están configuradas como confiables o no confiables:**

 ```
 Switch# show ip arp inspection interfaces
 ```

---
### Ejemplo de configuración DAI

En la topologia anterior S1 está conectado a dos usuarios en la VLAN 10. DAI será configurado para mitigar ataques ARP spoofing and ARP poisonig.

Como se muestra en el ejemplo, la inspección DHCP está habilitada porque DAI requiere que funcione la tabla de enlace de inspección DHCP. Siguiente,la detección de DHCP y la inspección de ARP están habilitados para la computadora en la VLAN 10. El puerto de enlace ascendente al router es confiable y, por lo tanto, está configurado como confiable para la inspección DHP y ARP.

```
S1(config)# ip dhcp snooping

S1(config)# ip dhcp snooping vlan 10

S1(config)# ip arp inspection vlan 10

S1(config)# interface fa0/24

S1(config-if)# ip dhcp snooping trust

S1(config-if)# ip arp inspection trust
```

DAI se puede configurar para revisar si hay direcciones MAC e IP de destino o de origen:

- **MAC de destino :** comprueba la dirección MAC de destino en el encabezado de Ethernet con la dirección MAC de destino en el cuerpo ARP.

- **MAC de origen :** comprueba la dirección MAC de origen en el encabezado de Ethernet con la dirección MAC del remitente en el cuerpo ARP.

- **Dirección IP :** comprueba el cuerpo ARP en busca de direcciones IP no válidas e inesperadas, incluidas las direcciones 0.0.0.0, 255.255.255.255 y todas las direcciones de multidifusión IP.

El **ip arp inspection validate {[src-mac] [dst-mac] [ip]}**comando de configuración global se utiliza para configurar DAI para descartar paquetes ARP cuando las direcciones IP no son válidas. Se puede usar cuando las direcciones MAC en el cuerpo de los paquetes ARP no coinciden con las direcciones que se especifican en el encabezado Ethernet. Note como en el siguiente ejemplo solo un comando puede ser configurado. Por lo tanto, al ingresar varios comandos **ip arp inspection validate** se sobrescribe el comando anterior. Para incluir mas de un método de validación, ingreselos en la misma línea de comando como se muestra y verifica en la siguiente salida.

```
S1(config)# ip arp inspection validate ?

dst-mac  Validate destination MAC address  ip       Validate IP addresses  src-mac  Validate source MAC address

S1(config)# ip arp inspection validate src-mac

S1(config)# ip arp inspection validate dst-mac 

S1(config)# ip arp inspection validate ip

S1(config)# do show run | include validateip arp inspection validate ip

S1(config)# ip arp inspection validate src-mac dst-mac ip

S1(config)# do show run | include validateip arp inspection validate src-mac dst-mac ip

S1(config)#
```

----


