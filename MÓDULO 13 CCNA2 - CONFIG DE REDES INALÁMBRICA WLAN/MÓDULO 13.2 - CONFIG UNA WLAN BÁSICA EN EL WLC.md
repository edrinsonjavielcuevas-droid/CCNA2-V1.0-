
---

A diferencia de los routers domésticos, en las redes empresariales la configuración inalámbrica no se realiza en los Puntos de Acceso (APs) individuales, sino de forma centralizada a través de un Controlador de LAN Inalámbrica (WLC). A continuación se detallan los pasos y conceptos para configurar una WLAN básica desde la interfaz gráfica del WLC.

---
### Topología WLC

Para comprender la configuración, primero hay que entender la topología lógica y física:

**Conexión Física:** El WLC suele conectarse a un switch de distribución o núcleo a través de un enlace troncal (Trunk), ya que necesita manejar el tráfico de múltiples VLANs (una para administración y varias para las distintas redes Wi-Fi de los usuarios). Los APs se conectan a switches de acceso.

**Interfaces Lógicas del WLC:**

**Management Interface:** Se usa para administrar el WLC (vía HTTPS o SSH) y para la comunicación CAPWAP inicial con los APs.

**Virtual Interface:** Utilizada para la gestión de la movilidad (roaming) y los portales cautivos.

**Dynamic Interfaces:** Son interfaces creadas por el administrador que se mapean directamente a las VLANs de la red cableada. Cada nueva WLAN (SSID) que se crea, se asocia a una de estas interfaces dinámicas.

----
### Iniciar sesión en el WLC

El acceso al WLC se realiza a través de un navegador web.

**Proceso:** Se ingresa la dirección IP de la _Management Interface_ mediante HTTPS (ej. `[https://192.168.200.254](https://192.168.200.254)`).

**Panel inicial:** Tras introducir las credenciales de administrador, el WLC muestra un panel de control (Dashboard) resumido. Este panel ofrece una vista rápida del estado general de la red, mostrando el número de APs conectados, clientes activos, rogues (APs no autorizados) y el uso del ancho de banda.

---
### Ver la información del punto de acceso

Antes de crear una red para los usuarios, es importante verificar que los APs se hayan comunicado correctamente con el controlador mediante el protocolo CAPWAP.

**Verificación:** Dentro de la GUI, al navegar a la pestaña **Wireless** (Inalámbrico), se despliega una lista de todos los APs que se han unido al WLC.

**Detalles:** Desde allí puedes ver el nombre del AP, su dirección IP, la dirección MAC, su estado operativo y el modelo físico del equipo. Si un AP no aparece aquí, no podrá emitir ninguna red Wi-Fi.

---
### Configuración Avanzada

El panel inicial es útil para el monitoreo, pero para realizar configuraciones profundas se requiere la vista avanzada.

**Acceso:** Se debe hacer clic en el botón **"Advanced"** (Avanzado), generalmente ubicado en la esquina superior derecha del Dashboard.

**Estructura:** Esto cambia la interfaz a la vista clásica del WLC de Cisco, habilitando un menú superior completo con pestañas esenciales como _Monitor_, _WLANs_, _Controller_, _Wireless_, _Security_ y _Management_.

---
### Configurar una WLAN

Este es el proceso central para crear una nueva red Wi-Fi que los usuarios puedan ver y utilizar. Se realiza desde la pestaña **WLANs** en la vista avanzada:

**Crear nueva:** Seleccionar "Create New" y presionar "Go".

**Parámetros Generales:**

**Profile Name:** Un nombre descriptivo para la administración interna.

**SSID:** El nombre público de la red que verán los usuarios en sus dispositivos.

**ID:** Un número de identificación único para esta WLAN.

**Configuración de la WLAN (Edit):**

**General:** Se debe marcar la casilla **"Status: Enabled"** para encender la red. Aquí también se mapea la WLAN a la interfaz dinámica (VLAN) correspondiente en el campo _Interface/Interface Group_.

**Security (Seguridad):** En la subpestaña _Layer 2_, se define el protocolo (ej. WPA+WPA2), el algoritmo de cifrado (AES) y el método de autenticación (ej. PSK para introducir la contraseña compartida, o 802.1X para autenticación Enterprise con RADIUS).

**Aplicar:** Guardar los cambios. En este momento, el WLC envía la instrucción a todos los APs a través del túnel CAPWAP para que comiencen a emitir el nuevo SSID.

---

