Cual es la expresión de la capacidad máxima de un canal sin ruido? dar sus formulas y la correcta
bajo que condiciones y que comportamiento?
Que entendemos por un error en teleinformática 
Tipos de errores

Principales causas de errores, que son y de que se trata
Políticas para el tratamiento de errores 

¿ Calidad de servicio de un sistema que es y como se mide?
Como se define sus parámetros y corrección de errores


### Ejercicio 1 Principales causas de errores

Las alteraciones en la información durante su transmisión por un canal se deben fundamentalmente a los siguientes factores:

- **Ruido:** Interferencia general que afecta la señal.
- **Atenuación:** Pérdida de la potencia útil de la señal, especialmente crítica en transmisiones analógicas.
- **Distorsión:** Deformación de la forma de la señal, con mayor impacto en señales digitales.
- **Ancho de banda insuficiente:** Si el canal no puede soportar la cantidad de componentes de frecuencia de la señal, se producen deformaciones y pérdidas de información.
- **Tasa de información ($T$) mayor a la capacidad ($C$):** Intentar transmitir a una velocidad superior a la que el canal permite físicamente.
- **Interferencia intersímbolos y pérdida de sincronismo:** Fenómenos que causan que los pulsos se superpongan o se desplacen, dificultando su correcta interpretación en el destino.

### Ejercicio 2 Políticas para el tratamiento de errores

Las redes emplean dos políticas o acciones generales y sucesivas bajo el concepto de **control de errores**:

1. **Detección de errores:** Consiste en identificar que un mensaje ha llegado alterado. Se utilizan métodos como el **control de paridad** (vertical, longitudinal o cíclica), **códigos polinomiales (CRC)** y la **suma de verificación (checksum)**.
2. **Corrección de errores:** Una vez detectado el fallo, se procede a su solución. Esto puede hacerse mediante **retransmisión (ARQ)**, donde el receptor solicita el envío del mensaje nuevamente; **corrección hacia adelante (FEC)**, transmitiendo la información en diferentes intervalos de tiempo; o mediante **códigos autocorrector**, como el **código Hamming**, que permite al receptor corregir el error por sí mismo sin solicitar una retransmisión.

### Ejercicio 3 Incidencia en la Calidad de Servicio (QoS)

La aplicación de sistemas de corrección de errores impacta significativamente en la calidad del sistema, basándose en una **solución de compromiso**:

- **Mejora de la fidelidad:** La corrección de errores asegura que el mensaje recibido sea una réplica exacta del transmitido, lo que **eleva la calidad del servicio** al reducir la tasa de errores percibida por el usuario.
- **Reducción del rendimiento (Eficiencia):** Para detectar y corregir errores es necesario añadir **redundancia** (bits adicionales que no contienen información útil). Cuanta más protección se añada, menor será el rendimiento neto de la red, ya que se transmiten más bits totales para la misma cantidad de datos.
- **Introducción de retardos (Delay):** Técnicas como la retransmisión (ARQ) o la diversidad en tiempo (FEC) generan esperas en la recepción final de la información, lo que afecta el tiempo real de la comunicación aunque no altere la exactitud de los datos.
- **Medición de la calidad:** El parámetro principal para medir esta incidencia es el **BER (Bit Error Rate)**, que relaciona los bits erróneos recibidos frente a los totales transmitidos; un BER más bajo indica una mayor calidad de servicio.

### Cuando se aplica el codigo autocorrector
El **código autocorrector**, cuyo ejemplo principal en las fuentes es el **código Hamming**, se aplica en situaciones específicas donde la integridad de los datos es crítica y se busca evitar la retransmisión de información.

De acuerdo con las fuentes, su aplicación se da principalmente en los siguientes contextos:

