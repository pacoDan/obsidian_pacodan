La **capacidad de un canal** se define como la **máxima velocidad de transmisión de información sin errores**, la cual se mide generalmente en bits por segundo (bps). En un canal **sin ruido**, esta capacidad máxima depende fundamentalmente de dos factores: el **ancho de banda** ($W$) del medio y el **número de niveles** discretos ($n$) que puede adoptar la señal.

Según las fuentes, los aspectos clave para determinar esta capacidad son los siguientes:

- **Relación con la velocidad de transmisión:** La capacidad máxima efectiva corresponde a la velocidad de transmisión ($V_t$), que se calcula mediante la fórmula **$V_t = V_m \cdot \log_2(n)$**, donde $V_m$ es la velocidad de modulación en baudios.
- **Vínculo con el ancho de banda:** En el contexto de un tren de pulsos, la velocidad de modulación ($V_m$) es equivalente al **ancho de banda** del canal, lo que significa que el ancho de banda limita directamente la cantidad de pulsos que pueden enviarse por segundo.
- **Incremento mediante multinivel:** Para aumentar la capacidad sin necesidad de expandir el ancho de banda (lo cual en canales reales aumentaría el ruido), se utiliza la **transmisión multinivel**. Al incrementar el número de estados significativos ($n$), es posible transportar más bits por cada pulso o "golpe de clock".
- **Eficiencia de la fuente:** Para aprovechar la plena capacidad del canal, la **tasa de información de la fuente** ($T$) debe ser menor o igual a dicha capacidad ($T \leq C$); de lo contrario, se producirían errores en la comunicación.

En resumen, la capacidad máxima de un canal sin ruido está determinada por la expresión que relaciona el **doble del ancho de banda** (implícito en la relación de $V_m$) con el **logaritmo en base 2 del número de niveles** de la señal.