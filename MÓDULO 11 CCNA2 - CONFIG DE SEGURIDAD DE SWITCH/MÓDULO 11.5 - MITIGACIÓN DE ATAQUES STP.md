
---

El Protocolo Spanning Tree (STP) es vulnerable a manipulaciones. Un atacante puede conectar un switch falso a la red y enviar tramas BPDU (Bridge Protocol Data Units) con una prioridad baja para forzar una nueva elección y convertirse en el _Root Bridge_ (Puente Raíz). Esto le permitiría interceptar gran parte del tráfico de la red. Para mitigar esto, se utilizan las funciones de PortFast y Protección BPDU (BPDU Guard).

### PortFast y protección BPDU

**PortFast:** Es una característica que hace que un puerto configurado como puerto de acceso pase inmediatamente al estado de reenvío (_forwarding_), saltándose los estados de escucha (_listening_) y aprendizaje (_learning_) de STP. Esto es útil para dispositivos finales (PCs, servidores) que necesitan conectividad inmediata y no pueden crear bucles de red. 

**Nota importante:** PortFast nunca debe habilitarse en puertos conectados a otros switches.

**Protección BPDU (BPDU Guard):** Dado que un puerto de acceso con un dispositivo final conectado no debería recibir mensajes de control de switch (BPDUs), la protección BPDU se encarga de monitorear esto. Si un puerto configurado con BPDU Guard recibe un BPDU, el puerto se apaga inmediatamente (entra en estado _err-disabled_) para proteger la topología de la red.

### Configure PortFast

PortFast se puede configurar de dos maneras: directamente en una interfaz específica o de forma global para todos los puertos de acceso.

**Configuración por interfaz:** Se aplica en los puertos específicos donde están conectados los dispositivos finales.

```
Switch(config)# interface FastEthernet0/1

Switch(config-if)# switchport mode access

Switch(config-if)# spanning-tree portfast
```

_(Aparecerá un mensaje de advertencia recordando que solo debe usarse en puertos conectados a hosts)._

**Configuración global:** Habilita PortFast de manera predeterminada en todas las interfaces que estén configuradas en modo de acceso.

```
Switch(config)# spanning-tree portfast default
```

---
### Configurar la protección BPDU

Al igual que PortFast, la protección BPDU (BPDU Guard) se puede habilitar por interfaz o de manera global.

**Configuración por interfaz:** Habilita la protección independientemente de si PortFast está activo o no en ese puerto.

```
Switch(config)# interface FastEthernet0/1

Switch(config-if)# spanning-tree bpduguard enable
```

**Configuración global:** Habilita BPDU Guard implícitamente en todos los puertos que tengan PortFast activado. Esta es la práctica recomendada más común.

```
Switch(config)# spanning-tree portfast bpduguard default
```

**Recuperación de un puerto apagado por BPDU Guard:** Si un puerto entra en estado `err-disabled` por recibir un BPDU, el administrador debe entrar a la interfaz, aplicar `shutdown` y luego `no shutdown` para reactivarlo, o configurar la recuperación automática globalmente:

```
Switch(config)# errdisable recovery cause bpduguard

Switch(config)# errdisable recovery interval 300
```

----

