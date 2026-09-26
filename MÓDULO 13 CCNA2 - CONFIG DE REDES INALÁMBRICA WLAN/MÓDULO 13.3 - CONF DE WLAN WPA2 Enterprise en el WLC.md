
---

## Configure una red inalámbrica WLAN WPA2 Enterprise en el WLC

A diferencia de WPA2-Personal (que usa una contraseña compartida), WPA2 Enterprise requiere que cada usuario se autentique con sus propias credenciales mediante el estándar 802.1X y un servidor RADIUS. A continuación tienes el desarrollo de cada punto con los pasos exactos de configuración (como se mostrarían en los videos) listos para tus notas.

---
### Defina un servidor RADIUS y SNMP en el WLC

En un entorno empresarial, el WLC no guarda las contraseñas de los usuarios; debe delegar esta tarea a un servidor RADIUS externo. Además, se configura SNMP (Protocolo Simple de Administración de Red) para que un servidor de monitoreo central pueda registrar los eventos del WLC.

**Pasos de configuración mostrados en el video:**
    
Para RADIUS: Ve a la pestaña **Security** > **AAA** > **RADIUS** > **Authentication**. Haz clic en _New_.
        
Para SNMP: Ve a la pestaña **Management** > **SNMP** > **Trap Receivers**. Haz clic en _New_.

---
### SNMP y RADIUS

**SNMP:** Permite que el WLC envíe mensajes de registro (traps) a un servidor de administración (NMS) cuando ocurren eventos importantes (como la caída de un AP o la detección de un Rogue AP).

**RADIUS:** Es el protocolo de autenticación, autorización y contabilidad (AAA). El WLC actúa como un cliente RADIUS, pasando las credenciales del usuario inalámbrico al servidor RADIUS para que este las apruebe o rechace.

---

### Configurar Información del Servidor SNMP

Para completar la configuración del servidor de monitoreo (SNMP Trap Receiver) en la GUI del WLC:

Ingresa el nombre de la **Comunidad (Community Name)**, que actúa como una contraseña para SNMP (por ejemplo, `WLAN_SNMP`).

Ingresa la **Dirección IP** del servidor NMS que recibirá los registros.

Establece el estado en **Enable** y aplica los cambios.

---

### Configure los servidores RADIUS

Para enlazar el WLC con el servidor que verificará las contraseñas:

En la pantalla de nuevo servidor RADIUS, ingresa la **Dirección IP del servidor RADIUS**.

Ingresa el **Shared Secret (Secreto compartido)**. Esta es una contraseña interna que debe coincidir exactamente en el WLC y en el servidor RADIUS para que confíen el uno en el otro.

Confirma el Shared Secret, deja el número de puerto por defecto (1812) y haz clic en _Apply_.

---
### Configurar una VLAN para una nueva WLAN

Antes de crear la red Wi-Fi pública, el WLC necesita una interfaz virtual (dinámica) asociada a una VLAN específica de la red cableada, por donde saldrá el tráfico de los usuarios.

**Pasos de configuración mostrados en el video:**

Navega a **Controller** > **Interfaces**.

Haz clic en **New**.

Asigna un nombre a la interfaz (ej. `VLAN5_Int`) y el **VLAN ID** correspondiente (ej. `5`). Haz clic en _Apply_.

---
### Topologia de direcciones en VLAN 5

Toda interfaz en el WLC requiere parámetros IP válidos que coincidan con la topología de la red para esa VLAN específica (en este ejemplo, la VLAN 5). Necesitarás conocer la dirección IP que se le asignará a la interfaz del WLC, la máscara de subred y la IP del Gateway predeterminado (el router) de esa VLAN.

---
### Configurar una nueva interfaz

Continuando con la pantalla que se abrió en el paso 13.3.5:

Asigna la interfaz a un puerto físico del WLC (generalmente el puerto `1`).

Ingresa la **IP Address**, **Netmask** (Máscara de subred) y **Gateway** de la VLAN 5.

Define el **Primary DHCP Server** (la IP del servidor que dará direcciones a los clientes de esta VLAN). Si el propio WLC será el DHCP, se coloca su IP de administración.

Aplica y guarda la configuración.

---
### Configurar el Alcance de DHCP

El WLC tiene la capacidad de actuar como un servidor DHCP interno para asignar direcciones IP a los clientes inalámbricos que se conecten a la nueva red.

**Pasos de configuración mostrados en el video:**

Navega a **Controller** > **Internal DHCP Server** > **DHCP Scope**.

Haz clic en **New**, dale un nombre al alcance (ej. `Scope_VLAN5`) y haz clic en _Apply_.

Haz clic en el nuevo nombre creado para editar sus parámetros.

---
### Configurar el Alcance DHCP

Dentro de la edición del _DHCP Scope_:

Define el **Pool Start Address** (IP de inicio) y **Pool End Address** (IP final) para los clientes.

Especifica la **Network** y la **Netmask**.

Ingresa el **Default Routers** (el Gateway de la VLAN 5) y los servidores **DNS**.

Cambia el estado (Status) a **Enabled** y aplica los cambios.

---
### Configure una red inalámbrica WLAN WPA2 Enterprise

Una vez configurado RADIUS, la interfaz VLAN y el DHCP, es momento de crear la red Wi-Fi (SSID) y amarrar todos los elementos.

**Pasos de configuración mostrados en el video:**

Ve a la pestaña **WLANs** y selecciona **Create New**.

Asigna un Profile Name y el SSID (lo que verán los usuarios, ej. `Red_Empresarial`).

En la pestaña _General_, asegúrate de marcar **Status: Enabled** y selecciona la interfaz que creaste en el paso 13.3.5 (ej. `VLAN5_Int`) en el menú desplegable _Interface/Interface Group_.

---
### Configure una red inalámbrica WLAN WPA2 Enterprise

La parte crítica de la configuración WPA2 Enterprise se realiza en las pestañas de seguridad de la nueva WLAN:

Ve a la pestaña **Security** > subpestaña **Layer 2**.

Asegúrate de que la seguridad de Capa 2 sea **WPA+WPA2**.

En los parámetros WPA2, verifica que **AES** esté habilitado.

En _Auth Key Mgmt_, selecciona **802.1X** (y desmarca PSK si estuviera marcado).

Ve a la subpestaña **AAA Servers**. En la lista desplegable de _Server 1_, selecciona la dirección IP del servidor RADIUS que definiste en el paso 13.3.4.

Aplica los cambios. La red empresarial está lista y segura.

----



