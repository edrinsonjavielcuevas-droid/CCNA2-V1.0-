
---
### SERVIDOR CISCO IOS DHCPv4 

Un router con software Cisco IOS se puede configurar para que funcione como un servidor DHCPv4 independiente.

El servidor DHCPv4 integrado en Cisco IOS asigna y administra direcciones IPv4 a los clientes utilizando conjuntos de direcciones especificados dentro del propio router.

![](../CCNA2IMG/IMGM7/SERVIDOR%20CISCO%20IOS%20DHCPv4.png)

---
### Pasos para configurar un servidor DHCPv4 del IOS de Cisco

Utilice los siguientes pasos para configurar un servidor DHCPv4 del IOS de Cisco:

**Paso 1. Excluir direcciones IPv4**

El router que funciona como servidor de DHCPv4 asigna todas las direcciones IPv4 en un conjunto de direcciones DHCPv4, a menos que esté configurado para excluir direcciones específicas. Generalmente, algunas direcciones IPv4 de un conjunto se asignan a dispositivos de red que requieren asignaciones de direcciones estáticas. Por lo tanto, estas direcciones IPv4 no deben asignarse a otros dispositivos. La sintaxis del comando para excluir direcciones IPv4 es la siguiente:

```
Router(config)# ip dhcp excluded-address low-address [high-address]
```

Se puede excluir una única dirección o un rango de direcciones especificando la dirección más baja y la dirección más alta del rango. Las direcciones excluidas deben incluir las direcciones asignadas a los routers, a los servidores, a las impresoras y a los demás dispositivos que se configuraron o se configurarán manualmente. También puede introducir el comando varias veces.

---

**Paso 2. Defina un nombre de grupo DHCPv4**

La configuración de un servidor de DHCPv4 implica definir un conjunto de direcciones que se deben asignar.

Como se muestra en el ejemplo, el **ip dhcp pool** comando _pool-name_crea un conjunto con el nombre especificado y coloca al router en el modo de configuración de DHCPv4, que se identifica con el indicador Router(dhcp-config)#.

La sintaxis del comando para definir el grupo es la siguiente:

```
  Router(config)# ip dhcp pool pool-name    Router(dhcp-config)#
```

---

**Paso 3. Configure el grupo DHCPv4**

La tabla indica las tareas para finalizar la configuración del pool de DHCPv4.

El conjunto de direcciones y el router de gateway predeterminado deben estar configurados. Use la **network** instrucción para definir el rango de direcciones disponibles. Use el **default-router** comando para definir el router de gateway predeterminado. Normalmente, el gateway es la interfaz LAN del router más cercano a los dispositivos clientes. Se requiere un gateway, pero se pueden indicar hasta ocho direcciones si hay varios gateways.

Otros comandos del pool de DHCPv4 son optativos. Por ejemplo, la dirección IPv4 del servidor DNS que está disponible para un cliente DHCPv4 se configura mediante el comando **dns-server**. El comando **domain-name** se utiliza para definir el nombre de dominio. La duración del arrendamiento de DHCPv4 puede modificarse mediante el comando **lease**. El valor de arrendamiento predeterminado es un día. El comando **netbios-name-server** se utiliza para definir el servidor WINS con NetBIOS.

![](../CCNA2IMG/IMGM7/Paso%203.%20Configure%20el%20grupo%20DHCPv4.png)

**Nota:** Microsoft recomienda no implementar WINS, en su lugar configurar DNS para la resolución de nombres de Windows y retirar WINS.

---
### Ejemplo de configuración

La topología para el ejemplo de configuración se muestra en la figura.

![](../CCNA2IMG/IMGM7/EJMPLO%20DE%20CONFIG%20DHCPv4.png)

El ejemplo muestra la configuración para convertir a R1 en un servidor DHCPv4 para la LAN 192.168.10.0/24.

```
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9

R1(config)# ip dhcp excluded-address 192.168.10.254

R1(config)# ip dhcp pool LAN-POOL-1

R1(dhcp-config)# network 192.168.10.0 255.255.255.0

R1(dhcp-config)# default-router 192.168.10.1

R1(dhcp-config)# dns-server 192.168.11.5

R1(dhcp-config)# domain-name example.com

R1(dhcp-config)# end

R1#
```

---
### Comandos de verificación DHCPv4

Utilice los comandos de la tabla para verificar que el servidor DHCPv4 del IOS de Cisco esté funcionando.

![](../CCNA2IMG/IMGM7/Comandos%20de%20verificación%20DHCPv4.png)

---
### Desactive el servidor DHCPv4 del IOS de Cisco

El servicio DHCPv4 está habilitado de manera predeterminada. Para desabilitar el servicio, use el comando **no service dhcp** del modo de configuración global. Use el comando del modo **service dhcp** de configuración global para volver a habilitar el proceso del servidor DHCPv4, como se muestra en el ejemplo. Si los parámetros no se configuran, habilitar el servicio no tiene ningún efecto.

