La modulación **16 PSK** (Phase Shift Keying o Salto de Fase) es una técnica de **modulación continua digital** en la que la fase de la señal portadora salta entre 16 valores diferentes para representar datos binarios.

A continuación, se detallan sus características principales según las fuentes:

### 1. Capacidad de bits y estados

- **Símbolos y bits:** En esta técnica, el valor de estados posibles ($M$) es **16**. Aplicando la relación $M = 2^n$, se determina que cada salto de fase o símbolo transporta **4 bits** ($2^4 = 16$).
- **Velocidad:** Al transportar 4 bits por cada cambio de fase, permite alcanzar una **mayor velocidad binaria** en comparación con versiones de menos estados como 8 PSK o QPSK (4 PSK).

### 2. Representación en la Constelación

- **Amplitud Constante:** A diferencia de las técnicas QAM, en la modulación 16 PSK la amplitud de la señal no varía. Todos los estados se distribuyen sobre **una sola circunferencia** en el diagrama fasorial o de constelación.
- **Separación angular:** Los estados se ubican siguiendo la fórmula $\theta = 2\pi/M$. Para 16 PSK, esto significa que los puntos están separados por ángulos muy pequeños, lo que provoca que los estados estén **muy próximos entre sí**.

### 3. Rendimiento y Probabilidad de Error

- **Sensibilidad al ruido:** Debido a que los 16 estados están "amontonados" en el mismo círculo, existe una **mayor probabilidad de error** ante la presencia de ruido en el canal. El ruido puede hacer que el receptor confunda un estado con el adyacente más fácilmente que en modulaciones con menos saltos.
- **Comparación con 16 QAM:** Aunque 16 PSK y 16 QAM tienen la misma velocidad binaria (4 bits por símbolo), la 16 QAM es más eficiente frente al error. Esto se debe a que la 16 QAM distribuye los estados en varias amplitudes (varios círculos o una rejilla rectangular), manteniéndolos más separados.
- **Potencia:** Para que una señal 16 PSK tenga la misma resistencia al ruido (misma tasa de error) que una 16 QAM, es necesario **incrementar significativamente la potencia** para ampliar el radio del círculo y separar más los puntos.

En resumen, la 16 PSK es una técnica que prioriza la velocidad de transmisión, pero requiere un canal muy limpio o un aumento de potencia debido a la **proximidad de sus estados** en la constelación.