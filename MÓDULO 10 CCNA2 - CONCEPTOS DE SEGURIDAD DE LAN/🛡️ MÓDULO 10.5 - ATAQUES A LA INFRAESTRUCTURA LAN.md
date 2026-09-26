
---

Los atacantes abusan de protocolos específicos para comprometer la infraestructura de switching:

### 1. Ataques de Salto de VLAN (VLAN Hopping)

Permite a un atacante enviar tráfico a redes virtuales restringidas.

**Switch Spoofing:** El atacante configura su equipo para emitir mensajes DTP, negociando un enlace troncal con el switch y obteniendo acceso a todas las VLANs.

**Double Tagging:** El atacante oculta una segunda etiqueta 802.1Q maliciosa en la trama. El switch descarta la primera etiqueta al enviarla por la VLAN nativa, provocando que el siguiente switch reenvíe el tráfico a la VLAN objetivo.

**Mitigación:** Desactivar DTP (`switchport nonegotiate`), configurar puertos de usuario estáticamente como acceso y cambiar la VLAN nativa a un número no utilizado.

### 2. Ataques DHCP

**Agotamiento (Starvation):** Inundar el servidor DHCP con cientos de peticiones falsas para agotar todas las direcciones IP disponibles, causando una denegación de servicio (DoS) para usuarios legítimos.

**Spoofing:** El atacante conecta un servidor DHCP falso que responde más rápido que el legítimo, entregando una configuración envenenada (ej. cambiando la IP del Gateway para interceptar el tráfico mediante _Man-in-the-Middle_).

**Mitigación:** Configurar **DHCP Snooping** para clasificar los puertos del switch como confiables (solo para servidores reales) y no confiables.

### 3. Ataques ARP (ARP Spoofing / Poisoning)

**Mecanismo:** El atacante envía proactivamente mensajes de respuesta ARP falsificados, asociando su propia dirección MAC con la dirección IP del Gateway predeterminado.

**Impacto:** Engaña la memoria caché ARP de todos los equipos. Todo el tráfico destinado a internet se envía físicamente a la computadora del atacante en lugar del router real.

**Mitigación:** Implementar **Dynamic ARP Inspection (DAI)**, que verifica los paquetes ARP cruzándolos con la base de datos segura creada por DHCP Snooping.

### 4. Ataques STP (Spanning Tree Protocol)

**Mecanismo:** El atacante inyecta mensajes de control STP (BPDU) falsificados con una prioridad extremadamente baja (cero).

**Impacto:** Engaña a la red para que su computadora sea elegida como el _Root Bridge_ (Puente Raíz), obligando a que enormes cantidades de tráfico se desvíen hacia él.

**Mitigación:** Configurar **BPDU Guard** (apaga inmediatamente el puerto si detecta un mensaje de switch) y **Root Guard** (impide que switches no autorizados asuman el rol de Puente Raíz).

---

