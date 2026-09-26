
---
### Proceso de decisión de reenvío de paquetes

Cuando una trama llega a la interfaz de un router, el dispositivo sigue un proceso lógico estructurado para decidir qué hacer con ella:

**Verificación de Capa 2:** El router examina la dirección MAC de destino de la trama entrante. Si coincide con la MAC de su propia interfaz (o es un broadcast/multicast que debe procesar), acepta la trama. Si la FCS (Secuencia de Verificación de Trama) no tiene errores, desencapsula el paquete IP.

**Búsqueda en Capa 3:** El router examina la dirección IP de destino del paquete y busca en su tabla de enrutamiento la mejor ruta (usando la regla de la coincidencia más larga).

**Decisión:**

Si encuentra una ruta, el paquete se envía a la interfaz de salida correspondiente.

Si no hay ruta específica ni una ruta estática predeterminada (_Gateway of Last Resort_), el router descarta el paquete y envía un mensaje ICMP de "Destino inalcanzable" al origen.

---
### Reenvío de paquetes

Una vez que el router sabe por qué interfaz de salida debe enviar el paquete, debe prepararlo para su viaje a través del siguiente medio físico:

**Encapsulamiento:** El router vuelve a encapsular el paquete IP en una nueva trama de Capa 2 apropiada para el enlace de salida (por ejemplo, Ethernet, PPP, HDLC).

**Resolución de direcciones (Siguiente salto):** Si la interfaz de salida es una red Ethernet, el router necesita conocer la dirección MAC del siguiente dispositivo (el próximo router o el host final).

Para IPv4, el router consulta su tabla ARP o envía una solicitud ARP.

Para IPv6, utiliza el protocolo de Descubrimiento de Vecinos (ND) mediante mensajes ICMPv6.

**Transmisión:** La nueva trama se envía por la interfaz física.

----
### Mecanismos de reenvío de paquetes

Históricamente, los routers Cisco han utilizado tres mecanismos diferentes para reenviar los paquetes internamente (desde la interfaz de entrada hasta la de salida):

**Conmutación de procesos (Process Switching):** Es el método más antiguo y lento. La CPU del router debe procesar individualmente cada paquete que llega. La CPU examina la tabla de enrutamiento, determina la salida, reescribe el encabezado MAC y lo envía. Consume muchos recursos.

**Conmutación rápida (Fast Switching):** Utiliza una memoria caché (caché de conmutación rápida). Cuando llega el primer paquete de un flujo, la CPU lo procesa (como en Process Switching) y guarda la decisión en la caché. Todos los paquetes siguientes dirigidos a ese mismo destino se reenvían instantáneamente usando la caché, sin interrumpir a la CPU.

**Cisco Express Forwarding (CEF):** Es el mecanismo más moderno, eficiente y rápido. Es el método de reenvío **predeterminado** en los routers Cisco actuales. CEF no espera a que llegue un paquete para construir una caché. En su lugar, preconstruye estructuras de datos en el hardware (ASIC) basadas en la tabla de enrutamiento y la tabla ARP:

**FIB (Base de Información de Reenvío):** Es una copia optimizada por hardware de la tabla de enrutamiento.

**Tabla de adyacencia:** Contiene la información de direcciones de Capa 2 (MAC) ya resuelta para todos los siguientes saltos.

_Resultado:_ Cuando el paquete llega, el router ya sabe exactamente a dónde enviarlo y con qué encabezado MAC, operando a la velocidad del cable.

---

