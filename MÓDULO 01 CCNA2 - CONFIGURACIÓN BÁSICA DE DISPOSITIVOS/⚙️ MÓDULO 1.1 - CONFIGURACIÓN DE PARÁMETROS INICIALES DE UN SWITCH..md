
---
### SECUENCIA DE ARRANQUE DE UN SWITCH.

Antes de poder configurar un switch, debe encenderlo y permitirle pasar por la secuencia de arranque de cinco pasos. 

Después de encender un switch Cisco, pasa por la siguiente secuencia de inicio de cinco pasos:

**Paso 1:** Primero, el switch carga un programa de autodiagnóstico al encender (POST) almacenado en la memoria ROM. El POST verifica el subsistema de la CPU. Este comprueba la CPU, la memoria DRAM y la parte del dispositivo flash que integra el sistema de archivos flash.  
  
**Paso 2:** A continuación, el switch carga el software del cargador de arranque. El cargador de arranque es un pequeño programa almacenado en la memoria ROM que se ejecuta inmediatamente después de que el POST se completa correctamente.  
  
**Paso 3:** El cargador de arranque lleva a cabo la inicialización de la CPU de bajo nivel. Inicializa los registros de la CPU, que controlan dónde está asignada la memoria física, la cantidad de memoria y su velocidad. 
  
**Paso 4:** El cargador de arranque inicia el sistema de archivos flash en la placa del sistema.  
  
**Paso 5:** Por último, el cargador de arranque localiza y carga una imagen de software del sistema operativo de IOS en la memoria y delega el control del switch a IOS.

---- 

### EL COMANDO BOOT SYSTEM.

**Comportamiento predeterminado:** Si no se establece la variable de entorno BOOT, el switch intenta cargar y ejecutar el primer archivo ejecutable que puede encontrar.

**Inicialización:** El sistema operativo IOS luego inicializa las interfaces utilizando el archivo de configuración de inicio (el archivo _startup-config_ se llama **config.text** y se encuentra en flash).

**Configuración manual:** La variable de entorno BOOT se establece mediante el comando **`boot system`** desde el modo de configuración global, donde se especifica la ruta exacta del archivo (Ejemplo: `S1(config)# boot system flash:/ruta/archivo.bin`).

**Verificación:** Use el comando **`show boot`** para ver en qué está configurado el archivo de arranque IOS actual.

La  siguiente tabla define cada parte del comando **boot system**.

![](PARTES%20DEL%20COMANDO%20BOOT%20SYSTEM.png)

---
### INDICADORES LED DEL SWITCH.

![](INDICADORES%20LED%20DEL%20SWITCH.png)

---

**1. LED del Sistema (SYST)**

**Apagado**: El switch no está recibiendo energía o está apagado.

**Verde**: El dispositivo recibe energía y funciona correctamente.

**Ámbar**: Hay un fallo o el switch no está recibiendo alimentación adecuadamente.

---

**2. Sistema de Alimentación Redundante (RPS)**

**Apagado**: El sistema RPS está apagado o no está conectado correctamente.

**Verde**: Está conectado, funciona de manera correcta y está listo para proporcionar energía de respaldo.

**Verde intermitente**: Está conectado, pero actualmente se encuentra alimentando a otro dispositivo.

**Ámbar**: El RPS está en modo de reserva o presenta algún fallo.

**Ámbar intermitente**: La fuente de alimentación interna del switch falló y el sistema RPS ha entrado a alimentar el switch.

---

**3. Estado del Puerto (STAT)**

**Apagado**: No hay enlace; el puerto está inactivo.

**Verde**: El enlace está correcto.

**Verde intermitente**: El puerto está enviando y recibiendo datos con normalidad.

**Parpadeo alterno (verde/ámbar)**: Se ha detectado un fallo en el enlace.

**Ámbar**: El puerto está bloqueado para evitar bucles en el dominio de reenvío.

**Ámbar intermitente**: El puerto está bloqueado para evitar que se forme un bucle en la red.

---

**4. Modo Dúplex (DUPLX)**

**Verde**: El puerto está operando en modo _Full-Duplex_ (puede enviar y recibir datos simultáneamente).

---

**5. Velocidad (SPEED)**

**Apagado**: Transmitiendo a una velocidad de 10 Mbps.

**Verde**: Transmitiendo a una velocidad de 100 Mbps.

**Verde intermitente**: Transmitiendo a una velocidad de 1000 Mbps (1 Gbps).

---

**6. Alimentación por Ethernet (PoE)**

**Apagado**: No se provee PoE a ningún puerto; el sistema funciona con normalidad sin fallos de energía.

**Ámbar intermitente**: El PoE no está seleccionado globalmente, pero a al menos un puerto se le ha denegado la alimentación o tiene un fallo.

**Verde**: El modo PoE está seleccionado.

---
### RECUPERARSE DE UN BLOQUEO DEL SISTEMA. 

(Uso del Cargador de Arranque)
#### ¿Para qué sirve?

El cargador de arranque (boot loader) es una herramienta de rescate. Te permite acceder a la línea de comandos básica del switch y a la memoria flash cuando el sistema operativo (IOS) principal está dañado, corrupto o ha sido eliminado.

**Pasos para acceder al Cargador de Arranque:**

**Paso 1: Preparar la conexión.** Conecta tu computadora al puerto de consola del switch usando el cable correspondiente y abre tu software de emulación de terminal.

**Paso 2: Corte de energía.** Desconecta el cable de alimentación del switch.

**Paso 3: Interrupción del arranque.** Vuelve a conectar la energía. En los primeros 15 segundos, **mantén presionado el botón Mode** (el LED del sistema estará parpadeando en verde).

