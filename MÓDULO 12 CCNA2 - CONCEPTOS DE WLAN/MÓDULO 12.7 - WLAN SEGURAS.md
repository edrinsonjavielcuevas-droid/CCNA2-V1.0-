
---

Para asegurar tu bóveda de Obsidian con los conceptos de redes para tu estudio, aquí tienes el desarrollo detallado de la sección sobre la seguridad en las redes inalámbricas (WLAN). Esta sección es crucial porque las ondas de radio viajan por el espacio libre, lo que obliga a implementar protocolos robustos para evitar la interceptación y el acceso no autorizado.

### Encubrimiento SSID y filtrado de direcciones MAC

Históricamente, los administradores usaban estas dos técnicas para ocultar la red, pero hoy en día **no se consideran métodos de seguridad reales**, sino medidas disuasorias básicas:

**Encubrimiento de SSID (SSID Cloaking):** Consiste en configurar el Punto de Acceso (AP) para que deje de emitir el nombre de la red en sus tramas _Beacon_.

_Falla de seguridad:_ Aunque la red no aparezca en la lista de Wi-Fi, el SSID sigue viajando en texto plano cuando un cliente legítimo se asocia (en los _Probe Requests/Responses_). Un atacante con un _sniffer_ de paquetes (como Wireshark) puede descubrir el nombre oculto en segundos.

**Filtrado de direcciones MAC:** Se configura una lista blanca en el AP o router permitiendo el acceso solo a direcciones MAC específicas.

_Falla de seguridad:_ Las direcciones MAC se transmiten sin cifrar. Un atacante solo necesita escuchar el tráfico, copiar (clonar) la dirección MAC de un cliente legítimo autorizado y suplantarlo (MAC Spoofing) para saltarse este filtro.

---
### Métodos de autenticación originales

El estándar 802.11 original definía dos mecanismos básicos, ambos obsoletos hoy en día:

**Sistema abierto (Open System):** No requiere contraseña. Cualquier cliente puede asociarse al AP. Se usa comúnmente en redes públicas (cafeterías, aeropuertos), pero todo el tráfico viaja sin cifrar.

**WEP (Wired Equivalent Privacy):** Fue el primer intento de asegurar las WLAN. Utilizaba el algoritmo de cifrado RC4 con una clave estática. Se descubrió que tenía fallas matemáticas graves y su cifrado puede romperse en minutos con herramientas como _Aircrack-ng_. **Nunca debe usarse en la actualidad.**

---
### Métodos de autenticación de clave compartida

Tras el fracaso de WEP, la Wi-Fi Alliance introdujo los protocolos WPA (Wi-Fi Protected Access). En entornos domésticos o pequeñas empresas, estos utilizan una clave precompartida.

**WPA:** Una solución temporal que reemplazó a WEP. Aún usaba el algoritmo RC4 pero introdujo TKIP para cambiar las claves dinámicamente. Ya está obsoleto.

**WPA2:** El estándar de la industria durante muchos años. Introdujo el uso obligatorio del algoritmo de cifrado **AES** (Advanced Encryption Standard), el cual es altamente seguro y utilizado a nivel gubernamental.

**WPA3:** La generación más reciente, que soluciona vulnerabilidades estructurales de WPA2.

---
### Autenticando a un usuario doméstico

Para entornos SOHO (Small Office / Home Office), se utiliza el modo **Personal**.

**WPA2-Personal (o WPA2-PSK):** Utiliza una Clave Precompartida (PSK - Pre-Shared Key). Todos los usuarios de la red introducen exactamente la misma contraseña para conectarse.

_Vulnerabilidad principal:_ Está sujeto a ataques de diccionario o fuerza bruta fuera de línea si el atacante captura el _Handshake_ (el saludo de 4 vías) inicial entre el cliente y el AP. Además, si un empleado se va de la empresa, la contraseña debe cambiarse para todos.

---
### Métodos de encriptación

Es fundamental distinguir entre el protocolo de autenticación (WPA/WPA2) y el algoritmo matemático utilizado para cifrar los datos (encriptación):

**TKIP (Temporal Key Integrity Protocol):** Utilizado por WPA. Fue diseñado como un parche de software para los equipos que usaban WEP. Es vulnerable y Cisco recomienda no utilizarlo.

**AES (Advanced Encryption Standard):** Utilizado por WPA2 y WPA3. Emplea el protocolo CCMP para proporcionar cifrado de grado militar. Es el único método de cifrado recomendado actualmente para proteger el tráfico de datos.

---
### Autenticación en la empresa

Para redes corporativas más grandes y seguras, se utiliza el modo **Enterprise** (Empresarial).

**WPA2/WPA3-Enterprise:** En lugar de usar una contraseña única para todos, utiliza el estándar **802.1X**.

_Funcionamiento:_ Requiere un servidor de autenticación externo (como un servidor **RADIUS**). Cada usuario debe introducir credenciales individuales (su propio nombre de usuario y contraseña, o un certificado digital).

_Ventaja:_ Si un empleado deja la empresa, simplemente se desactiva su usuario en el servidor RADIUS, sin afectar al resto de la red.

---

### WPA3

Es el estándar de seguridad más moderno y aborda las debilidades de WPA2. Sus mejoras clave incluyen:

**WPA3-Personal (SAE):** Reemplaza el antiguo PSK por **SAE (Simultaneous Authentication of Equals)**. Esto hace que sea prácticamente imposible realizar ataques de diccionario fuera de línea. Aunque el atacante capture el tráfico, no puede adivinar la contraseña intentando miles de combinaciones en su computadora.

**WPA3-Enterprise:** Añade una suite de seguridad opcional de 192 bits para entornos que requieren una seguridad criptográfica extrema (gobiernos, finanzas).

**Redes Abiertas Mejoradas (OWE):** En redes públicas sin contraseña, WPA3 utiliza _Opportunistic Wireless Encryption_ (OWE) para cifrar individualmente el tráfico de cada usuario, evitando que otros usuarios en la misma cafetería puedan interceptar sus datos.

---

