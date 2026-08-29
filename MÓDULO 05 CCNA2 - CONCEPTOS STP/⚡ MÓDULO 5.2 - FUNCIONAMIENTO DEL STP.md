
---
### Pasos para una topología sin bucles

Para evitar que las tramas Ethernet giren infinitamente y colapsen la red, STP construye un "árbol" lógico donde corta los enlaces redundantes que causan bucles. Lo hace siguiendo **4 pasos secuenciales**:

Elige un único **Root Bridge** (Puente Raíz) para toda la red.

Cada switch que no es el raíz elige un **Root Port** (Puerto Raíz).

Se elige un **Designated Port** (Puerto Designado) por cada segmento de red.

Los puertos sobrantes se convierten en **Alternate Ports** (Puertos Alternativos) y se bloquean.

---
### Elige el Root Bridge

El Root Bridge es el "jefe" de la red; el punto de referencia para todos los demás switches.

La elección se basa en el **Bridge ID (BID)**. El switch con el **BID más bajo** gana.

El BID se compone de dos partes:

**Prioridad del puente** (Bridge Priority).

**Dirección MAC** del switch.

Los switches se envían mensajes llamados **BPDU** (Bridge Protocol Data Units) cada 2 segundos para comparar sus BIDs y decidir quién manda.

---
### Impacto de las pujas por defecto

De fábrica, **todos** los switches Cisco vienen con la misma prioridad por defecto: **32768**.

Si no cambias esto, todos empatan en prioridad, y el desempate se hace por la **dirección MAC más baja**.

**El peligro:** Una dirección MAC más baja suele pertenecer al switch más viejo y obsoleto de la red. Si dejas la configuración por defecto, un switch viejo de 10 Mbps podría convertirse en el Root Bridge, forzando a que todo el tráfico de tu red Gigabit pase por él, creando un cuello de botella terrible.

---
### Determinar el costo de la ruta raíz

Una vez que hay un Root Bridge, los demás switches necesitan saber cuál es el camino más rápido para llegar a él.

El "costo" se basa en el ancho de banda del enlace (más rápido = menor costo).

**Costos estándar (IEEE):**

10 Gbps = Costo 2

1 Gbps = Costo 4

100 Mbps = Costo 19

10 Mbps = Costo 100

El costo de la ruta es **acumulativo**. El switch suma los costos de los puertos por los que recibe las BPDU hasta llegar al Root Bridge.

---
### Elegir los Puertos Raíz (Root Ports)

Todo switch que **no** sea el Root Bridge debe elegir **un único** Puerto Raíz.

¿Cuál elige? El puerto que tenga el **menor costo total** de ruta hacia el Root Bridge.

Este puerto es la ruta principal de ese switch para enviar tráfico hacia el resto del árbol.

---
### Seleccionar Puertos Designados (Designated Ports)

Cada "segmento" de red (el cable que conecta dos dispositivos) debe tener **un único** Puerto Designado.

El Puerto Designado es el encargado de enviar el tráfico desde y hacia ese segmento.

**Regla de oro:** TODOS los puertos del Root Bridge son Puertos Designados (porque él tiene el costo 0 hacia sí mismo).

En los demás segmentos, el switch que tenga la ruta más barata hacia el Root Bridge es el que pone su puerto como Designado.

---
### Seleccionar Puertos Alternativos (Bloqueados)

Aquí es donde se rompe el bucle.

Si un puerto no fue elegido ni como Puerto Raíz (Root Port) ni como Puerto Designado (Designated Port), se convierte automáticamente en un **Puerto Alternativo**.

STP pone estos puertos en estado de **Bloqueo (Blocking)**. Se ignoran los datos de usuario, pero el puerto sigue escuchando las BPDU por si alguna ruta principal falla y necesita activarse para salvar la red.

---
### Seleccione un puerto raíz a partir de varias rutas de igual coste

¿Qué pasa si un switch tiene dos cables idénticos que van hacia el Root Bridge y ambos tienen exactamente el mismo costo? STP tiene 3 criterios de desempate en este orden estricto:

**El BID del remitente más bajo:** Si los cables vienen de switches distintos, gana el puerto conectado al switch vecino con menor Bridge ID.

**Prioridad de puerto del remitente más baja:** Si ambos cables vienen del _mismo_ switch vecino, gana el puerto que el vecino haya configurado con menor prioridad (por defecto es 128).

**ID de puerto del remitente más bajo:** Si todo lo anterior empata, gana el cable conectado al puerto físico con el número más bajo en el switch vecino (ej. Fa0/1 le gana a Fa0/2).

---
### Temporizadores STP y Estados de puerto

STP usa 3 temporizadores principales basados en BPDUs:

**Hello Time:** Frecuencia con la que se envían BPDUs (2 segundos por defecto).

**Forward Delay:** Tiempo que un puerto pasa en los estados de transición (15 segundos).

**Max Age:** Tiempo máximo que un switch guarda una BPDU antes de considerarla inválida si deja de escuchar al Root (20 segundos).

---
### Detalles Operativos de cada Estado Portuario

Un puerto no se enciende de golpe, pasa por fases para asegurarse de no crear un bucle:

**Bloqueo (Blocking):** No envía datos, no aprende MACs. Solo escucha BPDUs. (20 segundos max).

**Escucha (Listening):** Determina su rol (Raíz o Designado). Envía y recibe BPDUs. No aprende MACs ni envía datos. (15 segundos).

**Aprendizaje (Learning):** Comienza a registrar direcciones MAC en su tabla para prepararse, pero aún no reenvía datos de usuario. (15 segundos).

**Reenvío (Forwarding):** El puerto está 100% operativo. Envía datos y aprende MACs.

_(Deshabilitado/Disabled)_: Un puerto apagado administrativamente.

---
### Per-VLAN Spanning Tree (PVST)

El STP original creaba un solo árbol para toda la red, lo que desaprovechaba los enlaces bloqueados.

Cisco creó **PVST+**, que ejecuta una instancia de STP **por cada VLAN**.

**La gran ventaja:** Puedes hacer balanceo de carga. Puedes configurar que el Switch A sea el Root Bridge para la VLAN 10, y el Switch B sea el Root Bridge para la VLAN 20. Así, un cable físico puede estar bloqueado para la VLAN 10, pero activo transmitiendo tráfico para la VLAN 20.

---