- **Información sensible y encriptada:** Se emplea para transmitir datos que no deben ser de conocimiento público y que viajan de forma **encriptada**. En estos casos, es vital que el mensaje recibido sea una réplica exacta del original, ya que cualquier alteración mínima en datos encriptados puede invalidar toda la información.
- **Terminales bancarias:** Un uso frecuente se encuentra en las **terminales bancarias**, donde se procesan movimientos de cuentas y transacciones que requieren una seguridad del **100%** y una llegada exacta al destino.
- **Sistemas Punto a Punto:** Estos códigos se utilizan habitualmente en transmisiones de **Punto a Punto**.
- **Evitar la retransmisión (ARQ):** Se aplican cuando se prefiere que el receptor tenga la capacidad de detectar y **corregir el error automáticamente** con sus propios medios tecnológicos, sin necesidad de solicitar que el emisor envíe el paquete nuevamente.
- **Capas del Modelo OSI:** Dependiendo de la estrategia de la red, estos mecanismos pueden implementarse en la **capa de red (nivel 3)** o en la **capa de transporte (nivel 4)** para asegurar la corrección de los datos antes de que lleguen a la aplicación final.

Técnicamente, su funcionamiento se basa en la **distancia de Hamming**, que es la diferencia de bits entre dos tramas de igual peso; esta métrica permite determinar matemáticamente cuántos errores es capaz de corregir el sistema de forma inherente.


Para responder a tu consulta, basándome en las fuentes proporcionadas, aquí tienes los detalles solicitados:

### Ejercicio 9. Protocolos que emplean CRC para detección de errores

El código de redundancia cíclica (CRC) se utiliza ampliamente en la capa de enlace (nivel 2) del modelo OSI para asegurar la integridad de los datos. Al menos cuatro protocolos o sistemas que lo emplean son:

- **Ethernet:** Utilizado en redes de área local (LAN).
- **HDLC (High-Level Data Link Control):** Un protocolo de control de enlace de alto nivel.
- **Frame Relay:** Un sistema de transmisión por conmutación de paquetes.
- **MPLS (Multiprotocol Label Switching):** Otra técnica de conmutación de paquetes por un mismo vínculo físico.

### Ejercicio 10. Protocolos que emplean Suma de Verificación (Checksum)

Este método es común en las capas de red y transporte para verificar la integridad de los cabezales y datos. Cuatro ejemplos de protocolos que lo utilizan son:

- **TCP (Transmission Control Protocol):** Asociado a la capa de transporte.
- **UDP (User Datagram Protocol):** También en la capa de transporte.
- **IP (Internet Protocol):** En la capa de red del modelo internet.
- **ICMP (Internet Control Message Protocol):** Protocolo de control de mensajes de internet.

### Ejercicio 11. Uso de códigos correctores de errores y ejemplos

Los códigos autocorrector, como el **código Hamming**, se emplean cuando se requiere una seguridad extrema y se desea evitar la retransmisión de datos. Ejemplos específicos de su aplicación incluyen:

- **Transmisión de información encriptada:** Donde la sensibilidad de los datos es alta y cualquier alteración mínima puede comprometer el mensaje.
- **Terminales bancarias:** Para cursar movimientos de cuentas y transacciones financieras que deben llegar 100% seguras y exactas.
- **Sistemas Punto a Punto:** En estos vínculos se prefiere que el receptor corrija el error automáticamente con su propia tecnología.
- **Capas superiores del modelo OSI:** Se aplican en las capas de transporte (nivel 4) o red (nivel 3) para garantizar la corrección antes de la entrega final.

### Ejercicio 12. Manifestación y medición del error en redes de datos

En las redes de datos, el error se manifiesta principalmente de dos formas:

- **Alteración de bits:** Cuando un bit cambia de valor durante el trayecto (un 0 llega como 1 o viceversa).
- **Pérdida de información:** Se manifiesta como la pérdida de paquetes completos cuando no se pueden recuperar mediante los métodos aplicados.

Para medir estos errores, se utilizan los siguientes parámetros:

- **BER (Bit Error Rate):** Es la medida principal en redes digitales y se define como la relación entre los **bits erróneos recibidos respecto de los bits totales transmitidos**. Se suele expresar en notación científica (ej. $10^{-9}$ para una red LAN).
- **Relación Señal a Ruido (SNR):** Se utiliza mayormente para señales analógicas, midiendo la potencia útil de la señal frente a la potencia del ruido.
- **Tasa de pérdida de paquetes:** Indicador de cuántos paquetes no llegaron correctamente al destino tras agotar los intentos de retransmisión.
----

