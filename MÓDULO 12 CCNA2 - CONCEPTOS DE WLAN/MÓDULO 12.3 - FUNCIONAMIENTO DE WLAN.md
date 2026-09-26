
---

## Funcionamiento de WLAN

Para que una red inalámbrica funcione correctamente, los dispositivos deben seguir un conjunto estricto de reglas (el estándar 802.11) para descubrir la red, conectarse a ella y evitar chocar entre sí al transmitir datos por el aire.

### Modos de topología inalámbrica

Las redes 802.11 pueden estructurarse en diferentes topologías lógicas:

**Modo Ad-hoc (IBSS - Independent Basic Service Set):** Es una conexión de igual a igual (peer-to-peer) donde dos dispositivos inalámbricos se conectan directamente entre sí sin necesidad de un Punto de Acceso o router central. Ejemplos: Compartir archivos por Bluetooth o Wi-Fi Direct.

**Modo de Infraestructura:** Es el estándar en redes empresariales y domésticas. Los clientes inalámbricos no se comunican directamente entre sí; todo el tráfico debe pasar a través de un Punto de Acceso (AP) central que los conecta a la red cableada.

**Tethering (Anclaje a red):** Una variación donde un dispositivo móvil (como un smartphone) actúa temporalmente como un router/AP, compartiendo su conexión de datos celulares (WWAN) creando un pequeño punto de acceso Wi-Fi (WLAN) para otros dispositivos.

----
### BSS y ESS

En el modo de infraestructura, la red se organiza en conjuntos de servicios:

**BSS (Basic Service Set - Conjunto de Servicios Básicos):** Consiste en un único Punto de Acceso (AP) y todos los clientes inalámbricos asociados a él. El área de cobertura de este AP se llama BSA (Basic Service Area). El BSS se identifica de forma única mediante el **BSSID**, que es simplemente la dirección MAC del AP.

**ESS (Extended Service Set - Conjunto de Servicios Extendidos):** Cuando el área a cubrir es muy grande (como un edificio), se conectan múltiples BSS a través de un sistema de distribución (generalmente la red Ethernet cableada de la empresa). Todos los APs en un ESS difunden el mismo nombre de red (SSID), lo que permite a los usuarios caminar por el edificio y cambiar de un AP a otro de forma transparente (roaming).

----
### Estructura del Frame

La trama inalámbrica (802.11) es más compleja que una trama Ethernet cableada (802.3) porque necesita manejar más información para cruzar el aire de forma segura. Mientras que Ethernet solo usa dos direcciones MAC (Origen y Destino), la trama 802.11 contiene hasta **cuatro campos de dirección MAC**:

1. MAC del dispositivo de destino final.
    
2. MAC del dispositivo de origen inicial.
    
3. MAC del dispositivo transmisor (el AP que está enviando la señal al aire).
    
4. MAC del dispositivo receptor (el router o AP siguiente en la infraestructura). Además, incluye un campo de control de trama (Frame Control) que indica el tipo de mensaje (datos, administración o control) y un campo de duración para reservar el aire.

---

### CSMA/CA (Acceso Múltiple por Detección de Portadora con Prevención de Colisiones)

A diferencia de las redes cableadas que usan CSMA/CD (Detección de colisiones), los dispositivos inalámbricos no pueden escuchar y transmitir al mismo tiempo. Por lo tanto, no pueden "detectar" una colisión. En su lugar, intentan **evitarlas** usando CSMA/CA:

1. **Escuchar:** El dispositivo escucha el canal para ver si está inactivo.

2. **Esperar (Backoff):** Si el canal está ocupado, el dispositivo espera un tiempo aleatorio.

3. **RTS/CTS (Opcional):** El cliente puede enviar un mensaje "Request to Send" (Solicitud para enviar). El AP responde con "Clear to Send" (Listo para enviar), reservando el canal para ese cliente y silenciando a los demás.

4. **Transmisión y ACK:** El cliente envía los datos. Como las colisiones aún pueden ocurrir por interferencias, el cliente debe recibir un acuse de recibo (**ACK**) del AP por cada trama enviada. Si no recibe el ACK, asume que hubo una colisión y retransmite.

---

### Asociación de AP de cliente inalámbrico

Para que un cliente envíe datos, debe pasar por un proceso estricto de tres estados con el AP:

1. **Descubrimiento:** El cliente busca redes disponibles (SSIDs).
    
2. **Autenticación:** El cliente y el AP negocian las credenciales de seguridad (abierto, WPA2, WPA3, etc.). En esta etapa, el cliente demuestra que tiene permiso para unirse.
    
3. **Asociación:** Una vez autenticado, el AP acepta al cliente, registra su dirección MAC en su tabla de clientes y le permite el paso de tráfico de datos.

---

### Modo de entrega pasiva y activa

El primer paso de la asociación (Descubrimiento) puede ocurrir de dos maneras:

 **Modo Pasivo (Passive Discovery):** El AP anuncia constantemente su presencia enviando tramas llamadas **Beacons** (Balizas) periódicamente (por defecto cada 100 milisegundos). El Beacon contiene el SSID, las velocidades soportadas y la configuración de seguridad. El cliente simplemente escucha el aire, lee los Beacons y decide a qué red conectarse.

**Modo Activo (Active Discovery):** El cliente inalámbrico toma la iniciativa. Envía una **Probe Request** (Solicitud de sondeo) al aire buscando un SSID específico (o buscando cualquier SSID si el campo está en blanco). Si el AP configurado con ese SSID escucha la solicitud, responde directamente al cliente con un **Probe Response** que contiene la información para conectarse.

----


