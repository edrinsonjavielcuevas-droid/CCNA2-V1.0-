
---
### CONFIG DE HOST CON IPv6 

**Requisito inicial:** Para utilizar la configuración automática de direcciones _stateless_ (SLAAC) o DHCPv6, primero se deben revisar las direcciones globales de unidifusión (GUA) y las direcciones _link-local_ (LLAs).

**Configuración en un router:** Una GUA IPv6 se configura de forma manual mediante el comando de configuración de interfaz `ipv6 address ipv6-address/prefix-length`.

**Configuración en un host:** Un equipo con Windows también se puede configurar de manera manual con una GUA IPv6.

![](GUA%20IPv6.png)

Introducir manualmente una GUA IPv6 puede llevar mucho tiempo y ser algo propenso a errores. Por lo tanto, la mayoría de los hosts de Windows están habilitados para adquirir dinámicamente una config GUA IPv6, como se muestra en la imagen.

![](WINDOWS%20GUA.png)

----
### **IPv6 Host Link-Local Address**

**Direccionamiento automático:** Cuando se selecciona el direccionamiento IPv6 automático, el host intenta obtener y configurar su información de red utilizando uno de los tres métodos definidos por los mensajes _Router Advertisement_ (RA) de ICMPv6 recibidos en la interfaz.

**Mensajes RA:** Un router IPv6 en el mismo vínculo envía mensajes RA para sugerir a los hosts cómo obtener su información de direccionamiento.

**Creación de la LLA:** El host crea automáticamente su dirección local del vínculo IPv6 (LLA) en cuanto se inicia y la interfaz Ethernet se activa, pudiendo visualizarse mediante el comando **ipconfig**.

**Ausencia de GUA:** Si la interfaz no crea una GUA IPv6, se debe a que el segmento de red no cuenta con un router que proporcione instrucciones de configuración para el host.

**Nota sobre ID de zona/ámbito:** Los sistemas operativos pueden mostrar la LLA anexada con un "%" y un número, conocido como ID de zona o ID de ámbito, utilizado para asociar la LLA con una interfaz específica.

**Nota sobre RFC:** DHCPv6 se define en la RFC 3315.

![](02_CCNA/CCNA2IMG/IMGM8/IPCONFIG.png)

----
### **IPv6 GUA Assignment**

**Diseño y simplificación:** IPv6 fue creado para simplificar la manera en que un host adquiere su configuración de red. De forma predeterminada, un router habilitado anuncia su información para permitir que el host cree o adquiera dicha configuración de forma dinámica.

**Asignación dinámica:** Las direcciones IPv6 GUA se pueden asignar dinámicamente haciendo uso de servicios _stateless_ (sin estado) y _stateful_ (con estado).

**Uso de mensajes RA:** Todos los métodos _stateless_ y _stateful_ emplean mensajes _Router Advertisement_ (RA) de ICMPv6 para indicarle al host cómo obtener su configuración. Aunque los sistemas operativos suelen seguir esta sugerencia, la decisión final recae en el propio host.

![](Asignación%20dinámica%20de%20GUA.png)

----
### **Tres flags de mensaje RA**

**Dependencia de configuración:** La forma en que un cliente adquiere una GUA IPv6 está determinada por la configuración interna del mensaje RA.

**Los tres flags principales en los mensajes de RA ICMPv6:**

**Flag A (mencionado en el texto como "Un flag"):** Indicador de configuración automática de direcciones que emplea SLAAC para crear una GUA IPv6.

**Flag O (Other):** Indica que se encuentra disponible información complementaria a través de un servidor DHCPv6 _stateless_.

**Flag M (Managed Address):** Indicador que señala la necesidad de utilizar un servidor DHCPv6 _stateful_ para obtener una GUA IPv6.

**Uso combinado:** Las distintas combinaciones entre los flags informan al host sobre las opciones de configuración dinámica disponibles en la red.

![](TRES%20FLAGs%20DE%20MENSAJE%20RA.png)

---

