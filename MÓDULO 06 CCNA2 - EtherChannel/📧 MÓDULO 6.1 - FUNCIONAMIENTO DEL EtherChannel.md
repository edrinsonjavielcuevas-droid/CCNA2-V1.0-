
---
### AÑADIDURA DE ENLACES

#### **El problema con los enlaces múltiples y STP**

En algunas topologías, un solo cable no da abasto y se necesita conectar varios enlaces entre dispositivos para aumentar el ancho de banda y tener redundancia.

El gran inconveniente es que el **Spanning Tree Protocol (STP)** (que viene activado de fábrica en los switches Cisco de capa 2) detecta estos cables adicionales como un bucle y, lógicamente, **los bloquea**.

#### **La solución: EtherChannel**

Para evitar que STP corte estas conexiones redundantes, se requiere una tecnología de "agregación de enlaces". La respuesta a esto es **EtherChannel**.

![](../CCNA2IMG/IMGM6/Solucion%20EtherChannel.png)

#### **¿Qué es y cuáles son sus beneficios?**

EtherChannel es una tecnología que agrupa múltiples cables físicos de Ethernet y los combina para formar **un único enlace lógico**. (STP ve un solo cable muy gordo, por lo que no lo bloquea).

**Ventajas clave:**

**Mayor ancho de banda:** Suma la velocidad de los enlaces físicos para acelerar la comunicación general entre switches.

**Tolerancia a fallos y redundancia:** Si un cable físico del grupo se rompe, el tráfico se redirige automáticamente por los demás sin que se caiga la red.

**Uso compartido de carga (Load Sharing):** Balancea el tráfico de datos entre los distintos cables agrupados. Funciona perfectamente entre switches, routers y servidores.

![](EtherChannel.png)

---
### **Ventajas de EtherChannel**

**Coherencia de configuración:** Permite aplicar configuraciones directamente en la interfaz del EtherChannel en lugar de modificar cada puerto individualmente.

**Aprovechamiento de recursos:** Utiliza los puertos de switch ya existentes, evitando la necesidad de invertir en conexiones más rápidas y costosas para ampliar el ancho de banda.

**Equilibrio de carga:** Distribuye el tráfico entre los enlaces físicos usando diferentes métodos según el hardware, tales como MAC de origen a destino o IP de origen a destino.

**Compatibilidad con STP:** Al agruparse en un único enlace lógico, STP lo trata como una sola conexión; si solo existe un grupo, todos sus enlaces físicos se mantienen activos.

**Resiliencia ante fallos:** La pérdida de un enlace físico dentro del canal no altera la topología de la red, por lo que **no se requiere un recálculo de STP** y el EtherChannel sigue funcionando (aunque con menor rendimiento).

---
### **Restricciones de implementación**

EtherChannel presenta ciertas restricciones y reglas clave que se deben cumplir para su correcto funcionamiento:

**No se pueden mezclar tipos de interfaz:** No es posible combinar diferentes tecnologías en un mismo EtherChannel; por ejemplo, no se pueden mezclar puertos Fast Ethernet y Gigabit Ethernet.

**Límite de puertos y ancho de banda:** Cada EtherChannel puede constar de hasta ocho puertos Ethernet configurados de manera compatible. Esto proporciona un ancho de banda full-duplex de hasta 800 Mbps para Fast EtherChannel o hasta 8 Gbps para Gigabit EtherChannel entre un switch y otro dispositivo.

**Capacidad según el modelo:** El switch Cisco Catalyst 2960 Layer 2 soporta actualmente hasta seis EtherChannels. Sin embargo, esto puede variar con el desarrollo de nuevas versiones de IOS y plataformas, permitiendo en algunos casos una mayor cantidad de puertos o de Gigabit EtherChannels.

**Coherencia en la configuración:** Los puertos individuales que forman parte del grupo deben estar configurados de forma coherente en ambos extremos del enlace. Si un lado usa enlaces troncales (trunks) en una VLAN nativa, el otro lado debe configurarse exactamente igual, y todos los puertos del enlace deben ser de capa 2.

**Interfaz de canal lógica:** Cada EtherChannel cuenta con una interfaz lógica de canal de puertos. Cualquier configuración aplicada a esta interfaz lógica afecta de manera automática a todas las interfaces físicas que se le han asignado.

![](Restricciones.png)

---
### PROTOCOLOS DE NEGOCIACIÓN AUTOMÁTICA

