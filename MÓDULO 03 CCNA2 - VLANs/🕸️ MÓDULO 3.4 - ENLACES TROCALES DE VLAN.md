
---
### Comandos de configuración troncal

Ahora que ha configurado y verificado VLAN, ha llegado el momento de configurar y verificar los troncos de VLAN. Un enlace troncal de VLAN es un enlace de capa 2 del modelo OSI entre dos switches que transporta el tráfico para todas las VLAN (a menos que se restrinja la lista de VLAN permitidas de manera manual o dinámica).

Para habilitar los vínculos troncal, configure los puertos de interconexión con el conjunto de comandos de configuración de interfaz que se muestran en la tabla.


![](../CCNA2IMG/IMGM3/config%20trocal.png)

---
### Ejemplo de configuración de troncal

En la figura 2, las VLAN 10, 20 y 30 admiten las computadoras de Cuerpo docente, Estudiante e Invitado (PC1, PC2 y PC3). El puerto F0/1 del switch S1 se configuró como puerto de enlace troncal y reenvía el tráfico para las VLAN 10, 20 y 30. La VLAN 99 se configuró como VLAN nativa.

![](../CCNA2IMG/IMGM3/Ej%20troncal.png)


El ejemplo muestra la configuración del puerto F0/1 en el conmutador S1 como puerto troncal. La VLAN nativa se cambia a VLAN 99 y la lista de VLAN permitidas se restringe a 10, 20, 30 y 99.

![](../CCNA2IMG/IMGM3/Config%20de%20trunk.png)

**Nota:** Esta configuración supone el uso de los switches Cisco Catalyst 2960 que utilizan de manera automática la encapsulación 802.1Q en los enlaces troncales. Es posible que otros switches requieran la configuración manual de la encapsulación. Siempre configure ambos extremos de un enlace troncal con la misma VLAN nativa. Si la configuración de enlace troncal 802.1Q no es la misma en ambos extremos, el software IOS de Cisco registra errores.

---
### Verifique la configuración de enlaces troncales.

La salida del switch muestra la configuración del puerto del switch F0/1 en el switch S1. La configuración se verifica con el **show interfaces** comando **switchport** _interface-ID_.

![](../CCNA2IMG/IMGM3/sh%20inter%20switchport.png)

En el área superior resaltada, se muestra que el modo administrativo del puerto F0/1 se estableció en **trunk**. El puerto está en modo de enlace troncal. En la siguiente área resaltada, se verifica que la VLAN nativa es la VLAN 99. Más abajo en el resultado, en el área inferior resaltada, se muestra que las VLAN 10,20,30 y 99 están habilitadas en el enlace troncal.

---
### Restablecimiento del enlace troncal al estado predeterminado

Use el **no switchport trunk allowed vlan** y el **no switchport trunk native vlan** comando para eliminar las VLAN permitidas y restablecer la VLAN nativa del enlace troncal. Cuando se restablece al estado predeterminado, el enlace troncal permite todas las VLAN y utiliza la VLAN 1 como VLAN nativa. El ejemplo muestra los comandos utilizados para restablecer todas las características de enlace troncal de una interfaz troncal a la configuración predeterminada.

![](../CCNA2IMG/IMGM3/Desactivar%20las%20trunk.png)

El comando **show interfaces fa0/1 switchport** revela que la troncal se ha reconfigurado a un estado predeterminado.

![](../CCNA2IMG/IMGM3/Verificar%20trunk.png)

La figura muestra el resultado de los comandos utilizados para eliminar la característica de enlace troncal del puerto F0/1 del switch S1. El **show interfaces f0/1 switchport** comando revela que la interfaz F0/1 ahora está en modo de acceso estático.

![](../CCNA2IMG/IMGM3/Trunk%201.png)

---

