
----
### Dos funciones del router

Un router es un dispositivo de Capa 3 cuya responsabilidad principal es mover datos entre diferentes redes. Para lograr esto, realiza dos funciones fundamentales:

**Determinación de la ruta (Enrutamiento):** Utiliza su tabla de enrutamiento para evaluar la dirección IP de destino de un paquete entrante y decidir cuál es el mejor camino (trayecto) para llegar a esa red.

**Reenvío de paquetes (Conmutación):** Una vez que determina la ruta, el router mueve el paquete desde la interfaz de entrada (por donde llegó) hacia la interfaz de salida correcta. Este proceso implica desencapsular y volver a encapsular el paquete en una nueva trama de enlace de datos adecuada para el siguiente salto.

---
### Ejemplo de Funciones del router

El flujo de trabajo estándar cuando un router recibe un paquete es el siguiente:

El router recibe una trama de Capa 2 y verifica la secuencia de verificación de trama (FCS) para detectar errores. Si está bien, extrae el paquete IP de Capa 3.

El router lee la dirección IP de destino del paquete.

Busca en su tabla de enrutamiento una coincidencia para esa red de destino.

Si encuentra una coincidencia, encapsula el paquete IP en una nueva trama de Capa 2 con la dirección MAC (o el identificador de Capa 2 correspondiente) del siguiente salto o del dispositivo final.

Envía la trama por la interfaz de salida.

---
### Mejor ruta es igual a la coincidencia más larga

Cuando un router busca la dirección IP de destino en su tabla de enrutamiento, puede encontrar múltiples rutas que incluyan esa dirección. La regla universal que utilizan los routers para desempatar se conoce como la **coincidencia más larga (Longest Prefix 
Match)**

El router elegirá la ruta que tenga la mayor cantidad de bits de red idénticos a la dirección IP de destino.

En términos prácticos, esto significa que el router siempre preferirá la ruta con la **máscara de subred (o longitud de prefijo) más larga**, ya que representa la ruta más específica hacia el destino.

---
### Ejemplo de coincidencia más larga de direcciones IPv4

Supongamos que un router recibe un paquete destinado a la IP `192.168.1.50`. Su tabla de enrutamiento tiene estas tres rutas:

1. `192.168.0.0 /16` (Coinciden los primeros 16 bits)
    
2. `192.168.1.0 /24` (Coinciden los primeros 24 bits)
    
3. `192.168.1.32 /27` (Coinciden los primeros 27 bits)
    

Aunque el paquete "cabe" dentro de las tres subredes, el router elegirá la ruta **3 (`192.168.1.32 /27`)** porque tiene la coincidencia más larga (/27), siendo la instrucción más específica.

---

### Ejemplo de coincidencia más larga de direcciones IPv6

El mismo principio se aplica exactamente igual para IPv6. Si un paquete va dirigido a `2001:db8:acad:1::10`, y el router tiene estas rutas:

1. `2001:db8:: /32`
    
2. `2001:db8:acad:: /48`
    
3. `2001:db8:acad:1:: /64`
    

El router elegirá la ruta **3 (`2001:db8:acad:1:: /64`)** porque tiene la mayor cantidad de bits coincidentes (/64).

---

### Creación de la tabla de enrutamiento

Para que el router pueda tomar estas decisiones, necesita construir y mantener su tabla de enrutamiento. Aprende sobre las redes de tres maneras principales:

**Redes conectadas directamente:** Se agregan automáticamente a la tabla cuando se configura una dirección IP en una interfaz del router y se activa (`no shutdown`).

**Rutas estáticas:** El administrador de red configura manualmente la ruta hacia una red de destino específica. Son útiles en redes pequeñas o para crear rutas predeterminadas.

**Protocolos de enrutamiento dinámico:** Protocolos como OSPF, EIGRP o BGP permiten que los routers intercambien información entre sí automáticamente, aprendiendo sobre redes remotas y ajustando las rutas si la topología cambia (por ejemplo, si se cae un enlace).

---

