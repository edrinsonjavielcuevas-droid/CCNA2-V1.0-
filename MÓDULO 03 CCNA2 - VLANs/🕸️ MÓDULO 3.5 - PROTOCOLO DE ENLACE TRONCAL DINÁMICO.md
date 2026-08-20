
---
### INTRODUCCIÓN A DTP

**¿Qué es?:** DTP (Protocolo de Enlace Troncal Dinámico) es un protocolo **exclusivo de Cisco** que permite negociar automáticamente la formación de un enlace troncal entre dispositivos vecinos. Solo funciona en conexiones de punto a punto.

**Estado por defecto:** Viene habilitado de manera automática en switches como los Catalyst 2960 y 3560 (y 3650). La configuración DTP predeterminada para estos equipos es **automática dinámica** (_dynamic auto_).

**Compatibilidad:** Los switches de otros fabricantes **no admiten DTP**.

**Regla de Precaución:** Los dispositivos que no soportan DTP pueden llegar a reenviar las tramas DTP de manera incorrecta, provocando errores de configuración en la red. Como regla general, se debe desactivar DTP en las interfaces conectadas a equipos de otros proveedores.

**Configuración manual (Desactivación):** Para establecer un enlace troncal hacia un dispositivo que no admite DTP, debes usar los comandos `switchport mode trunk` seguido de `switchport nonegotiate`. Esto fuerza a la interfaz a convertirse en un tronco, pero detiene por completo la generación de tramas DTP.

![](../CCNA2IMG/IMGM3/Desactivaci'on%20de%20DTP.png)

Para volver a habilitar el protocolo de enlace troncal dinámico, utilice el **switchport mode dynamic auto** comando.

![](../CCNA2IMG/IMGM3/switchport%20mode%20dynamic%20auto.png)

### Comportamiento y configuración de DTP

**Troncal estricto:** Al combinar los comandos `switchport mode trunk` y `switchport nonegotiate`, el puerto ignora cualquier anuncio DTP y se queda fijo como troncal.

**Falla de negociación:** Si ambos extremos del enlace se dejan en modo _automático dinámico_, ninguno tomará la iniciativa para negociar. Ambos se quedarán en modo de acceso y el enlace troncal quedará inactivo.

**Mejor práctica (Cero ambigüedad):** Para configurar un troncal de forma segura y definitiva, usa siempre el comando `switchport mode trunk`. Esto garantiza que el enlace esté siempre activo, sin depender de lo que haga el otro switch.

---
### MODOS DE INTERFAZ NEGOCIADOS 

### Opciones del comando switchport mode

**Propósito:** El comando `switchport mode` cuenta con diferentes opciones que permiten forzar o negociar el modo de funcionamiento de una interfaz de red.

**Sintaxis:** La estructura completa del comando en el modo de configuración de interfaz es la siguiente:

Switch(config-if)# switchport mode { access | dynamic { auto | desirable } | trunk }

### Opciones del comando `switchport mode`

| **Opción**            | **Descripción**                                                                                                                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **access**            | • Pone la interfaz en modo permanente de _no trunking_ y negocia para convertir el enlace en no troncal.<br><br>• Asume este rol de acceso independientemente de si la interfaz vecina es troncal o no.            |
| **dynamic auto**      | • Es el modo predeterminado para todas las interfaces Ethernet.<br><br>• El puerto espera pasivamente; solo se convierte en troncal si la interfaz vecina está en modo troncal (_trunk_) o deseable (_desirable_). |
| **dynamic desirable** | • Hace que el puerto tome la iniciativa e intente convertir activamente el enlace en troncal.<br><br>• Logra ser troncal si el vecino está configurado en automático (_auto_), deseable o troncal.                 |
| **trunk**             | • El puerto queda en modo troncal de forma permanente y negocia para que el enlace se convierta en conexión troncal.<br><br>• Asume el rol troncal incluso si la interfaz vecina no lo es.                         |

### Desactivar la negociación DTP

**Comando:** `switchport nonegotiate`.

**Función:** Detiene la participación del switch en la negociación DTP para esa interfaz específica.

**Condición:** Este comando **solo** se puede aplicar si el puerto ya está configurado estáticamente en modo **`access`** o **`trunk`**.

**Consideración:** Si desactivas DTP en un enlace troncal, estás obligado a configurar manualmente el puerto del dispositivo vecino como troncal para que la conexión funcione.

---
### Resultados de una configuración DTP

**Mejor práctica:** Siempre que sea posible, se recomienda configurar los enlaces troncales **de manera estática** en lugar de depender de la negociación DTP.

La siguiente tabla ilustra qué tipo de enlace resulta al combinar diferentes configuraciones DTP en los extremos opuestos de una conexión:

| **Configuración en Extremos** | **Dinámico automático** | **Dinámico deseado (desirable)** | **Troncal (Trunk)**     | **Acceso (Access)**     |
| ----------------------------- | ----------------------- | -------------------------------- | ----------------------- | ----------------------- |
| **Dinámico automático**       | Acceso                  | Troncal                          | Troncal                 | Acceso                  |
| **Dinámico deseado**          | Troncal                 | Troncal                          | Troncal                 | Acceso                  |
| **Troncal**                   | Troncal                 | Troncal                          | Troncal                 | _Conectividad limitada_ |
| **Acceso**                    | Acceso                  | Acceso                           | _Conectividad limitada_ | Acceso                  |

---
### VERIFICACIÓN DEL MODO DTP

**Modo predeterminado:** El modo DTP que viene por defecto varía dependiendo de la plataforma y de la versión del software Cisco IOS.

**Comando de verificación:** Para determinar cuál es el modo DTP actual en una interfaz, debes ejecutar el comando `show dtp interface`.

**Regla para troncales:** Cuando se requiere un enlace troncal, la mejor práctica general es configurarlo manualmente estableciendo la interfaz en `trunk` y aplicando `nonegotiate`.

**Seguridad:** Se debe inhabilitar DTP obligatoriamente en todos aquellos enlaces donde no se deban usar enlaces troncales.

![](../CCNA2IMG/IMGM3/Show%20dtp%20inter.png)

---