### Capacidad Máxima de un Canal sin Ruido

La expresión general para la capacidad ($C$) de un canal ideal (sin ruido) es **$C = 2 \cdot BW \cdot \log_2(n)$**.

- **$BW$**: Es el ancho de banda del canal.
- **$n$**: Representa la cantidad de estados significativos o niveles de la señal (transmisión multinivel).

**Consideraciones y Comportamiento:**

- **Caso particular:** Cuando la transmisión es bit a bit (binaria), $n=2$, por lo que la fórmula se reduce a $C = 2 \cdot BW$, ya que el logaritmo en base 2 de 2 es igual a 1.
- **Condiciones:** Para buscar la máxima velocidad de transmisión en condiciones ideales, el ancho de banda ($BW$) debe tender a infinito. Si se tratara de un canal con ruido, se debería considerar la relación señal a ruido en la fórmula.

### El Error en Teleinformática

En el ámbito de la teleinformática, un **error** se define como **toda alteración que provoca que un mensaje recibido no sea una réplica exacta del mensaje transmitido**. El objetivo de los sistemas de comunicación es que la información llegue con la menor probabilidad de error posible.

### Tipos de Errores

Los errores se pueden tipificar en tres grandes grupos según cómo afecten a los bits:

1. **Aislado o Simple:** Afecta a **un solo bit** por vez y los errores son independientes entre sí.
2. **En Ráfagas:** Afecta a **varios bits consecutivos** en un periodo de tiempo determinado.
3. **Agrupados:** Afectan a varios bits en tandas sucesivas, pero **no necesariamente de forma consecutiva** (puede haber transmisión normal entre bits con error dentro de un mismo grupo).

### Principales Causas de Errores

Las alteraciones en la trama de bits mientras la información viaja por el canal se deben principalmente a:

- **Ruido:** Afectación general que altera la señal.
- **Atenuación:** Pérdida de la potencia útil de la señal, con mayor peso en señales analógicas.
- **Distorsión:** Alteración de la forma de la señal, que afecta predominantemente a las señales digitales.
- **Ancho de banda insuficiente:** Si el canal no tiene capacidad suficiente para la cantidad de información, se producen pérdidas que se traducen en errores.
- **Tasa de información ($T$) mayor a la capacidad ($C$):** Cuando se intenta transmitir más información de la que el canal puede soportar.

### Políticas para el Tratamiento de Errores

Existen dos políticas generales y sucesivas para gestionar los errores:

1. **Detección:** El primer paso es identificar que existe un error mediante métodos como el control de paridad (vertical, longitudinal o cíclica) o códigos polinomiales (CRC).
2. **Corrección:** Una vez detectado, se procede a su solución a través de técnicas como la retransmisión (ARQ), corrección hacia adelante (FEC) o códigos autocorrector (como el código Hamming).

### Calidad de Servicio (QoS) y Parámetros

La **Calidad de Servicio** en un sistema de transmisión se mide principalmente a través de la tasa de errores, ya que una alta presencia de errores implica una baja performance y rendimiento.

**Parámetros de medición:**

- **BER (Bit Error Rate):** Es la relación entre la cantidad de **bits erróneos recibidos respecto de los bits totales transmitidos**. Se expresa en notación científica; por ejemplo, una red LAN suele tener un BER de $10^{-9}$ (un error cada mil millones de bits), lo cual indica una alta calidad.
- **Relación Señal a Ruido (SNR):** Utilizada normalmente para señales analógicas, mide la potencia útil de la señal respecto a la potencia del ruido.

**Corrección y Calidad:** La aplicación de sistemas de detección y corrección mejora la calidad del servicio al garantizar que la información llegue de forma exacta, aunque esto puede introducir **redundancia** (bits extra que no aportan información) y disminuir el rendimiento neto o causar **delays** (retardos) en la transmisión. Por ejemplo, el **Código Hamming** permite detectar y corregir errores automáticamente basándose en la "distancia de Hamming" entre tramas de bits.