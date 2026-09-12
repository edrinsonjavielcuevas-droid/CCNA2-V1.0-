
---

## HSRP (Hot Standby Router Protocol)

HSRP es un protocolo propietario de Cisco diseñado para proporcionar redundancia de gateway predeterminado. Evita que un único punto de falla aísle a los dispositivos de una red local.

### HSRP - Descripción general

HSRP agrupa múltiples routers físicos para que actúen como un único **Router Virtual**. A este router virtual se le asigna una IP Virtual y una MAC Virtual que los hosts de la LAN utilizarán como su _Default Gateway_.

**Roles:** En un grupo HSRP, un router es elegido como **Activo** (enruta el tráfico de forma activa) y otro como **Standby** (permanece a la espera). Los demás routers quedan en estado _Listen_.

**Versiones de HSRP:**

**HSRPv1 (Por defecto):** Utiliza la dirección multicast IPv4 `224.0.0.2`. Su dirección MAC virtual tiene el formato `0000.0c07.acXX` (donde XX es el número de grupo en hexadecimal).

**HSRPv2:** Ofrece soporte para IPv6, temporizadores en milisegundos y utiliza la dirección multicast `224.0.0.102`. Su MAC virtual es `0000.0C9F.FXXX`.

### Prioridad e Intento de Prioridad del HSRP

La elección del router Activo y Standby se basa en la prioridad asignada a las interfaces.

**Prioridad HSRP (Priority):**

El valor por defecto es **100**.

El rango es de 0 a 255.

**El router con la prioridad más alta gana y se convierte en el router Activo.**

_Empate:_ Si las prioridades son iguales, gana el router con la dirección IP más alta configurada en esa interfaz.

**Comando:**

```

Router(config-if)# standby 1 priority 150

```

**Intento de Prioridad (Preemption):**

Por defecto, si un router se enciende y tiene una prioridad mayor que el router Activo actual, **NO** tomará el control. HSRP espera a que el Activo falle.

Habilitar `preempt` fuerza una nueva elección. El router con mayor prioridad tomará inmediatamente el rol de Activo, desplazando al actual.
 
**Comando:**

```
Router(config-if)# standby 1 preempt

```

### Estados y Temporizadores de HSRP

Para mantener el conocimiento mutuo, los routers HSRP intercambian mensajes de saludo (_Hello_).

**Temporizadores (Timers):**

**Hello Timer:** Intervalo en el que el router Activo envía mensajes multicast (por defecto: **3 segundos**).

**Hold Timer:** Tiempo que el router Standby esperará sin recibir un _Hello_ antes de declarar muerto al Activo y asumir el control (por defecto: **10 segundos**, casi siempre es 3 veces el tiempo de Hello).

**Comando (opcional para modificarlos):**

```
Router(config-if)# standby 1 timers 5 15
```

_(En este ejemplo: Hello cada 5 seg, Hold de 15 seg)._

---

**Estados de HSRP:** Al arrancar, una interfaz pasa por una serie de estados antes de volverse Activa o Standby:

1. **Initial:** Estado de inicio cuando HSRP se configura o la interfaz se enciende.

2. **Learn:** El router aún no sabe la IP virtual y espera mensajes Hello del router Activo.

3. **Listen:** Ya conoce la IP virtual, pero no es ni Activo ni Standby. Simplemente escucha los paquetes Hello.

4. **Speak:** El router participa activamente en la elección y envía sus propios paquetes Hello.

5. **Standby:** Es el candidato principal para convertirse en el próximo router Activo.

6. **Active:** El router que actualmente asume y reenvía el tráfico dirigido a la MAC e IP virtuales.

----

