
---
## Configuración básica de un router

La configuración inicial de un router Cisco establece la base de seguridad, identificación y conectividad del dispositivo en la red. A continuación se detallan los comandos esenciales y técnicas de filtrado que debes dominar.

### Topología

Antes de introducir cualquier comando, es fundamental comprender la topología física y lógica de la red.

**Física:** Muestra cómo están conectados los cables entre el router, los switches de la LAN y el enlace WAN hacia el proveedor de servicios o hacia otros routers.

**Lógica:** Define el esquema de direccionamiento IP (IPv4 e IPv6), indicando qué subredes pertenecen a qué interfaces (ej. GigabitEthernet0/0/0) y cuáles serán las direcciones de _Gateway_ predeterminado para los hosts.

---
### Comandos de Configuración

Estos son los comandos básicos universales que se deben aplicar a cualquier router nuevo para asegurarlo e identificarlo en la red:

**Asignar nombre al dispositivo:**

```
Router(config)# hostname R1
```


**Asegurar el modo EXEC privilegiado:**

```
R1(config)# enable secret class
```

**Asegurar el puerto de consola (acceso físico):**

```
R1(config)# line console 0

R1(config-line)# password cisco

R1(config-line)# login

R1(config-line)# exit
```

**Asegurar las líneas VTY (acceso remoto SSH/Telnet):**

```
R1(config)# line vty 0 4

R1(config-line)# password cisco

R1(config-line)# login

R1(config-line)# exit
```

**Cifrar todas las contraseñas de texto sin formato:**

```
R1(config)# service password-encryption
```

**Crear un mensaje del día (Banner MOTD) para advertencia legal:**

```
R1(config)# banner motd # ACCESO RESTRINGIDO. SOLO PERSONAL AUTORIZADO. #
```

**Guardar la configuración en la NVRAM:**

```
 R1# copy running-config startup-config 
```

----
### Comandos de verificación

Una vez configurado el equipo, debes verificar que los parámetros se hayan aplicado correctamente y que las interfaces estén operativas.

**Verificar el estado de las interfaces (Resumen rápido):**

```
R1# show ip interface brief

R1# show ipv6 interface brief
```

**Verificar la tabla de enrutamiento:**

```
R1# show ip route

R1# show ipv6 route
```

**Ver la configuración activa actual completa:**

```
R1# show running-config
```

----
### Salida del comando de filtro

Cuando los archivos de configuración o las tablas de enrutamiento son muy grandes, navegar por toda la salida de un comando `show` es ineficiente. Cisco IOS permite usar el carácter de tubería (pipe `|`) para filtrar los resultados.

**`section` (sección):** Muestra toda la sección que comienza con la expresión de filtrado.

```
R1# show running-config | section line vty
```

**`include` (incluir):** Muestra solo las líneas exactas que contienen la palabra especificada. Es ideal para buscar interfaces activas.

```
R1# show ip interface brief | include up
```

**`exclude` (excluir):** Muestra todas las líneas de salida excepto las que coinciden con la palabra especificada. Útil para descartar interfaces apagadas.

```
R1# show ip interface brief | exclude unassigned
```

**`begin` (comenzar):** Muestra todas las líneas a partir de un punto específico hacia abajo.

```
R1# show ip route | begin Gateway
```

----

### Comandos de Terminal adicionales

**Evitar que los mensajes del sistema interrumpan tu escritura en la consola:**

```
R1(config)# line console 0

R1(config-line)# logging synchronous
```

**Permitir acceso remoto específico por las líneas VTY (SSH y Telnet):**

```
R1(config)# line vty 0 4

R1(config-line)# transport input ssh telnet
```

### Habilitar el Enrutamiento IPv6

**Comando global indispensable para que el router procese tráfico IPv6:**

```
R1(config)# ipv6 unicast-routing
```

### Configuración de Interfaces (IPv4 e IPv6)

Estos comandos configuran las direcciones IP, añaden una descripción útil y encienden los puertos físicos del router.

**Configuración de la interfaz GigabitEthernet 0/0/0 (LAN 1):**

```
R1(config)# interface gigabitethernet 0/0/0

R1(config-if)# description Link to LAN 1

R1(config-if)# ip address 10.0.1.1 255.255.255.0

R1(config-if)# ipv6 address 2001:db8:acad:1::1/64

R1(config-if)# ipv6 address fe80::1:a link-local

R1(config-if)# no shutdown

R1(config-if)# exit
```

**Configuración de la interfaz GigabitEthernet 0/0/1 (LAN 2):**

```
R1(config)# interface gigabitethernet 0/0/1

R1(config-if)# description Link to LAN 2

R1(config-if)# ip address 10.0.2.1 255.255.255.0

R1(config-if)# ipv6 address 2001:db8:acad:2::1/64

R1(config-if)# ipv6 address fe80::1:b link-local

R1(config-if)# no shutdown

R1(config-if)# exit
```

**Configuración de la interfaz Serial 0/1/1 (Enlace WAN hacia R2):**

```
R1(config)# interface serial 0/1/1

R1(config-if)# description Link to R2

R1(config-if)# ip address 10.0.3.1 255.255.255.0

R1(config-if)# ipv6 address 2001:db8:acad:3::1/64

R1(config-if)# ipv6 address fe80::1:c link-local

R1(config-if)# no shutdown

R1(config-if)# exit
```

----
