
---

La seguridad de puertos (Port Security) es una característica de Capa 2 en los switches Cisco que permite restringir la entrada a una interfaz limitando y controlando las direcciones MAC de las estaciones que pueden acceder a ese puerto.

### Asegurar los puertos sin utilizar

Una medida de seguridad fundamental en cualquier dispositivo de red es deshabilitar físicamente todos los puertos que no estén asignados o en uso activo.

**Comandos de configuración:** Se utiliza el comando `interface range` para seleccionar múltiples puertos a la vez y apagarlos.

```
Switch(config)# interface range FastEthernet 0/15 - 24

Switch(config-if-range)# shutdown
```

----
### Mitigación de ataques por saturación de tabla de direcciones MAC

La seguridad de puertos es el método principal para mitigar los ataques de _MAC Flooding_ (saturación de la tabla CAM). Al limitar el número de direcciones MAC permitidas en un puerto, se evita que un atacante inunde el switch con direcciones falsas, impidiendo que el switch entre en estado _fail-open_ (comportamiento de hub).

### Habilitar la seguridad del puerto

Para habilitar Port Security, la interfaz debe estar configurada primero como un puerto de acceso estático o un puerto troncal. No funciona en puertos dinámicos (DTP).

**Comandos de configuración:**

```
Switch(config)# interface FastEthernet 0/1

Switch(config-if)# switchport mode access

Switch(config-if)# switchport port-security
```

_(Nota: El comando `switchport port-security` por sí solo habilita la función con los valores por defecto: máximo 1 MAC, modo de violación Shutdown)._

----

### Limitar y Aprender MAC Addresses

Puedes controlar cuántas direcciones MAC se permiten y de qué manera el switch las aprende.

**Definir el número máximo de direcciones MAC permitidas:**

```
Switch(config-if)# switchport port-security maximum 3
```

**Aprender direcciones MAC (3 métodos):**

**Configuración estática:** El administrador define la MAC manualmente.

```
Switch(config-if)# switchport port-security mac-address 0011.2233.4455
```
   
**Configuración dinámica (por defecto):** El switch aprende la MAC dinámicamente y se guarda solo en la RAM (se pierde al reiniciar). No requiere comando adicional.

**Configuración persistente (Sticky):** El switch aprende la MAC dinámicamente, pero la añade automáticamente a la configuración en ejecución (running-config). Si guardas la configuración, sobrevive a reinicio.

```
Switch(config-if)# switchport port-security mac-address sticky
```

---
### Vencimiento de la seguridad del puerto (Aging)

El _aging_ (envejecimiento) se utiliza para eliminar direcciones MAC seguras (estáticas o dinámicas) de un puerto después de un tiempo, liberando espacio para nuevos dispositivos sin tener que intervenir manualmente.

**Tipos de Aging:**

**Absolute (Absoluto):** La MAC se elimina después del tiempo configurado, independientemente de si hay tráfico o no.

**Inactivity (Inactividad):** La MAC se elimina solo si no se ha detectado tráfico de ella durante el tiempo configurado.

**Comandos de configuración (ejemplo: 10 minutos por inactividad):**

```
Switch(config-if)# switchport port-security aging time 10

Switch(config-if)# switchport port-security aging type inactivity
```

---
### Seguridad de puertos - Modos de violación de seguridad

Cuando se conecta una MAC no autorizada o se supera el límite máximo configurado, se produce una violación. Existen tres modos de respuesta:

**Protect (Proteger):** Las tramas de la MAC desconocida se descartan. No se genera ningún mensaje de registro (syslog) ni se incrementa el contador de violaciones.

**Restrict (Restringir):** Las tramas se descartan, se incrementa el contador de violaciones y se genera un mensaje syslog/SNMP.

**Shutdown (Apagar - Valor por defecto):** El puerto se apaga inmediatamente (entra en estado _err-disabled_), se genera un mensaje syslog y se incrementa el contador.

**Comando de configuración:**

```
Switch(config-if)# switchport port-security violation {protect | restrict | shutdown}
```

----

### Puertos en Estado error-disabled

Cuando un puerto entra en estado `err-disabled` debido a una violación de seguridad, el puerto se apaga lógicamente y el LED de enlace se apaga.

**Para recuperar el puerto manualmente:** Debes apagar la interfaz administrativamente y luego volver a encenderla.

```
Switch(config-if)# shutdown

Switch(config-if)# no shutdown
```

_(Opcional) Recuperación automática:_ Puedes configurar el switch para que intente recuperar los puertos err-disabled automáticamente después de un tiempo definido.

```
Switch(config)# errdisable recovery cause psecure-violation

 Switch(config)# errdisable recovery interval 300
```

---
### Verificar la seguridad del puerto

Comandos clave para verificar que la configuración se aplicó correctamente y monitorear su estado:

**Ver un resumen de la seguridad en todos los puertos:**
```
 Switch# show port-security
```


**Ver los detalles de seguridad de una interfaz específica:**

```
Switch# show port-security interface FastEthernet 0/1
```

_(Muestra el estado del puerto, el modo de violación, el conteo máximo de MACs, y el contador de violaciones)._

**Ver todas las direcciones MAC seguras aprendidas y estáticas:**

```
Switch# show port-security address
```

----

