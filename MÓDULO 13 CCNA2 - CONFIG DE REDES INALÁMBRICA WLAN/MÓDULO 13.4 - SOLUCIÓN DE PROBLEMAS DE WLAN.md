
---
## Solución de problemas de WLAN

La resolución de problemas en redes inalámbricas puede ser más compleja que en las redes cableadas debido a la naturaleza invisible del medio de transmisión (radiofrecuencia). A continuación, se detallan los enfoques y escenarios comunes para diagnosticar y solucionar fallos en una WLAN.

### Enfoques para la Solución de Problemas

Al igual que en las redes cableadas, se pueden aplicar modelos estructurados (ascendente, descendente o divide y vencerás), pero en WLANs es altamente efectivo comenzar por la **Capa 1 (Física)** y la **Capa 2 (Enlace de Datos)**.

**Verificación de Capa 1:** Confirmar que el Punto de Acceso (AP) tenga energía (revisar el inyector PoE o el switch) y que no haya fuentes masivas de interferencia electromagnética cerca.

**Verificación de Capa 2:** Asegurarse de que el AP esté transmitiendo el SSID correctamente y que el túnel CAPWAP hacia el Controlador de LAN Inalámbrica (WLC) esté operativo.

---
### Cliente Inalámbrico no está conectando

Si un host no puede asociarse a la red, el problema suele radicar en discrepancias de configuración entre el cliente y el AP. Los puntos clave a revisar son:

-**SSID incorrecto:** Verificar que el cliente intente conectarse al nombre de red exacto (es sensible a mayúsculas y minúsculas).

**Incompatibilidad de seguridad:** Confirmar que la contraseña precompartida (PSK) sea correcta o que las credenciales 802.1X del usuario sean válidas en el servidor RADIUS. También puede ocurrir que el AP exija WPA3 y la tarjeta de red del cliente sea antigua y solo soporte WPA2.

**Falla de direccionamiento:** Si el cliente se conecta pero muestra "Sin Internet" o recibe una dirección APIPA (`169.254.x.x`), el problema no es la señal de radio, sino que el servidor DHCP está inalcanzable o el _pool_ de direcciones está agotado.

---
### Resolución de Problemas cuando la red esta lenta

Cuando los usuarios están conectados pero experimentan alta latencia o bajas velocidades, los factores suelen ser ambientales o de saturación del medio:

**Interferencia de canal:** Ocurre si hay demasiados APs vecinos transmitiendo en el mismo canal (saturación) o en canales superpuestos. La solución es realizar un análisis del espectro y cambiar el AP a un canal menos congestionado (ej. usar 1, 6 u 11 en 2.4 GHz, o migrar usuarios a 5 GHz).

**Distancia y obstáculos:** Si el cliente está muy lejos del AP o hay paredes gruesas, la señal se debilita. El estándar 802.11 ajusta dinámicamente la modulación, reduciendo la velocidad para mantener la conexión.

**Dispositivos heredados (Legacy):** Un dispositivo muy antiguo (ej. 802.11b) en la red puede obligar al AP a consumir más tiempo de aire (Airtime) para transmitirle, ralentizando a los dispositivos modernos.

---
### Actualizar el Firmware

Mantener los equipos actualizados es una práctica de solución de problemas proactiva y reactiva.

**Propósito:** Las actualizaciones de firmware corrigen errores de software (bugs), mejoran la compatibilidad con nuevos clientes inalámbricos y, lo más importante, parchean vulnerabilidades de seguridad críticas.

**Proceso en la empresa:** En arquitecturas centralizadas, el administrador solo necesita actualizar el firmware del Controlador de LAN Inalámbrica (WLC). El WLC se encarga automáticamente de empujar (push) la actualización a todos los APs ligeros conectados a él durante su proceso de arranque.

---

**Routers Inalámbricos (SOHO):** Integran funciones de switch, puerto WAN y acceso Wi-Fi, utilizando DHCP para la asignación automática de direcciones. La prioridad inicial debe ser cambiar el usuario y contraseña predeterminados. También se utiliza NAT para convertir direcciones IPv4 privadas en públicas y QoS para priorizar el tráfico de voz y video.

**Administración Centralizada (WLC):** Los Puntos de Acceso Ligeros (LAP) utilizan el protocolo LWAPP para comunicarse con el Controlador WLAN (WLC). El WLC permite una gestión centralizada y avanzada del sistema a través de su interfaz.

**Servicios Empresariales:** Para redes como WPA2 Enterprise, el WLC se integra con SNMP para enviar registros (_traps_) de monitoreo de red, y con servidores RADIUS para manejar la autenticación, autorización y registro (AAA) de los usuarios.

**Solución de Problemas y Mantenimiento:** Se recomienda utilizar un proceso de eliminación de seis pasos para fallas de conectividad o rendimiento. Para mejorar el ancho de banda, se puede dividir el tráfico o actualizar los clientes inalámbricos, siendo crucial revisar y aplicar periódicamente las actualizaciones de firmware del fabricante para parchear vulnerabilidades y corregir errores.

---


