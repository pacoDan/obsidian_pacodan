Los **filtros** son circuitos, sistemas o componentes de las redes de comunicaciones que presentan características selectivas respecto a las frecuencias de una señal.

Su **objetivo principal** es permitir el paso libre de las bandas de frecuencias que contienen la información útil que se desea transmitir, y aplicar una alta atenuación (debilitar o suprimir) a las frecuencias indeseables. Esto se logra porque la atenuación dentro del filtro varía según la frecuencia, lo que le permite discriminar qué partes de la señal pasan libremente y cuáles quedan bloqueadas.

Dentro de las clasificaciones de los filtros, el **filtro pasa bajos** cumple un rol específico:

- **¿Qué es?** Es un filtro diseñado para permitir el paso únicamente de las señales cuyas frecuencias van desde cero hasta un valor límite determinado, el cual recibe el nombre de **frecuencia de corte superior**. Las frecuencias que superan este valor son fuertemente atenuadas o eliminadas.
- **Detalle técnico:** En el punto exacto de la frecuencia de corte superior, la atenuación que produce este filtro es de **3 dB**, lo que significa que a partir de ese límite, la mitad de la potencia de la señal de entrada es eliminada.

**¿Para qué sirven en la práctica?** Los filtros pasa bajos son fundamentales para limitar el ancho de banda y acondicionar señales en los sistemas de comunicaciones. Por ejemplo, en un sistema de Modulación por Pulsos Codificados (PCM) que digitaliza la voz, los filtros pasa bajos se utilizan en dos etapas clave:

1. **En el transmisor:** Se colocan justo antes del muestreador para limitar el ancho de banda de la señal analógica original (por ejemplo, recortando todo lo que supere los 4000 Hz en un canal telefónico) para prepararla antes de su procesamiento digital.
2. **En el receptor:** Se utilizan como etapa final para poder recuperar y reconstruir la señal analógica original de forma inteligible a partir de las muestras digitales recibidas.

----


La limitación de la voz a 4000 Hz y la regla de los 3 dB responden a cuestiones de eficiencia técnica, economía de recursos en las redes y fundamentos físicos de la electrónica.

**¿Por qué la voz se limita a ese rango y de dónde salen los 4000 Hz?**

- **La concentración de energía de la voz:** Aunque el oído humano puede captar sonidos que van desde los 20 Hz hasta los 20.000 Hz, los estudios demuestran que **la mayor potencia y energía de la voz humana está comprendida estrictamente entre los 300 Hz y los 3300 Hz (o 3400 Hz)**.
- **Inteligibilidad suficiente:** La industria de las telecomunicaciones comprobó que no se necesitan canales más amplios para poder entender perfectamente el mensaje hablado y, además, reconocer el timbre y la identidad de la persona. Transmitir el resto de las frecuencias agudas o graves sería un gran desperdicio de ancho de banda para un servicio que no lo requiere. Por esto, la red telefónica recorta la voz a un canal útil de aproximadamente 3,1 kHz (de 300 a 3400 Hz).
- **El origen de los 4000 Hz (La Banda de Guarda):** Si el canal útil ocupa solo ~3100 Hz, el número estandarizado de 4000 Hz (4 kHz) se adopta por una necesidad de seguridad. La diferencia de frecuencias sobrantes se utiliza como una **"banda de guarda"** (o banda de protección). Al agrupar (multiplexar) muchas llamadas telefónicas por un mismo cable, este espacio vacío evita que las frecuencias de un canal se solapen o generen interferencias mutuas con el canal vecino. Además, al fijar el límite máximo absoluto en 4000 Hz, los sistemas digitales (como el modulador PCM) pueden aplicar de forma exacta el teorema de Nyquist, tomando **8000 muestras por segundo** (el doble de 4000) para digitalizar la voz exitosamente.

**¿Qué tienen que ver los 3 dB y de dónde sale ese número?** El valor de **3 dB (decibeles)** es la frontera estándar que se utiliza mundialmente en ingeniería para definir dónde termina el "ancho de banda" útil de un sistema y dónde actúa la "frecuencia de corte" de los filtros (como el filtro pasa bajos que acondiciona tu voz).

- **La caída de la potencia a la mitad:** Los decibeles son una escala logarítmica. Matemáticamente, una atenuación o pérdida de exactamente 3 dB significa que **la potencia de la señal eléctrica ha caído al 50%** (la mitad de su energía original).
- **El punto límite del filtro:** Dentro del ancho de banda que el filtro deja pasar libremente, las componentes de tu voz sufren atenuaciones tolerables (a lo sumo, llegan hasta los 3 dB de pérdida). El instante exacto donde la señal pierde la mitad de su potencia marca el fin del ancho de banda y el inicio del corte.
- A partir de ese punto de los 3 dB, el filtro actúa con fuerza: todas las frecuencias que continúan por encima o por debajo de ese límite sufren atenuaciones tan severas que su energía residual se vuelve prácticamente nula o despreciable, impidiendo que pasen al sistema.

En síntesis, se usa el límite de atenuación de **3 dB** para demarcar el umbral técnico donde el equipo considera que la señal ya no posee la energía suficiente para ser útil y decide recortar el resto de la información.




