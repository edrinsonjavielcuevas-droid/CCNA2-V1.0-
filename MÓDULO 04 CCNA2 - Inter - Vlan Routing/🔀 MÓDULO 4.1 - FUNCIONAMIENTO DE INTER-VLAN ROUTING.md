
---

### ¿Qué es el enrutamiento Inter-VLAN (Inter-VLAN Routing)?

**El problema:** Las VLAN aíslan lógicamente las redes en Capa 2. Por defecto, los equipos que pertenecen a diferentes VLAN no pueden comunicarse entre sí.

**La solución (Definición):** El enrutamiento Inter-VLAN es el proceso de reenviar el tráfico de red de una VLAN a otra. Para que esto sea posible, es estrictamente necesario utilizar un **router** o un **switch de Capa 3**.

**Opciones de implementación:** Existen tres métodos principales para lograrlo:

**Inter-VLAN Routing heredado:** Es la solución más antigua y tradicional. No escala bien, por lo que está en desuso.

**Router-on-a-stick:** Es una solución aceptable y muy utilizada para redes de tamaño pequeño o mediano.

**Switch de Capa 3 con interfaces virtuales (SVI):** Es la solución más rápida y escalable, diseñada para infraestructuras de organizaciones medianas y grandes.

---
### Inter-VLAN Routing heredado

La primera solución de inter-VLAN routing se basó en el uso de un router con múltiples interfaces Ethernet. Cada interfaz del router estaba conectada a un puerto del switch en diferentes VLAN. Las interfaces del router sirven como default gateways para los hosts locales en la subred de la VLAN.

Por ejemplo, consulte la topología donde R1 tiene dos interfaces conectadas al switch S1.

![](../CCNA2IMG/IMGM4/Inter%20vlan%20routing%20heredado.png)

Observe que en el ejemplo la tabla de direcciones MAC de S1 se completa de la siguiente manera:

- El puerto Fa0/1 está asignado a la VLAN 10 y está conectado a la interfaz R1 G0/0/0.
- El puerto Fa0/11 está asignado a la VLAN 10 y está conectado a la PC1.
- El puerto Fa0/12 está asignado a la VLAN 20 y está conectado a la interfaz R1 G0/0/1.
- El puerto Fa0/24 está asignado a la VLAN 20 y está conectado a la PC2.

### Tabla de direcciones MAC para S1

![](../CCNA2IMG/IMGM4/Tabla%20de%20direcciones%20MAC%20para%20S1.png)

Cuando PC1 envía un paquete a PC2 en otra red, lo reenvía a su puerta de enlace predeterminada 192.168.10.1. R1 recibe el paquete en su interfaz G0/0/0 y examina la dirección de destino del paquete. R1 luego enruta el paquete hacia fuera de su interfaz G0/0/1 al puerto F0/12 en la VLAN 20 en S1. Finalmente, S1 reenvía la trama a PC2.

Inter-VLAN routing heredado, usa las interfaces fisicas funciona, pero tiene limitaciones significantes. No es razonablemente escalable porque los routers tienen un número limitado de interfaces físicas. Requerir una interfaz física del router por VLAN agota rápidamente la capacidad de la interfaz física del router

En nuestro ejemplo, R1 requería dos interfaces Ethernet separadas para enrutar entre la VLAN 10 y la VLAN 20. ¿Qué ocurre si hubiera seis (o más) VLAN para interconectar? Se necesitaría una interfaz separada para cada VLAN. Obviamente, esta solución no es escalable.

**Nota:** Este método de inter-VLAN routing ya no se implementa en redes de switches y se incluye únicamente con fines explicativos.

---

### Enrutamiento Inter-VLAN: Router-on-a-Stick

**Concepto principal:** Supera las limitaciones del método heredado utilizando **una única interfaz física** en el router para enrutar el tráfico entre múltiples VLANs. Esta interfaz se conecta a un puerto troncal (802.1Q) del switch de Capa 2.

**Subinterfaces lógicas:** La magia ocurre al dividir la interfaz física en **subinterfaces virtuales** basadas en software. Cada subinterfaz se configura de forma independiente con su propia dirección IP y se asocia a una VLAN específica (y a su subred correspondiente).

**Flujo del tráfico:**

1.El tráfico etiquetado entra al router y se asocia a su subinterfaz de origen.

2.El router toma la decisión de enrutamiento analizando la dirección IP de destino.

3.El router reenvía internamente la trama a la subinterfaz de salida, le coloca la nueva etiqueta de la VLAN de destino y la envía de vuelta al switch por el mismo enlace físico.

4.El switch recibe la trama, le quita la etiqueta y la entrega al puerto de acceso del dispositivo de destino.

**Límite de escalabilidad:** Aunque es excelente para redes pequeñas y medianas, este método tiene un límite estricto: **no escala más allá de 50 VLANs**.

---
### Inter-VLAN Routing en un switch de capa 3

El método moderno para realizar inter-VLAN routing es utilizar switches de capa 3 e interfaces virtuales del switch (SVI). Una SVI es una interfaz virtual configurada en un switch multicapa, como se muestra en la figura.

**Nota:** Un switch de capa 3 también se denomina switch multicapa ya que funciona en la capa 2 y la capa 3. Sin embargo, en este curso usamos el término switch de capa 3.

![](../CCNA2IMG/IMGM4/Inter-VLAN%20Routing%20en%20un%20switch%20de%20capa%203.png)

Los SVIs entre VLAN se crean de la misma manera que se configura la interfaz de VLAN de administración. El SVI se crea para una VLAN que existe en el switch. Aunque es virtual, el SVI realiza las mismas funciones para la VLAN que lo haría una interfaz de router. Específicamente, proporciona el procesamiento de Capa 3 para los paquetes que se envían hacia o desde todos los puertos de switch asociados con esa VLAN.

A continuación se presentan las ventajas del uso de switches de capa 3 para inter-VLAN routing:

Es mucho más veloz que router-on-a-stick, porque todo el switching y el routing se realizan por hardware.

El routing no requiere enlaces externos del switch al router.  

No se limitan a un enlace porque los EtherChannels de Capa 2 se pueden utilizar como enlaces troncal entre los switches para aumentar el ancho de banda.

La latencia es mucho más baja, dado que los datos no necesitan salir del switch para ser enrutados a una red diferente.  

Se implementan con mayor frecuencia en una LAN de campus que en routers.

La única desventaja es que los switches de capa 3 son más caros.

----

