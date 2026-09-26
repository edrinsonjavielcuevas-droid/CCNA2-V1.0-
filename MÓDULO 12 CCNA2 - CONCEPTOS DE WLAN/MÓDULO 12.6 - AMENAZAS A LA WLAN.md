
---

A diferencia de las redes cableadas donde un atacante necesita acceso físico a un switch o un cable, las redes inalámbricas utilizan el aire como medio de transmisión. Esto las hace inherentemente más vulnerables a diversas amenazas de seguridad, ya que cualquier persona con la herramienta adecuada puede interceptar la señal.

### Resumen de seguridad inalámbrica

Para asegurar una WLAN, es vital comprender que la cobertura de la señal no se detiene en las paredes del edificio de la empresa.

Interceptación: Los datos transmitidos sin cifrar (o con protocolos débiles como WEP) pueden ser capturados fácilmente por atacantes utilizando software de rastreo (sniffers) de paquetes inalámbricos.

Intrusión: Si la red no requiere una autenticación robusta, usuarios no autorizados pueden conectarse a la red corporativa desde el estacionamiento o la calle, accediendo a recursos internos.

Mitigación básica: Se requiere implementar protocolos de cifrado fuertes (como WPA2 o WPA3) y métodos de autenticación centralizados (como 802.1x con servidores RADIUS) para proteger el tráfico.

---

### Ataques de DoS (Denegación de Servicio)

Los ataques DoS en redes inalámbricas buscan impedir que los usuarios legítimos puedan acceder a los recursos de la red. Pueden ocurrir de varias formas:

**Interferencia Accidental:** Dispositivos que operan en las mismas bandas (especialmente 2.4 GHz), como hornos microondas, teléfonos inalámbricos o monitores de bebés, pueden saturar el canal e interrumpir la comunicación.

**Interferencia Intencional (RF Jamming):** Un atacante utiliza un transmisor de radiofrecuencia para emitir ruido blanco y abrumar las frecuencias de la red Wi-Fi, inutilizando los Puntos de Acceso.

**Ataques de Desautenticación (Deauth):** El atacante falsifica tramas de administración 802.11 (específicamente tramas de _Deauthentication_) haciéndose pasar por el Punto de Acceso. Esto obliga a los clientes inalámbricos a desconectarse continuamente de la red.

---
### Puntos de acceso no autorizados (Rogue APs)

Un _Rogue AP_ es un Punto de Acceso inalámbrico que ha sido conectado a la red corporativa sin la autorización explícita del departamento de TI.

**Origen común:** A menudo son instalados por empleados bien intencionados que traen un router de su casa para tener mejor señal Wi-Fi en su escritorio.

**El peligro:** Al conectar este AP no seguro a un puerto de pared de la empresa, se crea una "puerta trasera". Un atacante externo puede conectarse al Rogue AP (que no tiene las contraseñas ni la seguridad de la empresa) y obtener acceso directo a la red cableada interna, eludiendo por completo el firewall perimetral y los APs legítimos.

**Mitigación:** Se utilizan Sistemas de Prevención de Intrusiones Inalámbricas (WIPS) y políticas de seguridad de puertos (Port Security) en los switches para detectar y bloquear estos dispositivos.

---
### Ataque man-in-the-middle (MitM)

En el contexto inalámbrico, el ataque _Man-in-the-Middle_ más común se conoce como **Evil Twin** (Gemelo Malvado).

**Mecanismo:** El atacante configura su propia computadora o un Punto de Acceso malicioso para que transmita exactamente el mismo nombre de red (SSID) y, a menudo, la misma dirección MAC que el Punto de Acceso legítimo de la empresa o de un lugar público (como un café).

**Impacto:** El atacante emite la señal con mayor potencia para que los dispositivos de las víctimas se conecten a su AP falso en lugar del real. Una vez conectados, todo el tráfico de internet de la víctima (contraseñas, correos, navegación) pasa a través de la máquina del atacante antes de salir a internet, permitiéndole interceptar y robar información confidencial.

----

