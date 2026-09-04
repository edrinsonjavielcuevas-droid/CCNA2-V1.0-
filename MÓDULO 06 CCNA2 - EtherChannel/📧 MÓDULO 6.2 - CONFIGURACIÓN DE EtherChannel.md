
---
### INSTRUCCIONES DE CONFIG

Ya que tenemos conocimiento de EtherChannel, en este tema se explica cómo configurarlo. Las siguientes pautas y restricciones son útiles para configurar EtherChannel:

**SOPORTE DE EtherChannel** - Todas las interfaces EtherChannel deben admitir EtherChannel, sin necesidad de que las interfaces sean físicamente contiguas.

**VELOCIDAD Y DÚPLEX** - Configure todas las interfaces de un EtherChannel para que funcionen a la misma velocidad y en el mismo modo dúplex. 

**COINCIDENCIA VLAN** - Todas las interfaces en el grupo EtherChannel se deben asignar a la misma VLAN o se deben configurar como enlace troncal.

**RANGO DE VLAN** - Un EtherChannel admite el mismo rango permitido de VLAN en todas las interfaces de un EtherChannel trunking. Si el rango permitido de VLAN no es el mismo, las interfaces no forman un EtherChannel, incluso cuando están configuradas en modo o auto deseable.

La figura muestra una configuración que permitiría que se forme un EtherChannel entre el S1 y el S2.

![](../CCNA2IMG/IMGM6/EtherChannel%20Formado.png)

En la siguiente figura, los puertos de S1 están configurados en modo semidúplex. Por lo tanto, no se formará un EtherChannel entre el S1 y el S2.

![](../CCNA2IMG/IMGM6/EtherChannel%20Mal%20Formado.png)

Si se deben modificar estos parámetros, configúrelos en el modo de configuración de interfaz de canal de puertos. Cualquier configuración que se aplique a la interfaz de canal de puertos también afectará a las interfaces individuales. Sin embargo, las configuraciones que se aplican a las interfaces individuales no afectan a la interfaz de canal de puertos. Por ello, realizar cambios de configuración a una interfaz que forma parte de un enlace EtherChannel puede causar problemas de compatibilidad de interfaces.

El canal de puertos se puede configurar en modo de acceso, modo de enlace troncal (más frecuente) o en un puerto enrutado.

---
### Ejemplo de Configuración de LACP

EtherChannel está deshabilitado de forma predeterminada y debe configurarse. La topología de la figura se utilizará para demostrar un ejemplo de configuración de EtherChannel utilizando LACP.

![](../CCNA2IMG/IMGM6/Ejemplo%20de%20Configuración%20de%20LACP.png)

La configuración de EtherChannel con LACP requiere tres pasos:

**Paso 1.** Especifique las interfaces que conforman el grupo EtherChannel mediante el **interface range** el comando de modo de configuración _interface_ global. La palabra clave **range** le permite seleccionar varias interfaces y configurarlas a la vez.

**Paso 2.** Cree la interfaz port channel con el **channel-group** comando **mode active** _identifier_ en el modo de configuración de interface range. El identificador especifica el número del grupo del canal. Las **mode active** palabras clave identifican a esta configuración como EtherChannel LACP.

**Paso 3.** Para cambiar la configuración de capa 2 en la interfaz de canal de puertos, ingrese al modo de configuración de interfaz de canal de puertos mediante el **interface port-channel** comando, seguido del identificador de la interfaz. En el ejemplo, S1 está configurado con un EtherChannel LACP. El canal de puertos está configurado como interfaz de enlace troncal con VLAN permitidas específicas.

![](../CCNA2IMG/IMGM6/CONFIG%20EtherChannel%20con%20LACP.png)

---

