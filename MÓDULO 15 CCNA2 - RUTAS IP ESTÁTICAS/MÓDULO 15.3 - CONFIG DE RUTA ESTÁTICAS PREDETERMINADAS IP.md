
---
## Configuración de rutas estáticas predeterminadas IP

Las rutas estáticas predeterminadas (o por defecto) son una herramienta esencial para simplificar las tablas de enrutamiento, especialmente en los routers de borde que se conectan a un Proveedor de Servicios de Internet (ISP).

### Ruta estática por defecto

Una ruta estática predeterminada es una ruta que coincide con **todos** los paquetes.

**Funcionamiento:** Actúa como el _Gateway of Last Resort_ (Puerta de enlace de último recurso). Cuando un router recibe un paquete, busca el destino en su tabla de enrutamiento; si no encuentra ninguna coincidencia específica, enviará el paquete utilizando esta ruta predeterminada.

**Caso de uso principal:** Se utiliza comúnmente al conectar un router de borde de una empresa (router _stub_) a la red de un ISP. En lugar de que el router necesite aprender millones de rutas de Internet, simplemente se le instruye: "Cualquier tráfico hacia una red que no conozcas, envíalo al ISP".

**Representación:**

En IPv4, se representa con la dirección de red de ceros y la máscara de ceros: `0.0.0.0 0.0.0.0` (o `0.0.0.0/0`).

En IPv6, se representa con el prefijo de ceros: `::/0`.

----
### Configuración de una ruta estática predeterminada

Al igual que las rutas estáticas normales, puedes configurar una ruta predeterminada indicando la IP del siguiente salto o la interfaz de salida local.

**Configuración para IPv4:**

```
Router(config)# ip route 0.0.0.0 0.0.0.0 {<ip-siguiente-salto> | <interfaz-salida>}
```

_Ejemplo (usando IP de siguiente salto):_ `R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2`

_Ejemplo (usando interfaz de salida):_ `R1(config)# ip route 0.0.0.0 0.0.0.0 Serial0/1/0`

**Configuración para IPv6:**

```
Router(config)# ipv6 route ::/0 {<ip-siguiente-salto> | <interfaz-salida>}
```

_Ejemplo (usando IP de siguiente salto):_ `R1(config)# ipv6 route ::/0 2001:db8:acad:1::2`

_Ejemplo (usando interfaz de salida):_ `R1(config)# ipv6 route ::/0 Serial0/1/0`

---
### Verificar una ruta estática predeterminada

Es crucial verificar que la ruta se haya instalado correctamente en la tabla de enrutamiento para asegurar que el tráfico desconocido tenga una salida válida.

**Verificar en IPv4:**

```
 R1# show ip route static
```

En la salida, buscarás dos cosas clave:

Un mensaje en la parte superior que diga: `Gateway of last resort is 10.1.1.2 to network 0.0.0.0`.
 
La entrada de la ruta identificada con un asterisco **`S*`**. El asterisco indica explícitamente que es una ruta candidata a ser la predeterminada.

_Ejemplo de salida:_ `S* 0.0.0.0/0 [1/0] via 10.1.1.2`

**Verificar en IPv6:**

```
R1# show ipv6 route static
```

A diferencia de IPv4, IPv6 no utiliza el asterisco explícito para la ruta predeterminada. Simplemente verás una ruta estática normal apuntando al prefijo de ceros. _Ejemplo de salida:_ `S ::/0 [1/0] via 2001:DB8:ACAD:1::2`

---