**Paso 4: Confirmación visual.** Sigue presionando el botón Mode hasta que el LED del sistema se ponga brevemente en color ámbar y luego pase a **verde fijo**. En ese momento, suelta el botón.

**Paso 5: Acceso exitoso.** En la pantalla de tu terminal aparecerá el prompt especial del cargador de arranque: `switch:`

---

#### Comandos dentro del modo `switch:`

**`help`** o **`?`**: Muestra la lista de todos los comandos que tienes disponibles en este modo de rescate.

**`set`**: Muestra el estado actual de la variable de entorno BOOT (te dice qué archivo está intentando cargar el switch por defecto).

![](COMANDO%20SET.png)

**`flash_init`**: Inicializa el sistema de archivos flash. 

>**Nota importante:** Debes ejecutar este comando primero si deseas interactuar con los archivos y ver qué hay guardado en la memoria flash del equipo.

Después de que flash haya terminado de inicializar, puede ingresar el **dir flash:** comando para ver los directorios y archivos en flash, como se muestra en la salida.

![](COMANDO%20DIR%20FLASH.png)

Introduzca el **BOOT=flash** comando para cambiar la ruta de la variable de entorno BOOT que utiliza el switch para cargar el nuevo IOS en flash. Para verificar la nueva ruta de la variable de entorno BOOT, vuelva a **set** ejecutar el comando. Finalmente, para cargar el nuevo IOS escriba el **boot** comando sin ningún argumento, como se muestra en la salida.

![](VARIABLE%20BOOT.png)

Los comandos del gestor de arranque admiten la inicialización de flash, el formateo de flash, la instalación de un nuevo IOS, el cambio de la variable de entorno BOOT y la recuperación de contraseñas pérdidas u olvidadas.

---
### ACCESO A ADMINISTRACIÓN DE SWITCHES.

**Requisito remoto:** Para administrar un switch a distancia, necesita una dirección IP y máscara de subred.

**SVI (Interfaz Virtual):** La IP se configura en la SVI, no en un puerto físico.

**Puerta de enlace:** Solo es necesaria si lo administras desde una red distinta.

**Acceso inicial:** La primera configuración debe hacerse conectando una PC con un cable de consola.

![](CONFIG%20SVI.png)

---

### EJEMPLO DE CONFIGURACIÓN DE SWITCH.

De manera predeterminada, el switch está configurado para controlar su administración a través de la VLAN 1. Por motivos de seguridad, se considera una práctica recomendada utilizar una VLAN distinta de la VLAN 1 para la VLAN de administración, como la VLAN 99 en el ejemplo.

### Paso 1: Configuración de la Interfaz de administración

Desde el modo de configuración de la interfaz VLAN, se aplica una dirección IPv4 y una máscara de subred a la SVI de administración del switch.

>**Nota:** El SVI para VLAN 99 no aparecerá como "activo / activo" hasta que se cree VLAN 99 y haya un dispositivo conectado a un puerto de switch asociado con VLAN 99.

>**Nota:** Es posible que el switch deba configurarse para IPv6. Por ejemplo, antes de que pueda configurar el direccionamiento IPv6 en un Cisco Catalyst 2960 que ejecute IOS versión 15.0, deberá ingresar el comando de configuración global `sdm prefer dual-ipv4-and-ipv6 default` y, a continuación, `reload` el switch.

| **Tarea**                                                           | **Comandos IOS**                                       |
| ------------------------------------------------------------------- | ------------------------------------------------------ |
| Ingrese al modo de configuración global.                            | `S1# configure terminal`                               |
| Ingrese al modo de configuración de interfaz para la SVI.           | `S1(config)# interface vlan 99`                        |
| Configure la dirección IPv4 de la interfaz de administración.       | `S1(config-if)# ip address 172.17.99.11 255.255.255.0` |
| Configure la dirección IPv6 de la interfaz de administración.       | `S1(config-if)# ipv6 address 2001:db8:acad:99::1/64`   |
| Habilite la interfaz de administración.                             | `S1(config-if)# no shutdown`                           |
| Vuelva al modo EXEC privilegiado.                                   | `S1(config-if)# end`                                   |
| Guarde la configuración en ejecución en la configuración de inicio. | `S1# copy running-config startup-config`               |

---
### Paso 2: Configuración del gateway predeterminado

Si el switch se va a administrar de forma remota desde redes que no están conectadas directamente, se debe configurar con un gateway predeterminado.

>**Nota:** Dado que recibirá la información de la puerta de enlace predeterminada de un mensaje de anuncio de router (RA), el switch no requiere una puerta de enlace predeterminada IPv6.

|**Tarea**|**Comandos IOS**|
|---|---|
|Ingrese al modo de configuración global.|`S1# configure terminal`|
|Configure el gateway predeterminado para el switch.|`S1(config)# ip default-gateway 172.17.99.1`|
|Vuelva al modo EXEC privilegiado.|`S1(config-if)# end`|
|Guarde la configuración en ejecución en la configuración de inicio.|`S1# copy running-config startup-config`|

---
### Paso 3: Verificar la configuración

Los comandos `show ip interface brief` y `show ipv6 interface brief` son útiles para determinar el estado de las interfaces físicas y virtuales. La información que se muestra 
confirma que la interfaz VLAN 99 se ha configurado con una dirección IPv4 e IPv6.

> **Nota:** Una dirección IP aplicada al SVI es solo para el acceso de administración remota al switch; esto no permite que el switch enrute paquetes de Capa 3.

![](SHOW%20IP%20INTER%20BRIEF.png)

---

