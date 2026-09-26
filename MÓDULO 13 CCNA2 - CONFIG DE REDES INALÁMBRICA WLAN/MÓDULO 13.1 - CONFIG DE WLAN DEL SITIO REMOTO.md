
---

Para configurar una red inalámbrica en un sitio remoto o en un entorno de pequeña oficina/oficina en casa (SOHO), se suele utilizar un router inalámbrico multifunción. A continuación, se detalla la estructura lógica y los pasos de configuración para este tipo de dispositivos.

----
### Router inalámbrico

El router inalámbrico de sitio remoto es un dispositivo "todo en uno" que integra tres funciones principales:

**Punto de Acceso (AP):** Proporciona la señal de radiofrecuencia (Wi-Fi) para los clientes inalámbricos.

**Switch Ethernet:** Incluye puertos LAN (generalmente 4) para conectar dispositivos locales por cable.

**Router:** Gestiona el tráfico entre la red local (LAN) y la red del proveedor de servicios de Internet (WAN), realizando funciones de enrutamiento y traducción de direcciones.

---
### Conéctese al router inalámbrico

Para realizar la configuración inicial, es altamente recomendable no hacerlo por Wi-Fi.

**El proceso:** Se debe conectar una PC directamente a uno de los puertos LAN del router utilizando un cable Ethernet.

**Acceso:** La PC obtendrá una dirección IP automáticamente (vía DHCP). Luego, el administrador debe abrir un navegador web e ingresar la dirección IP de la puerta de enlace predeterminada (frecuentemente `192.168.0.1` o `192.168.1.1`) para acceder al panel de administración mediante credenciales predeterminadas (como admin/admin).

---
### Configuración básica de red

Esta sección se divide en dos partes fundamentales dentro de la interfaz del router:

**Configuración WAN (Internet):** Define cómo el router obtiene su dirección IP pública del ISP. Puede ser mediante DHCP (dinámica), IP estática o PPPoE (requiere usuario y contraseña del proveedor).

**Configuración LAN:** Permite cambiar la dirección IP interna del propio router (por seguridad) y definir el rango de direcciones IP que el servidor DHCP interno asignará a los dispositivos de la red local.

---
### Configuración inalámbrica

Aquí se establecen los parámetros de la red Wi-Fi para que los dispositivos puedan conectarse de forma segura:

**SSID (Nombre de la red):** Se define el nombre público de la red. Es recomendable crear SSIDs separados si el router es de doble banda (2.4 GHz y 5 GHz).

**Canales y Ancho de banda:** Se selecciona el canal de radiofrecuencia con menor interferencia y el ancho del canal (ej. 20 MHz, 40 MHz).

**Seguridad:** Se debe configurar el modo de seguridad más alto disponible (WPA2-Personal o WPA3-Personal) y establecer una frase de contraseña (PSK) robusta.

---
### Configuración de una red de Malla Inalámbrica (Wireless Mesh)

Para sitios remotos grandes donde un solo router no cubre toda el área, se utilizan redes de malla.

**Funcionamiento:** En lugar de extensores de rango tradicionales, un sistema Mesh utiliza varios nodos (APs) que se comunican entre sí de forma inalámbrica para crear una única red unificada (mismo SSID).

**Ventaja:** Los dispositivos cliente experimentan un _roaming_ (itinerancia) perfecto al moverse por el sitio, ya que el sistema gestiona dinámicamente a qué nodo deben conectarse para obtener la mejor señal.

----
### NAT para IPv4

La Traducción de Direcciones de Red (NAT) es la función que permite a todos los dispositivos de la red local acceder a Internet.

**Mecanismo:** Traduce las múltiples direcciones IP privadas (ej. `192.168.1.x`) de la LAN a la única dirección IP pública asignada a la interfaz WAN del router. Esto oculta la estructura interna de la red de cara al exterior y conserva el espacio de direcciones IPv4.

---
### Calidad de servicio (QoS)

QoS es vital en enlaces de sitios remotos donde el ancho de banda de Internet puede ser limitado.

**Función:** Permite al administrador clasificar y priorizar ciertos tipos de tráfico sobre otros.

**Aplicación:** Se puede configurar el router para que el tráfico sensible a la latencia, como las llamadas de voz sobre IP (VoIP) o las videoconferencias, tenga prioridad absoluta sobre descargas de archivos pesados o actualizaciones en segundo plano.

---
### Reenvío de Puerto (Port Forwarding)

Por defecto, el router (gracias a NAT y su firewall integrado) bloquea cualquier conexión iniciada desde el exterior (Internet) hacia el interior (LAN).

**Uso:** El reenvío de puertos permite abrir un "túnel" específico. Si tienes un servidor web, una cámara de seguridad o un servidor de juegos en la red local, configuras el router para que cualquier petición que llegue a la IP pública en un puerto específico (ej. puerto 80) sea redirigida automáticamente a la IP privada y puerto de ese dispositivo interno.

---
