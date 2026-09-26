
---
### Revisión de ataques a VLAN

Como una revisión rápida, un ataque de salto a una VLAN puede ser lanzado en una de 3 maneras:

La suplantación de mensajes DTP del host atacante hace que el switch entre en modo de enlace troncal. Desde aquí, el atacante puede enviar tráfico etiquetado con la VLAN de destino, y el conmutador luego entrega los paquetes al destino.

Introduciendo un switch dudoso y habilitando enlaces troncales. El atacante puede acceder todas las VLANs del switch victima desde el switch dudoso.

Otro tipo de ataque de salto a VLAN es el ataque doble etiqueta o doble encapsulado. Este ataque toma ventaja de la forma en la que opera el hardware en la mayoría de los switches.

---
### Pasos para mitigar ataques de salto

Use los siguiente Pasos para mitigar ataques de salto

**Paso 1** : Deshabilite las negociaciones de DTP (enlace automático) en puertos que no sean enlaces mediante el **switchport mode access** comando de configuración de la interfaz.

**Paso 2** : Deshabilite los puertos no utilizados y colóquelos en una VLAN no utilizada.

**Paso 3** : Active manualmente el enlace troncal en un puerto de enlace troncal utilizando el **switchport mode trunk** comando.

**Paso 4** : Deshabilite las negociaciones de DTP (enlace automático) en los puertos de enlace mediante el comando **switchport nonegotiate**.

**Paso 5** : Establezca la VLAN nativa en otra VLAN que no sea la VLAN 1 mediante el comando **switchport trunk native vlan** _vlan _number_.

por ejemplo, asumamos lo siguiente

- Los puertos FastEthernet 0/1 hasta fa0/16 son puertos de acceso activos

- Los puertos FastEthernet 0/17 hasta 0/20 no estan en uso

- FastEthernet ports 0/21 through 0/24 son puertos troncales.

Salto de VLAN puede ser mitigado implementando la siguiente configuración.

Salto de VLAN puede ser mitigado implementando la siguiente configuración.

```
S1(config)# interface range fa0/1 - 16

S1(config-if-range)# switchport mode access

S1(config-if-range)# exit

S1(config)#


S1(config)# interface range fa0/17 - 20

S1(config-if-range)# switchport mode access

S1(config-if-range)# switchport access vlan 1000

S1(config-if-range)# shutdown

S1(config-if-range)# exit

S1(config)#


S1(config)# interface range fa0/21 - 24

S1(config-if-range)# switchport mode trunk

S1(config-if-range)# switchport nonegotiate

S1(config-if-range)# switchport trunk native vlan 999

S1(config-if-range)# end

S1#
```

- Los puertos Fastethernet 0/1 al 0/16 son puertos de acceso y por lo tanto troncal se deshabilita poniéndolos explicitamente como puertos de acceso.

- Los puertos FastEthernet 0/17 al 0/20 son puertos que no están en uso y están deshabilitados y asignados a una VLAN que no se usa.

- Los puertos FastEthernet 0/21 al 0/24 son enlaces troncales y se configuran manualmente como troncales con DTP deshabilitado. La VLAN nativa también se cambia de la VLAN predeterminada 1 a la VLAN 999 que no se usa.

-----

