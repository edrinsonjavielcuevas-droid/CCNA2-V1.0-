
---

Para proteger la red de forma integral, es vital controlar estrictamente quién y qué se conecta a ella a nivel de puerto físico.

**Marco AAA (Autenticación, Autorización y Contabilidad):**

**Autenticación:** Verifica la identidad del usuario ("¿quién eres?") consultando un servidor centralizado como RADIUS o TACACS+.

**Autorización:** Determina los privilegios y recursos permitidos ("¿qué puedes hacer?") tras una autenticación exitosa.

**Contabilidad:** Registra y audita las acciones y tiempos de conexión del usuario ("¿qué hiciste?").

**Estándar IEEE 802.1X:**

Proporciona control de acceso a la red basado en puertos. El puerto físico del switch se bloquea para el tráfico de datos regular hasta que el cliente se autentica correctamente.

Involucra tres componentes: el **suplicante** (software cliente), el **autenticador** (switch) y el **servidor de autenticación** (servidor RADIUS).

![](LINE%20VTY.png)

![](CONFIG%20SSH.png)

---