**Nota:** Si se borra los enlaces DHCP o se detiene y reinicia el servicio DHCP, se pueden asignar temporalmente direcciones IP duplicadas en la red.

```
R1(config)# no service dhcp

R1(config)# service dhcp

R1(config)#
```

---
### Retransmisión DHCPv4

En una red jerárquica compleja, los servidores empresariales suelen estar ubicados en una central. Estos servidores pueden proporcionar servicios DHCP, DNS, TFTP y FTP para la red. Generalmente, los clientes de red no se encuentran en la misma subred que esos servidores. Para ubicar los servidores y recibir servicios, los clientes con frecuencia utilizan mensajes de difusión.

En la figura, la PC1 intenta adquirir una dirección IPv4 de un servidor de DHCPv4 mediante un mensaje de difusión. En esta situación, el router R1 no está configurado como servidor de DHCPv4 y no reenvía el mensaje de difusión. Dado que el servidor de DHCPv4 está ubicado en una red diferente, la PC1 no puede recibir una dirección IP mediante DHCP. R1 debe configurarse para retransmitir mensajes DHCPv4 al servidor DHCPv4.

![](../CCNA2IMG/IMGM7/Retransmisión%20DHCPv4.png)

---

**ipconfig /release**

PC1 es una computadora con Windows. El administrador de red libera toda la información de direccionamiento IPv4 actual mediante el comando **ipconfig /release**. Observe que se libera la dirección IPv4 y ninguna dirección aparece.

```
C:\Users\Student> ipconfig /release
Configuración IP de Windows
Ethernet adapter Ethernet0:
   Sufijo de conexión específica DNS. :
   Default Gateway . . . . . . . . . :                              
```

---

**ipconfig /renew**

A continuación, el administrador de red intenta renovar la información de direccionamiento IPv4 con el comando **ipconfig /renew**. Este comando hace que la PC1 transmita por difusión un mensaje DHCPDISCOVER. En el resultado se muestra que la PC1 no puede ubicar el servidor de DHCPv4. Dado que los routers no reenvían mensajes de difusión, la solicitud no es correcta.

El administrador de red podría agregar servidores DHCPv4 en R1 para todas las subredes. Sin embargo, esto crearía costos adicionales y gastos administrativos.

```
C:\Users\Student> ipconfig /renew
Configuración IP de Windows
Error al renovar la interfaz Ethernet0: no se puede conectar al servidor DHCP. Expiró la solicitud.
```

---

**ip helper-address**

Una mejor solución es configurar R1 con el comando **ip helper-address** _address_ interface configuration. Esto hará que R1 retransmita transmisiones DHCPv4 al servidor DHCPv4. Como se muestra en el ejemplo, la interfaz en R1 que recibe la difusión desde PC1 está configurada para retransmitir la dirección DHCPv4 al servidor DHCPv4 en 192.168.11.6.

```
R1(config)# interface g0/0/0
R1(config-if)# ip helper-address 192.168.11.6
R1(config-if)# finalizar
R1#
```

---

**show ip interface**

Cuando se configura el R1 como agente de retransmisión DHCPv4, acepta solicitudes de difusión para el servicio DHCPv4 y, a continuación, reenvía dichas solicitudes en forma de unidifusión a la dirección IPv4 192.168.11.6. El administrador de red puede utilizar el comando **show ip interface** para verificar la configuración.

```
R1# show ip interface g0/0/0
GigabitEthernet0/0/0 is up, line protocol is up
  Internet address is 192.168.10.1/24
  Broadcast address is 255.255.255.255
  Address determined by setup command
  MTU is 1500 bytes
  Helper address is 192.168.11.6
(resultado omitido)
```

---

**ipconfig /all**

Como se muestra en la salida, PC1 ahora puede adquirir una dirección IPv4 del servidor DHCPv4 como se ha verificado con el **ipconfig /all** comando .

```
C:\Users\Student> ipconfig /all
Configuración IP de Windows
  
Ethernet adapter Ethernet0:
   Connection-specific DNS Suffix . : example.com
   IPv4 Address. . . . . . . . . . . : 192.168.10.10
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.10.1
```

---
### Otras transmisiones de servicio retransmitidas

DHCPv4 no es el único servicio que puede configurarse para que retransmita el router. De manera predeterminada, el **ip helper-address** comando reenvia los siguientes ocho servidcios UDP:

- Port 37: Tiempo
- Port 49: TACACS
- Port 53: Envenenamiento
- Port 67: DHCP/BOOTP server
- Port 68: DHCP/BOOTP client
- Port 69: TFTP
- Port 137: Servicio de nombre NetBIOS
- Port 138: Servicio de datagrama NetBIOS

----

