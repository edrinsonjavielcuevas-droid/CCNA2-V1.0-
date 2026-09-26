
---

La gestión adecuada del espectro de radiofrecuencia (RF) es fundamental para evitar la interferencia y garantizar un rendimiento óptimo en las redes inalámbricas (WLAN).

### Canal de frecuencia de saturación

**Concepto:** Ocurre cuando múltiples dispositivos o redes inalámbricas vecinas compiten por utilizar el mismo canal de radiofrecuencia en una misma área física.

**Problemas derivados:** La congestión de canales reduce drásticamente el ancho de banda efectivo, aumenta la latencia y genera pérdida de paquetes debido a la interferencia.

**Fuentes de interferencia:** Además de redes Wi-Fi superpuestas, dispositivos como hornos microondas, cámaras inalámbricas y conexiones Bluetooth contribuyen a saturar el espectro (especialmente en la banda de 2.4 GHz).

---
### Selección de canales

Para mitigar la saturación y maximizar el rendimiento, se aplican estrategias de asignación:

**Banda de 2.4 GHz:** Al contar con un espectro limitado, se deben utilizar únicamente los **canales no superpuestos (1, 6 y 11)** para evitar la interferencia de canal adyacente entre Puntos de Acceso cercanos.

**Banda de 5 GHz:** Ofrece un rango mucho más amplio y una cantidad superior de canales independientes, lo que reduce sustancialmente los problemas de congestión.

**Asignación Dinámica:** En arquitecturas empresariales gestionadas por controladores (WLC), el sistema evalúa el entorno de RF y ajusta automáticamente los canales de los APs para evitar interferencias en tiempo real.

---
### Planifique la implementación de WLAN

Una correcta planificación física y lógica previene puntos muertos y problemas de capacidad:

**Estudio de sitio (_Site Survey_):** Permite identificar obstáculos físicos (como paredes de concreto o estructuras metálicas) y fuentes de ruido electromagnético antes de desplegar el hardware.

**Superposición de celdas:** Se recomienda un solapamiento de cobertura del **10% al 15%** entre las celdas de Puntos de Acceso adyacentes para permitir un _roaming_ (itinerancia) fluido sin pérdida de conectividad.

**Reutilización de canales:** Distribuir los canales no superpuestos de forma alternada entre los APs vecinos para que no compitan directamente en la misma frecuencia.

---

