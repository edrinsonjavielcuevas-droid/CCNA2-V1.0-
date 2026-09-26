
---
### Origen de la ruta

Un router puede poblar su tabla de enrutamiento a partir de tres fuentes principales:

**Conectadas directamente:** Redes adjuntas a las interfaces físicas activas del propio router.

**Estáticas:** Rutas ingresadas manualmente por el administrador de red.

**Dinámicas:** Rutas aprendidas automáticamente intercambiando información con otros routers mediante protocolos (como OSPF o EIGRP).

### Principios de la tabla de enrutamiento

Existen tres principios fundamentales (definidos por Alex Zinin) sobre cómo operan los routers:

Cada router toma sus decisiones de forma independiente, basándose únicamente en la información de su propia tabla.
    
El hecho de que un router tenga cierta información en su tabla no significa que los demás routers la tengan.

La información de enrutamiento sobre un trayecto de ida no proporciona información sobre el trayecto de retorno (el enrutamiento es asimétrico).

---
### Entradas de la tabla de routing

Cada entrada en la tabla proporciona información vital. Puedes visualizarla con el comando `show ip route` (IPv4) o `show ipv6 route` (IPv6). Una entrada típica contiene:

**Origen de la ruta:** Cómo se aprendió (ej. C, S, O, D).

**Red de destino y máscara (Prefijo):** La red a la que se puede llegar.

**Distancia Administrativa (AD) y Métrica:** Valores que indican la confiabilidad y el costo de la ruta.

**Siguiente salto (Next Hop):** La dirección IP del router vecino al que se debe enviar el paquete.

**Interfaz de salida:** La interfaz física local por donde debe salir el paquete.

---
### Redes conectadas directamente

Cuando asignas una dirección IP a una interfaz y la enciendes (`no shutdown`), el router crea automáticamente dos entradas en la tabla:

**`C` (Conectada):** Representa la red completa a la que pertenece la interfaz (ej. `192.168.1.0/24`).

**`L` (Local):** Representa la dirección IP exacta de la interfaz del router (ej. `192.168.1.1/32` en IPv4 o una `/128` en IPv6). Esto optimiza el procesamiento del tráfico dirigido directamente al router.

---
### Rutas estáticas en la tabla de enrutamiento IP

Las rutas estáticas son configuradas manualmente y son ideales para redes de conexión única (Stub networks) o para tener control preciso del tráfico.

**Configuración:** `R1(config)# ip route 10.1.1.0 255.255.255.0 192.168.1.2`

**En la tabla:** Aparecen identificadas con la letra **`S`**. Tienen una Distancia Administrativa por defecto de `1`, lo que las hace preferibles frente a cualquier ruta dinámica.

---
### Protocolos de routing dinámico y sus rutas

Los protocolos dinámicos permiten a los routers adaptarse automáticamente a los cambios de la topología (como la caída de un enlace).

En la tabla de enrutamiento, estas rutas se identifican con códigos específicos del protocolo:

**`O`**: OSPF (Open Shortest Path First).

**`D`**: EIGRP (Enhanced Interior Gateway Routing Protocol).

**`R`**: RIP (Routing Information Protocol).

---
### Ruta predeterminada (Default Route)

Conocida también como _Gateway of Last Resort_ (Puerta de enlace de último recurso). Se utiliza cuando el router recibe un paquete destinado a una red que no coincide con ninguna entrada específica en su tabla.

Suele configurarse de manera estática hacia el router del ISP: `ip route 0.0.0.0 0.0.0.0 [IP_siguiente_salto]`.

En la tabla IPv4 aparece como **`S*`** (la `*` indica que es una ruta candidata a predeterminada). En IPv6 utiliza el prefijo `::/0`.

---
### Estructura de una tabla de enrutamiento IPv4

La salida de `show ip route` en IPv4 tiene una estructura jerárquica heredada de la época del enrutamiento con clase (Classful). Muestra rutas de nivel 1 (redes principales con clase) y rutas secundarias de nivel 2 sangradas debajo (las subredes reales).

### Estructura de una tabla de enrutamiento IPv6

A diferencia de IPv4, la tabla de enrutamiento IPv6 (`show ipv6 route`) no tiene estructura jerárquica con clase. Es "plana" (classless por diseño) y lista todas las rutas directamente. Además, para las rutas dinámicas, el siguiente salto en IPv6 suele ser la dirección _Link-Local_ (`fe80::...`) del router vecino, no su dirección global.

---
### Distancia administrativa (AD)

Si un router aprende sobre exactamente la misma red de destino desde dos orígenes diferentes (por ejemplo, mediante una ruta estática y también mediante OSPF), utiliza la Distancia Administrativa (AD) como criterio de desempate para elegir qué ruta instalar en la tabla.

**Regla:** Cuanto menor sea la AD, más confiable es la ruta.

**Valores comunes para recordar:**

Conectada directamente = 0

Ruta Estática = 1

EIGRP externo = 90

OSPF = 110

RIP = 120

---

