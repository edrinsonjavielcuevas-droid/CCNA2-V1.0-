
---
## Rutas estáticas

El enrutamiento estático requiere que el administrador de la red configure manualmente las rutas hacia redes de destino específicas. Es fundamental para redes pequeñas, conexiones de extremo a extremo (stub) y para crear rutas de respaldo.

### Tipos de rutas estáticas

Existen cuatro tipos principales de rutas estáticas que puedes implementar en un router:

**Ruta estática estándar:** Se utiliza para conectarse a una red remota específica.

**Ruta estática predeterminada (Default):** Coincide con todos los paquetes (IPv4 `0.0.0.0/0` o IPv6 `::/0`). Se usa generalmente para enviar tráfico hacia el Proveedor de Servicios de Internet (ISP) cuando no hay una ruta más específica en la tabla.

**Ruta estática resumida:** Agrupa varias subredes contiguas en una sola entrada de la tabla de enrutamiento para reducir su tamaño y ahorrar memoria (ej. agrupar `192.168.1.0/24` y `192.168.2.0/24` en `192.168.0.0/16`).

**Ruta estática flotante:** Es una ruta de respaldo que solo se activa si la ruta principal (dinámica o estática) falla. Se configura asignándole una Distancia Administrativa (AD) mayor que la ruta principal.

---
### Opciones de siguiente salto

Al configurar una ruta estática, debes indicarle al router por dónde enviar el paquete. Hay tres formas de especificar este "siguiente salto":

**Ruta del siguiente salto (Recursiva):** Solo se especifica la dirección IP del router vecino. El router debe realizar una doble búsqueda en la tabla de enrutamiento (una para encontrar la ruta remota y otra para resolver cómo llegar a esa IP del vecino).

**Ruta conectada directamente:** Solo se especifica la interfaz de salida local (ej. `GigabitEthernet0/0/1`). Es más rápida porque el router envía el paquete directamente por esa interfaz sin búsquedas adicionales.

**Ruta completamente especificada:** Se indica tanto la interfaz de salida local como la dirección IP del router vecino. Es obligatoria en redes de acceso múltiple (como Ethernet) para que el router sepa exactamente qué dirección MAC resolver mediante ARP o ND.

---
### Comando de ruta estática IPv4

La sintaxis global para configurar una ruta estática en IPv4 es:

```
Router(config)# ip route <red-destino> <máscara-subred> {<ip-siguiente-salto> | <interfaz-salida>}
```

_Ejemplo con IP del siguiente salto:_ `ip route 192.168.2.0 255.255.255.0 10.1.1.2` 

_Ejemplo con interfaz de salida:_ `ip route 192.168.2.0 255.255.255.0 s0/0/0`

### Comando de ruta estática IPv6

La sintaxis para IPv6 es idéntica en lógica, pero adaptada al formato de prefijo:

```
Router(config)# ipv6 route <prefijo-ipv6>/<longitud-prefijo> {<ip-siguiente-salto> | <interfaz-salida>}
```

_Ejemplo:_ `ipv6 route 2001:db8:acad:2::/64 2001:db8:acad:1::2`

---
### Topología Dual-Stack

En las redes modernas, los routers operan frecuentemente en modo Dual-Stack (Pila doble). Esto significa que procesan tráfico IPv4 e IPv6 simultáneamente en las mismas interfaces físicas. Es importante recordar que las tablas de enrutamiento para IPv4 e IPv6 son estructuras de datos completamente separadas e independientes; configurar una ruta estática para IPv4 no enruta automáticamente el tráfico IPv6 hacia ese destino, se deben configurar ambas.

---
### Iniciando tablas de enrutamiento IPv4

Antes de configurar cualquier ruta estática, un router recién inicializado con sus interfaces configuradas y encendidas poblará su tabla de enrutamiento IPv4 únicamente con las redes **Conectadas directamente (C)** y las rutas **Locales (L)** correspondientes a las IPs de sus propias interfaces. Cualquier destino fuera de estas redes será inalcanzable hasta que se agreguen rutas estáticas o dinámicas.

---
### Inicio de tablas de enrutamiento de IPv6

De manera similar, al configurar direcciones IPv6 y habilitar `ipv6 unicast-routing`, la tabla de enrutamiento IPv6 inicial solo mostrará las rutas **Conectadas (C)** y **Locales (L)**. Las rutas locales en IPv6 incluyen las direcciones Unicast Globales configuradas con prefijo `/128` y pueden incluir las direcciones Link-Local (`fe80::/10`).

---

