
---
## Roles de router DHCPv6

Un router con IPv6 puede desempeñar tres roles principales dentro de la infraestructura de red:

**Servidor DHCPv6:** Responde a las solicitudes de los clientes DHCPv6 (puede ser _stateless_ o _stateful_).

**Cliente DHCPv6:** El router solicita su propia configuración IP a un servidor DHCPv6 (común cuando un router de borde se conecta a un ISP).

**Agente de retransmisión (Relay Agent):** Reenvía mensajes DHCPv6 entre clientes y un servidor DHCPv6 ubicados en diferentes subredes.

----
## Configurar un servidor DHCPv6 stateless

En este modo, el router proporciona parámetros adicionales (como servidores DNS y nombres de dominio), mientras que la dirección IPv6 se obtiene mediante SLAAC.

### Pasos de configuración:

1. **Habilitar el enrutamiento IPv6** globalmente:

    ```
  Router(config)# ipv6 unicast-routing
    ```

2. **Crear el pool de DHCPv6** y asignarle un nombre:

    ```
    Router(config)# ipv6 dhcp pool OSPF-POOL
    ```

3. **Configurar los parámetros opcionales** dentro del pool (ej. DNS y dominio):

    ```
    Router(config-dhcpv6)# dns-server 2001:db8:cafe:1111::1
    
    Router(config-dhcpv6)# domain-name ejemplo.com
    
    Router(config-dhcpv6)# exit
    
    ```

4. **Vincular el pool a la interfaz** que interactúa con los clientes y configurar el flag **Other** (`O` flag en 1):

```
  Router(config)# interface GigabitEthernet0/0/1
  
  Router(config-if)# ipv6 dhcp server OSPF-POOL
  
  Router(config-if)# ipv6 nd other-config-flag

```

---
## Configurar un cliente DHCPv6 stateless

Si quieres configurar un router para que actúe como cliente _stateless_ (recibiendo configuraciones mediante RA y solicitando el resto por DHCPv6 stateless):

### Comandos de configuración:

Plaintext

```
Router(config)# interface GigabitEthernet0/0/0

Router(config-if)# ipv6 enable

Router(config-if)# ipv6 address autoconfig

```

**`ipv6 address autoconfig`**: Le indica al router que utilice SLAAC para autoconfigurar su dirección IPv6 global a partir del anuncio del router (RA) y que active el proceso stateless para obtener el DNS.

----
## Configurar un servidor DHCPv6 stateful

En este modo, el servidor asigna direcciones IPv6 completas además de la información de configuración (equivalente a DHCPv4).

### Pasos de configuración:

1. **Habilitar enrutamiento IPv6**:

    ```
    Router(config)# ipv6 unicast-routing
    ```

2. **Crear el pool de DHCPv6 stateful** y definir el prefijo de red a entregar:

    ```
    Router(config)# ipv6 dhcp pool STATEFUL-POOL
    
    Router(config-dhcpv6)# address prefix 2001:db8:acad:1::/64
    
    Router(config-dhcpv6)# dns-server 2001:db8:acad::254
    
    Router(config-dhcpv6)# domain-name cisco.local
    
    Router(config-dhcpv6)# exit
    ```

3. **Configurar la interfaz del router** y activar el flag **Managed** (`M` flag en 1):

    ```
    Router(config)# interface GigabitEthernet0/0/1
    
    Router(config-if)# ipv6 dhcp server STATEFUL-POOL
    
    Router(config-if)# ipv6 nd managed-config-flag
    
    ```

---
## Configurar un cliente DHCPv6 stateful

Para configurar una interfaz de un router para que obtenga su dirección IPv6 de forma dinámica y completa mediante DHCPv6 stateful:

### Comandos de configuración:

```
Router(config)# interface GigabitEthernet0/0/0

Router(config-if)# ipv6 enable

Router(config-if)# ipv6 address dhcp

```

**`ipv6 address dhcp`**: Fuerza a la interfaz a solicitar una dirección IPv6 completa y parámetros de configuración directamente a un servidor DHCPv6 stateful.

---
## Comandos de verificación del servidor DHCPv6

Utiliza estos comandos para comprobar el estado y las concesiones del servidor DHCPv6:

**Ver los pools configurados y clientes activos:**


    ```
    Router# show ipv6 dhcp pool
    ```

**Ver las asignaciones de direcciones IP (bindings) a los clientes:**

    ```
    Router# show ipv6 dhcp binding
    ```

(Muestra el DUID del cliente, la dirección IPv6 asignada y el tiempo de concesión).

**Verificar el estado de las interfaces y los flags activos (M y O):**

    ```
    Router# show ipv6 interface GigabitEthernet0/0/1
    ```

---
## Configuración del agente de retransmisión DHCPv6 (Relay Agent)

Se utiliza cuando el servidor DHCPv6 se encuentra en una red distinta a la de los clientes. El router intermediario intercepta los mensajes de solicitud (Solicit) y los reenvía por _unicast_ al servidor.

### Comandos de configuración:

1. Ir a la interfaz local que recibe las peticiones de los clientes:

    ```
    Router(config)# interface GigabitEthernet0/0/0
    ```

2. Configurar la dirección IP del servidor DHCPv6 de destino:
 
    ```
    Router(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 GigabitEthernet0/0/1
    ```

_(Donde `2001:db8:acad:2::1` es la IP global del servidor DHCPv6)._

---

## Verificar el agente de retransmisión de DHCPv6

Para comprobar que el agente de retransmisión está funcionando correctamente:

**Verificar la configuración de retransmisión en la interfaz:**

     ```
    Router# show ipv6 dhcp interface GigabitEthernet0/0/0
    ```

_(Debe mostrar que la interfaz actúa como agente de retransmisión hacia la IP del servidor)._

- **Ver los enlaces de retransmisión activos:**

```
    Router# show ipv6 dhcp relay binding
```

----