Los EtherChannels  se pueden formar por medio de una negociación con uno de dos protocolos. **Port Aggregation Protocol (PAgP) o Link Aggregation Control Protocol (LACP)**. Estos protocolos permiten que los puertos con características similares formen un canal mediante una negociación dinámica con los switches adyacentes.

**NOTA**: También es posible configurar un EtherChannel estático o incondiconal sin PAgP o LACP.

---
### **Funcionamiento PAgP**

**¿Qué es PAgP?:** Es un protocolo patentado por Cisco que ayuda a crear automáticamente enlaces EtherChannel al enviar paquetes PAgP entre puertos aptos para negociar y agrupar enlaces compatibles, agregándose luego al árbol de expansión como un único puerto.

**Funciones de administración:** PAgP envía paquetes cada 30 segundos para revisar la coherencia de la configuración, administrar los enlaces agregados y gestionar fallas entre switches.

**Regla obligatoria:** Todos los puertos deben tener la misma velocidad, configuración de dúplex e información de VLAN. Cualquier modificación posterior en un puerto afecta automáticamente a los demás del canal.

**Modos de PAgP:**

**On:** Obliga a la interfaz a formar un canal _sin_ usar PAgP (las interfaces no intercambian paquetes). Funciona solo si el otro extremo también está en modo **On**; si el otro usa PAgP para negociar, el canal no se forma ya que el modo On no negocia. Al no haber negociación, no hay control de compatibilidad en el otro extremo.

**PAgP deseable:** Coloca la interfaz en un estado de negociación **activa**, iniciando las conversaciones al enviar paquetes PAgP.

**PAgP automático:** Coloca la interfaz en un estado de negociación **pasiva**, respondiendo a los paquetes PAgP recibidos pero sin iniciar la negociación por sí misma.

**Compatibilidad de modos:** Los modos en ambos extremos deben ser compatibles. Si ambos extremos se configuran en modo _automático_, la negociación nunca inicia y el canal no se forma. Para deshabilitar los modos, se utiliza el comando **no**.

---
### EJEMPLO DE CONFIG DEL MODO PAgP

Considere los dos switches en la imagen. Si S1 y S2 establecen un EtherChannel usando PAgP depende de la config de modo en cada lado del canal.

![](PAgP.png)

La tabla muestra las diversas combinaciones de modos PAgP en S1 y S2 y el resultado del establecimiento de canales.

**PAgP Modes**

![](PAgP%20Modes.png)

---
### OPERACIÓN LACP

**¿Qué es LACP?:** Es un protocolo que forma parte de la especificación IEEE (originalmente definido como **802.3ad** y ahora en el estándar más moderno **802.1AX** para redes de área local y metropolitana) que permite agrupar varios puertos físicos para formar un único canal lógico.

**Propósito y Compatibilidad:** Permite que un switch negocie un grupo automático mediante el envío de paquetes LACP a otro switch. Realiza una función similar a PAgP, pero al ser un **estándar IEEE**, se puede utilizar para facilitar EtherChannels en entornos de varios proveedores (multivendor). En dispositivos Cisco se admiten ambos protocolos.

**Beneficios de negociación:** Ayuda a crear el enlace EtherChannel detectando la configuración de cada lado y asegurando que sean compatibles antes de habilitarlo.

**Modos de LACP:**

**On:** Obliga a la interfaz a proporcionar un canal _sin_ LACP; las interfaces configuradas en este modo no intercambian paquetes LACP.

**LACP activo:** Coloca el puerto en un estado de negociación **activa**, donde inicia las negociaciones con otros puertos enviando paquetes LACP.

**LACP pasivo:** Coloca el puerto en un estado de negociación **pasiva**, donde responde a los paquetes LACP recibidos pero no inicia la negociación.

**Compatibilidad de modos y enlaces de reserva:**

Los modos deben ser compatibles en ambos extremos para que se forme el enlace. El modo encendido (_On_) crea la configuración incondicionalmente sin negociación dinámica.

LACP permite tener hasta **ocho enlaces activos** y también **ocho enlaces de reserva**, los cuales se vuelven activos automáticamente si falla alguno de los enlaces activos actuales.

---
### EJEMPLO DE CONFIG DEL MODO LACP

Considere los dos switches de la siguiente imagen. Si S1 y S2 establecen un EtherChannel usando LACP depende de la config de modo en cada lado del canal.

![](LACP.png)

La tabla muestra las diversas combinaciones de modos LACP en S1 y S2 y el resultado resultante del establecimiento de canales.

![](TABLA%20LACP.png)

---



