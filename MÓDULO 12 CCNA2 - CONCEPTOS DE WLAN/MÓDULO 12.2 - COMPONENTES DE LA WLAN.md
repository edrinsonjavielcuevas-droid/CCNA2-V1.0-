
---

## Componentes de la WLAN

Para construir y operar una Red de Área Local Inalámbrica (WLAN), se requieren componentes de hardware específicos que permitan la transmisión y recepción de señales de radiofrecuencia (RF).

### Componentes WLAN

Los componentes principales presentados son:

**Puntos Terminales (Endpoints):** Son los dispositivos de usuario final (laptops, teléfonos inteligentes, tablets) que inician la comunicación. Deben contar con un transmisor/receptor de radio incorporado para interactuar con la red.

**Puntos de Acceso (Access Points - APs):** Actúan como el puente entre el mundo inalámbrico y el cableado. Reciben la señal de radiofrecuencia de los dispositivos finales, la convierten en señales eléctricas estandarizadas y la inyectan en la red física Ethernet de la empresa.

**Controlador de LAN Inalámbrica (WLC):** En infraestructuras empresariales, este dispositivo centraliza la administración de la red. Configura de forma masiva todos los APs, controla el _roaming_ (la transición fluida de un usuario de un AP a otro mientras camina por el edificio) y aplica políticas de seguridad.

**Infraestructura de Switching y Routing:** Los switches proporcionan la conectividad de datos a los APs y al WLC, además de suministrarles energía eléctrica (generalmente a través de PoE - _Power over Ethernet_). Los routers se encargan de dirigir el tráfico de la WLAN hacia subredes externas o Internet.

**El Medio Inalámbrico:** A diferencia del cable de cobre o la fibra óptica, el medio físico en una WLAN es el espacio libre donde se propagan las ondas de radiofrecuencia (RF), operando de manera estándar en las bandas de 2.4 GHz y 5 GHz.

---

### NIC inalámbrica

Para que un dispositivo final (host) pueda comunicarse de forma inalámbrica, debe estar equipado con una Tarjeta de Interfaz de Red Inalámbrica (WNIC, por sus siglas en inglés).

**Función:** La WNIC actúa como el transceptor (transmisor y receptor) del dispositivo. Se encarga de tomar los datos digitales del equipo, codificarlos y modularlos en señales de radiofrecuencia (RF) para enviarlos por el aire, y viceversa.

**Formatos:** Las WNIC pueden venir integradas en la placa base (como en teléfonos inteligentes, tablets y la mayoría de las laptops modernas) o pueden ser externas y añadirse posteriormente mediante adaptadores USB o tarjetas PCIe en computadoras de escritorio.


---
### Router de hogar inalámbrico

Los routers domésticos (también conocidos como routers SOHO - _Small Office/Home Office_) son dispositivos multifunción diseñados para proporcionar conectividad completa en entornos pequeños. Internamente, combinan las funciones de tres dispositivos de red distintos en una sola caja:

**Punto de acceso inalámbrico (AP):** Emite la señal Wi-Fi y permite que los dispositivos inalámbricos se conecten a la red local.

**Switch Ethernet:** Proporciona puertos físicos (generalmente 4) para conectar dispositivos por cable (VLAN local).

**Router:** Enruta el tráfico entre la red local y la red externa (el Proveedor de Servicios de Internet, ISP), realizando funciones vitales como NAT (Traducción de Direcciones de Red) y actuando como servidor DHCP para asignar direcciones IP locales.

---
### Puntos de acceso inalámbrico (AP)

A diferencia de los entornos domésticos, las redes empresariales requieren una infraestructura dedicada. Los Puntos de Acceso (AP) empresariales no enrutan tráfico ni asignan IPs; su única función es proporcionar acceso inalámbrico a la red cableada.

**Puente de Capa 2:** Un AP actúa fundamentalmente como un puente de Capa 2. Recibe tramas inalámbricas (estándar 802.11), las desencapsula y las vuelve a encapsular como tramas Ethernet (estándar 802.3) para enviarlas a través del cableado estructurado hacia el switch de acceso.

**Escalabilidad:** Para cubrir un edificio grande, se instalan múltiples APs conectados a la red cableada, permitiendo a los usuarios moverse por las instalaciones (roaming) sin perder la conexión.

---
### Categorías AP

Los Puntos de Acceso empresariales se dividen principalmente en dos categorías según su método de gestión y despliegue:

**AP Autónomos (Autonomous APs):** También conocidos como _Thick APs_. Son dispositivos independientes. Cada AP debe configurarse, administrarse y actualizarse manualmente uno por uno. Son útiles para redes pequeñas, pero difíciles de escalar.

**AP Basados en Controlador (Controller-based APs):** También conocidos como _Lightweight APs (LAPs)_. Estos APs no tienen configuración local. Dependen de un dispositivo central llamado **Controlador de LAN Inalámbrica (WLC)** para recibir su configuración, políticas de seguridad y actualizaciones. Utilizan el protocolo CAPWAP para comunicarse con el WLC, lo que permite gestionar cientos de APs desde una única interfaz centralizada.

---
### Antenas inalámbricas

Las antenas son los componentes físicos que irradian y capturan las ondas de RF. Su forma y diseño determinan cómo se distribuye la señal en el espacio.

**Antenas Omnidireccionales:** Irradian la señal por igual en todas las direcciones (360 grados) formando un patrón similar a una rosquilla. Son el estándar en los APs de techo y routers de hogar para proporcionar cobertura general en interiores (ej. antenas de dipolo).

**Antenas Direccionales:** Concentran la señal de RF en una dirección específica, lo que aumenta significativamente el alcance y la fuerza de la señal en ese vector, sacrificando la cobertura en otras direcciones. Son ideales para enlazar dos edificios (enlaces punto a punto). Ejemplos: antenas Yagi, parabólicas y de parche.

**Antenas MIMO:** (Multiple Input, Multiple Output). Utilizan múltiples antenas físicas tanto en el transmisor como en el receptor para enviar y recibir múltiples flujos de datos simultáneamente, aumentando drásticamente el rendimiento y mitigando la interferencia.

---
