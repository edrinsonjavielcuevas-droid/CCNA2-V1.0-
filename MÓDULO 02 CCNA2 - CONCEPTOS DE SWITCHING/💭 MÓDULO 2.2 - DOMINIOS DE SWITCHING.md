
----
### DOMINIOS DE COLISIONES 

Un **dominio de colisión** es un segmento de red donde los dispositivos comparten el mismo ancho de banda y compiten por transmitir; si dos intentan enviar datos al mismo tiempo, sus señales chocan (colisión).

En redes antiguas con hubs, todos los equipos compartían un único dominio. Actualmente, los switches eliminan este problema al operar por defecto en **dúplex completo (full-duplex)**, lo que significa que el tráfico puede enviarse y recibirse simultáneamente sin chocar. Un dominio de colisión en un switch moderno solo se genera si uno de sus puertos se ve obligado a funcionar en **semidúplex (half-duplex)**, situación que ocurre típicamente cuando se conecta a un equipo obsoleto (como un hub heredado).

![](Dominio%20de%20colisiones.png)

---
### DOMINIOS DE DIFUSIÓN

**Conceptos Clave de Capa 2 y Capa 3**

**Definición Básica:** Una serie de switches interconectados forma un único dominio de difusión (broadcast domain) simple.

**Segmentación:** Solo los dispositivos de capa de red (como los **routers**) tienen la capacidad de dividir un dominio de difusión de Capa 2. Los routers también segmentan los dominios de colisión.

**Direccionamiento MAC:** Al enviar una difusión de Capa 2, la dirección MAC de destino de la trama se configura exclusivamente con números uno binarios (es decir, `FF:FF:FF:FF:FF:FF`).

**Dominio de difusión MAC:** Se refiere al conjunto de todos los dispositivos en una LAN que reciben las tramas de difusión enviadas por un host.

---

**Comportamiento del Switch ante Difusiones**

**Regla de Reenvío:** Cuando un switch recibe una trama de difusión, la replica y la reenvía por **todos** sus puertos, con la única excepción del puerto de entrada por donde recibió la trama original.

**Expansión del Dominio:** Conectar múltiples switches entre sí aumenta el tamaño del dominio de difusión. Por ejemplo, si el switch S1 se conecta al switch S2, una difusión recibida en S1 se propagará hacia S2, y S2 la enviará a todos sus dispositivos conectados. Todos los equipos en ambos switches procesarán la trama.

----

**Impacto en el Rendimiento de la Red**

**Utilidad:** Las difusiones son estrictamente necesarias para la localización inicial de otros dispositivos y servicios dentro de la red (por ejemplo, solicitudes ARP).

**Desventajas:** El tráfico de difusión consume ancho de banda y reduce la eficacia general. Si hay un exceso de difusiones combinado con una carga de tráfico intensa, se puede generar congestión y una caída en el rendimiento de la red.

---
### ALIVIO DE LA CONGESTIÓN EN LA RED

Los switches LAN son fundamentales para aliviar la congestión debido a que, por defecto, intentan establecer enlaces en **dúplex completo (full-duplex)**. Esto elimina los dominios de colisión, permite aprovechar el ancho de banda total por puerto y es un requisito obligatorio para velocidades de 1 Gbps o superiores.

Para gestionar el tráfico de manera eficiente y evitar cuellos de botella, los switches se apoyan en cuatro características de hardware clave:

**Velocidades de puertos rápidas:** Dependiendo de su ubicación en la red (acceso, distribución o núcleo), ofrecen puertos de 100 Mbps, 1 Gbps, 10 Gbps, e incluso hasta 100 Gbps, lo que acelera masivamente el flujo de datos.

**Cambio interno rápido:** Emplean buses internos de alta velocidad o arquitectura de memoria compartida para garantizar un procesamiento inmediato de las tramas.

**Búferes de trama grandes:** Tienen memorias amplias para almacenar tramas temporalmente. Esto es vital para no perder datos cuando el tráfico viaja desde un puerto de alta velocidad (ej. 1 Gbps) hacia un puerto más lento (ej. 100 Mbps).

**Alta densidad de puertos:** Al integrar muchos puertos en un solo equipo (por ejemplo, switches de 48 puertos), se reduce la necesidad de comprar múltiples dispositivos y se logra mantener la mayor parte del tráfico fluyendo localmente, lo que reduce la congestión general.

---







