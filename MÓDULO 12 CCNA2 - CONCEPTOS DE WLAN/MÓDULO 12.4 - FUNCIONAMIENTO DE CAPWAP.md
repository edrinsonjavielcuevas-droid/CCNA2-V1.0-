
---
### Funcionamiento de CAPWAP

El protocolo CAPWAP (Control y Aprovisionamiento de Puntos de Acceso Inalámbricos) es el estándar de la industria que permite a un Controlador de LAN Inalámbrica (WLC) gestionar múltiples puntos de acceso ligeros (LAPs) de forma centralizada a través de la 
red.

### Introducción a la CAPWAP

CAPWAP es un protocolo estándar del IEEE que permite al WLC administrar los LAPs y enrutar el tráfico de los clientes WLAN.

**Funcionamiento:** CAPWAP establece túneles lógicos entre el WLC y cada AP a través de la red IP (funciona tanto en IPv4 como en IPv6).

**Puertos utilizados:** Utiliza dos puertos UDP distintos para separar el tráfico:

**UDP 5246:** Se usa para el túnel de **Control** (administración, configuración del AP y actualizaciones de firmware).

**UDP 5247:** Se usa para el túnel de **Datos** (el tráfico real de Internet o red local generado por los clientes inalámbricos).

----
### Arquitectura MAC dividida

Un concepto clave en redes inalámbricas empresariales es la Arquitectura MAC dividida (Split MAC), que divide las funciones de la Capa 2 entre el Punto de Acceso Ligero (LAP) y el Controlador (WLC) para optimizar el rendimiento.

**Responsabilidades del LAP (Funciones en tiempo real):** El AP físico se encarga de las tareas que requieren respuesta inmediata en el aire. Esto incluye transmitir mensajes Beacon y Probe Response, enviar acuses de recibo (ACKs) para evitar colisiones mediante CSMA/CA, y aplicar el cifrado a nivel de paquete en el aire.

**Responsabilidades del WLC (Funciones ajenas al tiempo real):** El controlador asume las tareas administrativas pesadas a nivel global. Esto incluye la autenticación segura de los clientes (ej. comunicarse con el servidor RADIUS), gestionar el _roaming_ cuando un usuario cambia de AP, y gestionar las políticas de Calidad de Servicio (QoS) y seguridad.

---
### Encriptación de DTLS

Para garantizar que la gestión centralizada sea segura y no pueda ser interceptada o saboteada por atacantes en la red cableada, CAPWAP utiliza Seguridad de la Capa de Transporte de Datagramas (DTLS).

**Túnel de Control seguro:** Por defecto, DTLS encripta todo el túnel de control CAPWAP (UDP 5246). Esto protege las contraseñas, configuraciones y actualizaciones que el WLC envía a los APs.

**Túnel de Datos:** Generalmente, el tráfico de datos de los usuarios (UDP 5247) **no** está encriptado por DTLS de forma predeterminada para no sobrecargar el procesador del AP ni del WLC. Si se requiere máxima seguridad (por ejemplo, en un entorno militar o bancario), la encriptación de datos DTLS puede habilitarse, pero a menudo requiere licencias o hardware especializado.

---

### AP FlexConnect

FlexConnect es una solución de implementación inalámbrica diseñada específicamente para sucursales u oficinas remotas.


**El problema original:** Si una sucursal tiene sus APs conectados a un WLC ubicado en la sede central a través de un enlace WAN (como Internet o una VPN), y ese enlace se cae, los APs ligeros normalmente dejarían de funcionar, desconectando a todos los usuarios de la sucursal.

**La solución (Modos de FlexConnect):**

**Modo conectado:** El AP funciona normalmente, tunelizando el tráfico de control y de datos hacia el WLC central.

**Modo autónomo (Standalone):** Si el enlace WAN hacia el WLC falla, un AP FlexConnect es capaz de conmutar (switch) el tráfico de datos de los clientes directamente en la red local de la sucursal y realizar autenticaciones locales. Esto garantiza que la sucursal mantenga conectividad a sus recursos locales (como impresoras o servidores de archivos) incluso si se pierde la conexión con el controlador central.

---

