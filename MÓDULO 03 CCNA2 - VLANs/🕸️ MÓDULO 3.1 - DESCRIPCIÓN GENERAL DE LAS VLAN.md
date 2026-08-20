
---
### DEFINICIONES DE VLAN

Las VLAN (Redes de Área Local Virtuales) simplifican la administración de la red al proporcionar segmentación y flexibilidad organizativa. Su característica principal es que agrupan los dispositivos basándose en **conexiones lógicas** en lugar de conexiones físicas, permitiendo que los equipos se comuniquen exactamente como si compartieran el mismo cable.

Esta independencia de la infraestructura física significa que los usuarios de distintos departamentos (como TI, Recursos Humanos o Ventas) pueden pertenecer a su propia red aislada, sin importar a qué switch físico estén conectados o en qué piso del edificio se encuentren.

Como se ilustra en el siguiente diagrama, la separación lógica permite mantener los departamentos segmentados en sus respectivas VLAN (2, 3 y 4) a través de múltiples pisos y switches físicos:

![](../CCNA2IMG/IMGM3/VLAN%20DIVIDIDAS.png)

### Complemento: Comportamiento del Tráfico y Reglas de VLAN

**VLAN = Dominio de difusión:** Cada VLAN crea su propio dominio de difusión lógico. Esto significa que todo el tráfico (unidifusión, multidifusión y difusión, como las solicitudes ARP) queda estrictamente confinado a los equipos de esa VLAN, lo que evita que las difusiones inunden toda la red física y mejora drásticamente el rendimiento.

**El problema de usar solo subredes IP:** Se pueden configurar varias subredes IP en un switch sin crear VLANs, pero todos esos dispositivos seguirían compartiendo el mismo dominio de difusión de Capa 2. Las VLANs son necesarias para cortar de raíz esa propagación.

**Requisito de Enrutamiento (Capa 3):** Como el tráfico está aislado, los dispositivos de una VLAN no pueden comunicarse directamente con los de otra VLAN. Para que los paquetes salgan hacia otra VLAN, es obligatorio usar un dispositivo que admita _routing_ (como un router o un switch multicapa).

**Seguridad y Regla de Puertos:** Al separar lógicamente los grupos, los administradores pueden aplicar políticas de seguridad específicas a cada uno. La regla de diseño estándar dicta que **cada puerto del switch solo puede asignarse a una única VLAN** (las únicas excepciones a esta regla ocurren cuando el puerto se conecta a un teléfono IP o forma un enlace troncal con otro switch).

----
### Diseño de VLAN y Direccionamiento Jerárquico

**Relación Directa (1:1):** En una red conmutada, cada VLAN debe corresponder lógicamente a una red (o subred) IP específica e independiente.

**Diseño Jerárquico:** Debido a esta relación, implementar VLANs exige planificar un esquema de direccionamiento IP que sea ordenado y jerárquico.

**Asignación de Bloques Contiguos:** El direccionamiento jerárquico consiste en reservar bloques de direcciones IP contiguas para asignarlos exclusivamente a los dispositivos de un área específica o VLAN.

**Ventaja Administrativa:** Distribuir los números de red IP de esta manera ordenada permite que toda la infraestructura se visualice y administre de manera lógica como un conjunto unificado.

![](topologias.png)

### Ventajas de un diseño de red con VLAN
  
| **Ventaja**                                               | **Descripción**                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Dominios de difusión más pequeños**                     | • Dividir una red en VLAN reduce el número de dispositivos en el broadcast domain.<br><br>• En la figura, hay seis computadoras en la red, pero solo tres dominios de difusión (es decir, Facultad, Estudiante e Invitado).                                                                                              |
| **Seguridad mejorada**                                    | • Sólo los usuarios de la misma VLAN pueden comunicarse juntos.<br><br>• En la figura, el tráfico de red de profesores en la VLAN 10 es completamente separados y protegidos de los usuarios en otras VLAN.                                                                                                              |
| **Mejora la eficiencia del departamento de IT.**          | • Las VLAN simplifican la administración de la red porque los usuarios con una red similar se pueden configurar en la misma VLAN.<br>  <br>• Las VLAN se pueden nombrar para facilitar su identificación.<br><br>• En la figura, VLAN 10 fue nombrado «Facultad», VLAN 20 «Estudiante», y VLAN 30 «Invitado.».           |
| **Reducción de costos**                                   | Las VLAN reducen la necesidad de realizar costosas actualizaciones de red y utilizan el ancho de banda existente y enlaces ascendentes de manera más eficiente, lo que resulta en costos Ahorro.                                                                                                                         |
| **Mejor rendimiento**                                     | Los dominios de difusión más pequeños reducen el tráfico innecesario en la red y mejorar el rendimiento.                                                                                                                                                                                                                 |
| **Administración más simple de proyectos y aplicaciones** | • Las VLAN agregan usuarios y dispositivos de red para admitir empresas o necesidades geográficas.<br><br>• Tener funciones separadas hace que administrar un proyecto o trabajar con un aplicación especializada más fácil; un ejemplo de tal aplicación es una plataforma de desarrollo de e-learning para profesores. |

---

### TIPOS DE VLAN

En las redes modernas, las VLAN se implementan y definen según las clases de tráfico que manejan o la función específica que cumplen en la infraestructura.

**VLAN predeterminada:** En los switches Cisco, esta es siempre la VLAN 1. De manera predeterminada, todos los puertos del switch están asignados a ella. Además, funciona inicialmente como la VLAN nativa y la VLAN de administración. Es fundamental recordar que la VLAN 1 no se puede eliminar ni renombrar.

**VLAN de datos:** Están configuradas específicamente para separar y agrupar el tráfico generado por los usuarios o dispositivos. Por reglas de diseño, en una VLAN de datos no se debe permitir tráfico de administración ni de voz.

**VLAN nativa:** Se emplea en los enlaces troncales 802.1Q (trunk ports) para transportar el tráfico que viaja sin etiqueta (untagged). Aunque en Cisco la VLAN nativa predeterminada es la VLAN 1, la mejor práctica de seguridad es asignar una VLAN fija y sin utilizar exclusivamente para este propósito.

**VLAN de administración:** Es una VLAN dedicada estrictamente al tráfico de control y gestión del switch, soportando protocolos como SSH, Telnet, HTTPS, HTTP y SNMP. En un switch de Capa 2, la VLAN 1 asume este rol por defecto hasta que se configure una distinta.

**VLAN de voz:** Las comunicaciones de Voz sobre IP (VoIP) exigen una VLAN separada y dedicada para garantizar la calidad de la llamada. Para que el tráfico VoIP funcione, esta VLAN requiere un ancho de banda garantizado, prioridad de transmisión sobre otros tráficos, capacidad para ser enrutada en áreas congestionadas y una latencia (demora) estrictamente inferior a 150 ms.

A continuación, se ilustra la topología lógica de una VLAN de voz (VLAN 150) operando de manera independiente a la VLAN de datos del estudiante (VLAN 20):

![700](topologias%20s.png)

----

