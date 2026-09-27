
---

## Configuración de rutas de host estáticas

Una ruta de host es una ruta que dirige el tráfico hacia una única dirección IP específica en lugar de hacia una red o subred completa. Al aplicar la regla de la "coincidencia más larga", una ruta de host siempre tendrá la máxima prioridad, ya que representa una coincidencia exacta de todos los bits.

### Rutas del host

**Concepto:** A diferencia de una ruta de red típica (ej. `/24` o `/64`), una ruta de host se define utilizando la máscara de subred más larga posible.

**En IPv4:** Se utiliza una máscara de **32 bits** (`255.255.255.255` o `/32`).

**En IPv6:** Se utiliza una longitud de prefijo de **128 bits** (`/128`).

### Rutas de host instaladas automáticamente

El router crea rutas de host por sí solo en ciertas circunstancias para optimizar su funcionamiento interno.

Cuando configuras una dirección IP en una interfaz activa del router, este instala automáticamente una ruta de host para esa IP exacta en la tabla de enrutamiento.

Se identifican en la tabla con la letra **`L` (Local)**. Esto permite al router procesar el tráfico dirigido directamente a él de manera mucho más eficiente.

### Ruta estática de host

Como administrador, puedes configurar manualmente una ruta de host estática.

**Caso de uso principal:** Se utilizan cuando deseas forzar que el tráfico hacia un servidor o dispositivo en particular tome un camino distinto al tráfico general de su subred. Por ejemplo, si todo el tráfico de la red `192.168.1.0/24` sale por el ISP 1, pero quieres que el tráfico hacia un servidor de base de datos específico (`192.168.1.50`) salga por un enlace dedicado seguro (ISP 2).

---
### Configuración de rutas de host estáticas

La sintaxis es idéntica a la de una ruta estática normal, pero asegurando el uso de la máscara `/32`.

**Sintaxis IPv4:**

```
Router(config)# ip route <ip-del-host> 255.255.255.255 <ip-siguiente-salto>
```

**Ejemplo:** Enrutar el tráfico específico para el servidor `10.1.1.50` a través del router vecino `192.168.1.2`.

```
R1(config)# ip route 10.1.1.50 255.255.255.255 192.168.1.2
```

### Verificar rutas de host estáticas

Para confirmar que la ruta se ha instalado correctamente con su máscara `/32`, utiliza los comandos de verificación estándar filtrando por el destino exacto.

```
R1# show ip route 10.1.1.50
```

La salida confirmará que la ruta es estática (S) y mostrará explícitamente el prefijo `/32`.

### Configurar rutas de host estáticas IPV6 con Link-Local de siguiente salto

Para crear una ruta de host en IPv6, se utiliza el prefijo `/128`. Si decides utilizar una dirección _Link-Local_ como el siguiente salto, recuerda la regla obligatoria: **debe ser una ruta completamente especificada** (indicando tanto la interfaz de salida como la IP del vecino).

**Sintaxis IPv6:**

```
Router(config)# ipv6 route <ip-del-host>/128 <interfaz-salida> <ip-link-local-vecino>
```

**Ejemplo:** Enrutar el tráfico hacia el servidor `2001:db8:acad:2::99` a través de la interfaz Gigabit 0/0/1 hacia la IP Link-Local `fe80::2`.

```
R1(config)# ipv6 route 2001:db8:acad:2::99/128 GigabitEthernet0/0/1 fe80::2
```

----
