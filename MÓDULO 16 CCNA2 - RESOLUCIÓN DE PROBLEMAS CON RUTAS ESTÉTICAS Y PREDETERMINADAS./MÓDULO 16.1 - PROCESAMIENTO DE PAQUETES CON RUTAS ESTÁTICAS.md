
---

## Procesamiento de paquetes con rutas estáticas

Para consolidar cómo funcionan las rutas estáticas en la práctica, es vital entender el proceso salto por salto. A continuación, se detalla el ciclo de encapsulamiento, búsqueda y reenvío de un paquete a través de múltiples routers, desde su origen hasta su destino final.

### Rutas estáticas y envío de paquetes

El siguiente flujo describe el viaje de un paquete de datos a través de una topología compuesta por tres routers (R1, R2 y R3) conectados por enlaces seriales punto a punto, hasta llegar a una PC de destino (PC3):

**Procesamiento en el Router de Origen (R1):**
    
1. El paquete llega a la interfaz de red de área local (GigabitEthernet 0/0/0) de R1.

2. R1 examina la dirección IP de destino (`192.168.2.0/24`). Al no tener una ruta específica para esa red en su tabla, recurre a utilizar su **ruta estática predeterminada**.

3. R1 encapsula el paquete en una nueva trama de Capa 2. Como la interfaz de salida es un enlace serial punto a punto hacia R2, R1 establece la dirección de destino de Capa 2 con puros "unos" (todos 1, equivalente a un broadcast), ya que no hay direcciones MAC en este tipo de enlaces.
  
4. La trama se reenvía físicamente desde la interfaz Serial 0/1/0 de R1 y llega a la interfaz Serial 0/0/0 de R2.
----

**Procesamiento en el Router Intermedio (R2):** 

5. R2 recibe la trama, la desencapsula y lee la IP de destino del paquete. Al consultar su tabla, R2 sí posee una **ruta estática específica** hacia la red `192.168.2.0/24`, la cual apunta hacia su interfaz Serial 0/1/1. 

6. R2 vuelve a encapsular el paquete en una nueva trama. Nuevamente, por tratarse de un enlace punto a punto hacia R3, utiliza una dirección de destino de Capa 2 de "todos unos" (1). 

7. La trama sale por la interfaz Serial 0/1/1 de R2 y llega a la interfaz Serial 0/0/1 de R3.

---

**Procesamiento en el Router de Destino (R3):** 

8. R3 desencapsula la trama y verifica la red de destino. Descubre en su tabla de enrutamiento que la red `192.168.2.0/24` es una **red conectada directamente** a su interfaz GigabitEthernet 0/0/0. 

9. Dado que GigabitEthernet es una red de acceso múltiple, R3 necesita la dirección MAC exacta de la PC de destino (`192.168.2.10`). Revisa su tabla ARP; si no encuentra la entrada, envía una solicitud ARP por la interfaz G0/0/0. PC3 responde entregando su dirección MAC. 

10. Con esta información, R3 encapsula el paquete en su trama final. Coloca la dirección MAC de su propia interfaz G0/0/0 como origen, y la dirección MAC de PC3 como destino de Capa 2. 

11. La trama se reenvía por la interfaz LAN y el paquete llega exitosamente a la tarjeta de interfaz de red (NIC) de PC3.

---

