Basado en el material proporcionado, a continuación se presentan las diferencias, ventajas y desventajas de las técnicas de modulación **M-PSK** (Modulación por Salto de Fase Multinivel) y **M-QAM** (Modulación de Amplitud en Cuadratura Multinivel).

### Diferencias entre M-PSK y M-QAM

- **Parámetros Modulados:** En **M-PSK**, la información digital modifica únicamente la **fase** de una sola señal portadora analógica, manteniendo constantes la amplitud y la frecuencia. En cambio, **M-QAM** es un tipo de modulación de fase multinivel que modifica simultáneamente tanto la **fase como la amplitud** de la señal.
- **Portadoras Utilizadas:** Mientras que las técnicas PSK convencionales operan con una sola portadora, la modulación **QAM** se caracteriza por intervenir con **dos portadoras** que ingresan a dos mezcladores y se suman en cuadratura a la salida.
- **Representación en Constelaciones:** Los estados de **M-PSK** se distribuyen en un círculo (diagrama vectorial), donde todos los puntos tienen la misma amplitud pero diferente ángulo de fase. En **M-QAM**, los estados pueden ocupar múltiples niveles de amplitud, lo que se visualiza en el diagrama como puntos distribuidos en diferentes radios o en una cuadrícula.

### M-PSK (Multifase o Multinivel)

**Ventajas:**

- **Eficiencia en el Ancho de Banda:** Permite aumentar la velocidad de transmisión de bits sin incrementar el ancho de banda del canal, ya que cada símbolo (o golpe de clock) transporta múltiples bits (2 para 4-PSK, 3 para 8-PSK, etc.).
- **Inmunidad al Ruido de Amplitud:** Al mantener la amplitud constante, es inherentemente más robusta frente a ruidos que afectan principalmente la potencia o amplitud de la señal en comparación con modulaciones como AM.

**Desventajas:**

- **Susceptibilidad al Ruido por Proximidad de Estados:** A medida que aumenta el valor de **M** (más niveles), el desfasaje angular entre los estados disminuye (por ejemplo, 22.5° para 16-PSK). Esto reduce el umbral de decisión, haciendo que la señal sea más propensa a errores de detección si hay ruido en la fase.
- **Límite de Estados:** Debido a la cercanía de los puntos en una constelación circular, no es práctico aumentar indefinidamente el número de niveles en PSK pura sin comprometer gravemente la tasa de error.

### M-QAM (Amplitud en Cuadratura)

**Ventajas:**

- **Alta Velocidad de Transmisión:** Al combinar variaciones de fase y amplitud, puede alcanzar estados más distinguidos que PSK para una misma cantidad de niveles, permitiendo altas tasas de datos (como 16-QAM que transmite 4 bits por símbolo).
- **Mejor Aprovechamiento del Espacio de Señal:** Permite colocar más estados significativos de manera más eficiente en el diagrama fasorial en comparación con PSK, lo que mejora la performance en sistemas de alta capacidad.

**Desventajas:**

- **Complejidad Técnica:** Requiere una implementación de hardware más compleja y costosa debido a la necesidad de manejar dos portadoras y realizar la suma en cuadratura.
- **Sensibilidad al Ruido de Amplitud:** A diferencia de PSK, al incluir variaciones en la amplitud, QAM es más sensible a ruidos e interferencias que degradan la potencia de la señal, lo que puede requerir una mejor relación señal/ruido en el canal para operar confiablemente.