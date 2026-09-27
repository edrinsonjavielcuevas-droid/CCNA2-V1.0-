
---
### Ruta estática IPv4 de siguiente salto

En este método, solo se especifica la dirección IP del router vecino (el siguiente salto). El router debe realizar una búsqueda recursiva: primero busca la red de destino para encontrar el siguiente salto, y luego busca en la tabla de enrutamiento cómo llegar a ese siguiente salto (qué interfaz local usar).

**Sintaxis:** `ip route [red-destino] [máscara-subred] [ip-siguiente-salto]`

**Ejemplo:** Para llegar a la red `192.168.2.0/24`, el paquete debe enviarse a la IP del router vecino `10.1.1.2`.

```
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.1.2
```

---
### Ruta estática IPv6 de siguiente salto

Aplica la misma lógica recursiva que IPv4, pero utilizando el formato de prefijo y dirección global de IPv6.

**Sintaxis:** `ipv6 route [prefijo-destino]/[longitud] [ip-siguiente-salto]`

**Ejemplo:** Para llegar a la red `2001:db8:acad:2::/64`, se envía a la dirección global del vecino `2001:db8:acad:1::2`.

```
R1(config)# ipv6 route 2001:db8:acad:2::/64 2001:db8:acad:1::2
```

---
### Ruta Estática IPv4 Conectada Directamente

Solo se especifica la interfaz de salida local. El router asume que el destino está directamente conectado a esa interfaz. Es ideal para conexiones seriales punto a punto. **No se recomienda** para redes de acceso múltiple (como Ethernet), ya que el router tendría que enviar solicitudes ARP por cada destino individual, saturando la red.

**Sintaxis:** `ip route [red-destino] [máscara-subred] [interfaz-salida]`

**Ejemplo:** Enviar el tráfico de la red `192.168.2.0/24` por la interfaz serial local.

```
R1(config)# ip route 192.168.2.0 255.255.255.0 Serial0/1/0
```

---
### Ruta Estática IPv6 Conectada Directamente

Sigue el mismo principio de usar solo la interfaz de salida, también recomendado exclusivamente para enlaces punto a punto.

**Sintaxis:** `ipv6 route [prefijo-destino]/[longitud] [interfaz-salida]`

**Ejemplo:**

```
R1(config)# ipv6 route 2001:db8:acad:2::/64 Serial0/1/0
```

----
### Ruta estática completamente especificada IPv4

Se declara tanto la interfaz de salida local como la dirección IP del siguiente salto. Es el método recomendado para interfaces Ethernet de acceso múltiple cuando se utiliza CEF (Cisco Express Forwarding), ya que elimina la búsqueda recursiva y proporciona al router la IP exacta para la cual debe resolver la dirección MAC.

**Sintaxis:** `ip route [red-destino] [máscara-subred] [interfaz-salida] [ip-siguiente-salto]`

**Ejemplo:**

```
R1(config)# ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0/1 10.1.1.2
```

----
### Ruta estática completamente especificada IPv6

En IPv6, este método es **obligatorio** si decides utilizar una dirección _Link-Local_ (`fe80::...`) como tu siguiente salto. Dado que las direcciones _Link-Local_ no son enrutables y pueden repetirse en diferentes enlaces, el router necesita saber exactamente por cuál interfaz física debe buscar ese siguiente salto.

**Sintaxis:** `ipv6 route [prefijo-destino]/[longitud] [interfaz-salida] [ip-link-local-siguiente-salto]`

**Ejemplo:**

```
R1(config)# ipv6 route 2001:db8:acad:2::/64 GigabitEthernet0/0/1 fe80::2
```

---
### Verificación de una ruta estática

Una vez configuradas las rutas, debes comprobar que se hayan instalado correctamente en la tabla de enrutamiento y que haya conectividad.

**Ver solo las rutas estáticas IPv4 (identificadas con 'S'):**

```
R1# show ip route static
```

**Ver la ruta específica que el router usará para un destino (útil para ver qué interfaz o salto eligió CEF):**

```
R1# show ip route 192.168.2.0
``` 

**Ver solo las rutas estáticas IPv6:**

```
R1# show ipv6 route static
```

**Probar conectividad al host destino:**

```
R1# ping 192.168.2.10
```

---

