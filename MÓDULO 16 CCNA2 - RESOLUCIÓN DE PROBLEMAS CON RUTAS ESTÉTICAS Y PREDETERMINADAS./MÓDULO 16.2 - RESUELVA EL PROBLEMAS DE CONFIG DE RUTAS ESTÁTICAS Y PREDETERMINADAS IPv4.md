
---
## Resuelva problemas de configuración de rutas estáticas y predeterminadas IPv4

El enrutamiento estático requiere un mantenimiento riguroso. A diferencia de los protocolos dinámicos, las rutas estáticas no se actualizan automáticamente cuando hay fallas o cambios físicos, lo que hace que la resolución de problemas (troubleshooting) sea una habilidad indispensable.

### Cambios en la red

Las redes son entornos dinámicos; los enlaces fallan, los cables se desconectan y las topologías crecen.

**El problema estático:** Si un enlace configurado como el "siguiente salto" en una ruta estática se cae, el router no buscará un camino alternativo por su cuenta (a menos que hayas configurado una ruta flotante). El tráfico simplemente se descartará o se enviará hacia un "agujero negro".

**Intervención manual:** Cualquier cambio en la topología requiere que el administrador ingrese a los routers afectados, evalúe la nueva ruta óptima y reconfigure las rutas estáticas para restaurar la conectividad.

---
### Comandos comunes para la solución de problemas

Para aislar y diagnosticar un problema de enrutamiento estático, debes dominar este conjunto de herramientas (comandos IOS):

**`ping <ip-destino>`:** Es tu primera prueba. Verifica la conectividad de Capa 3 de extremo a extremo.
    
**`traceroute <ip-destino>`:** Si el ping falla, este comando es crucial. Te muestra exactamente en qué router (salto) se está perdiendo el paquete, indicándote dónde empezar a investigar.

**`show ip route`:** Verifica si la ruta estática realmente está instalada en la tabla de enrutamiento. (Si la interfaz de salida está caída, el router elimina automáticamente la ruta estática de la tabla).

**`show ip interface brief`:** Confirma que las interfaces locales necesarias para la ruta estén en estado _Up/Up_ (Capa 1 y Capa 2 operativas).

**`show cdp neighbors`:** Útil para confirmar si realmente estás conectado al router vecino correcto y por la interfaz correcta.

---

### Resolución de un problema de conectividad

La metodología estándar para resolver un fallo en una ruta estática sigue un proceso estructurado:

1. **Encontrar el fallo:** Usa `ping` y `traceroute` desde el origen para identificar qué router está descartando el tráfico.

2. **Inspeccionar el router problemático:** Ingresa al router donde se detuvo el _traceroute_ y revisa su tabla de enrutamiento (`show ip route`).

3. **Identificar el error:** Los errores más comunes al configurar rutas estáticas incluyen:

Escribir mal la red de destino o la máscara de subred.

 Apuntar hacia la IP del siguiente salto equivocada (ej. usar la IP de tu propia interfaz en lugar de la del vecino).

La interfaz de salida está apagada administrativamente.

4. **Corregir el error (Punto Crítico):** Para corregir una ruta estática mal configurada, **primero debes eliminar la ruta incorrecta** usando la versión `no` del comando, y luego ingresar la correcta. Si solo ingresas la nueva, el router mantendrá ambas y el problema continuará.

```
R1(config)# no ip route 192.168.2.0 255.255.255.0 10.1.1.3  <-- Eliminar la mala

R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.1.2<-- Configurar la correcta
```

---

