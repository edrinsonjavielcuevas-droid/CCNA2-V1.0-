
---
### Diferentes versiones de STP

El STP original (estándar **802.1D**) tardaba hasta 50 segundos en reaccionar si se caía un cable. En redes modernas, 50 segundos sin conexión es una eternidad. Por eso, el protocolo tuvo que evolucionar:

**STP (802.1D):** El original, lento y crea un solo árbol para toda la red.

**PVST+:** La mejora de Cisco. Corre una instancia de STP por cada VLAN, permitiendo balanceo de carga, pero sigue siendo lento.

**RSTP (802.1w):** _Rapid Spanning Tree Protocol_. El salto cuántico. Reduce el tiempo de convergencia de 50 segundos a unos pocos milisegundos.

**Rapid PVST+:** La versión de Cisco de RSTP. Rápido y con una instancia por cada VLAN. (Este es el que más te vas a encontrar en equipos modernos).

**MSTP (802.1s):** _Multiple Spanning Tree_. Agrupa varias VLANs en una sola instancia de árbol para ahorrar recursos de CPU en los switches.

---
### Conceptos de RSTP (Rapid STP)

**El objetivo principal:** La velocidad. RSTP no espera a que los temporizadores expiren (como los 15 segundos de escucha o aprendizaje de STP).

**Negociación activa:** Los switches con RSTP se comunican proactivamente. Si un enlace falla, los switches negocian inmediatamente entre ellos para abrir una ruta alternativa casi al instante.

**Compatibilidad:** RSTP es compatible hacia atrás. Si conectas un switch nuevo (RSTP) a uno viejo (STP 802.1D), el switch nuevo bajará su velocidad para hablar en los términos del viejo en ese puerto específico.

---
### Estados de puerto RSTP y roles de puerto

RSTP simplifica los estados y redefine los roles para actuar más rápido.

**Estados simplificados (Pasa de 5 a solo 3):**

1. **Descartar (Discarding):** Unifica los viejos estados de _Disabled_, _Blocking_ y _Listening_. No envía datos de usuario, solo procesa BPDUs.

2. **Aprendizaje (Learning):** Aprende direcciones MAC, pero aún no envía datos.

3. **Reenvío (Forwarding):** Totalmente operativo.

**Roles de puerto actualizados:**

Mantiene el **Root Port** y **Designated Port**.

Divide el puerto bloqueado en dos:

**Puerto Alternativo (Alternate Port):** Es el respaldo directo para llegar al _Root Bridge_. Si el puerto raíz falla, este se activa al instante.

**Puerto de Respaldo (Backup Port):** Es un respaldo para un _Designated Port_ en un mismo segmento de red (menos común, ocurre cuando usas hubs).

----
### PortFast y protección BPDU (Vital para Seguridad)

Esto es oro puro para evitar dolores de cabeza y proteger la red de ataques o errores de capa 2.

**PortFast:** Se configura **únicamente** en puertos conectados a dispositivos finales (PCs, servidores, impresoras). Hace que el puerto se salte los estados de bloqueo, escucha y aprendizaje, pasando directamente a **Reenvío (Forwarding)**. Un PC obtiene conexión en 1 segundo en lugar de esperar 30.

**BPDU Guard (Protección BPDU):** Es el guardaespaldas de PortFast. Si alguien (un atacante o un empleado despistado) conecta un switch no autorizado en la pared de una oficina (donde hay un PC), ese puerto recibirá una BPDU. _BPDU Guard_ detecta esa trama e **inhabilita inmediatamente (err-disable)** el puerto. Evita que un switch intruso cause un bucle o intente convertirse en el Root Bridge.

---
### Alternativas a STP

STP y RSTP son muy buenos previniendo bucles lógicos, pero tienen una desventaja: obligan a bloquear cables físicos, desperdiciando ancho de banda.

En centros de datos modernos, se buscan topologías activas-activas donde **todos** los cables funcionen al mismo tiempo sin crear bucles.

**Soluciones alternativas:** Protocolos como _Shortest Path Bridging (SPB)_ o tecnologías de agregación de enlaces de chasis múltiple (como EtherChannel o VSS en Cisco), donde dos switches físicos actúan como uno solo lógico, eliminando la necesidad de que STP bloquee los enlaces redundantes.

---



