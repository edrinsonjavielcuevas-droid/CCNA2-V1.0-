
---

Los switches utilizan tablas de direcciones MAC (tablas CAM) en su memoria para registrar qué dirección MAC está conectada a qué puerto, permitiéndoles reenviar tramas de forma directa (unicast).

**El Ataque (MAC Flooding):** El atacante conecta una computadora y usa herramientas (como `macof`) para generar artificialmente miles de tramas falsas por segundo, llenando la memoria finita del switch.

**Estado "Fail-Open":** Cuando la tabla se llena, el switch no puede aprender nuevas direcciones. Para evitar que la red caiga, entra en un estado de emergencia (falla abierta) y comienza a comportarse como un antiguo Hub.

**El Impacto:** El switch inunda todo el tráfico entrante copiándolo por todos sus puertos. El atacante ahora puede capturar (sniffing) datos de otros usuarios que originalmente no estaban destinados a su puerto físico.

**Mitigación:** Implementar **Port Security** (Seguridad de Puertos), lo que obliga al switch a limitar estrictamente la cantidad de direcciones MAC diferentes que un puerto físico puede aprender.

---

