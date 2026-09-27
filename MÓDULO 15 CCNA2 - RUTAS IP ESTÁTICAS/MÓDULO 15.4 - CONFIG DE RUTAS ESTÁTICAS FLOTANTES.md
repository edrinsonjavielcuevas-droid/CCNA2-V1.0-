
---

## Configuración de rutas estáticas flotantes

Las rutas estáticas flotantes son rutas de respaldo que permanecen ocultas e inactivas hasta que la ruta principal hacia el mismo destino falla. Son una solución sencilla para proporcionar redundancia, especialmente en conexiones WAN o hacia un proveedor de servicios.

### Rutas estáticas flotantes

**Concepto clave:** Para que una ruta actúe como respaldo (que "flote"), se le debe asignar manualmente una **Distancia Administrativa (AD) mayor** que la de la ruta principal.

**Funcionamiento:** El router siempre instala en su tabla de enrutamiento la ruta con la AD más baja. Si la ruta principal es estática (AD predeterminada de 1), la ruta flotante debe configurarse con una AD de 2 o superior. Si la ruta principal cae, el router la elimina de la tabla e instala automáticamente la ruta flotante para mantener la conectividad.

### Configure las Rutas Estáticas Flotantes IPv4 y IPv6

La configuración es idéntica a la de una ruta estática predeterminada o estándar, simplemente agregando el valor deseado de la Distancia Administrativa al final del comando.

**Configuración en IPv4:**

Por ejemplo, si la ruta principal va por `10.1.1.2`, configuramos el enlace de respaldo por `10.1.2.2` asignándole una AD de `5`.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.2.2 5
```

**Configuración en IPv6:**

Aplicando la misma lógica para el tráfico de respaldo en IPv6 con una AD de `5`:

```
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2 5
``` 

### Pruebe la ruta estática flotante

Para garantizar que la redundancia esté bien configurada y lista para entrar en acción, debes simular una falla en la red.

**Estado normal:** Ejecuta `show ip route`. La ruta flotante **no** debe estar en la tabla; solo debes ver la ruta principal activa.

**Simular la caída:** Ingresa a la interfaz de salida de la ruta principal y apágala con el comando `shutdown`.

**Comprobar la conmutación:** Ejecuta nuevamente `show ip route`. Ahora la ruta flotante (identificada por el siguiente salto de respaldo) debe aparecer instalada en la tabla.

**Validar el flujo:** Utiliza `ping` o `traceroute` para confirmar que los paquetes están saliendo exitosamente a través del nuevo enlace.

----

