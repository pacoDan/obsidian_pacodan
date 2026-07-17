El proceso de digitalización es la transformación de una señal analógica (como la voz o el video, que varían de forma continua en el tiempo) en una señal puramente digital compuesta por dígitos binarios (ceros y unos).

Este proceso se lleva a cabo mediante tres etapas fundamentales y sucesivas:

1. **Muestreo:** Consiste en tomar muestras periódicas de la señal analógica original a intervalos de tiempo regulares. Para que la señal pueda ser reconstruida en el destino sin pérdida de información, se aplica el teorema de Nyquist, el cual establece que la frecuencia de muestreo debe ser, por lo menos, el doble de la frecuencia máxima de la señal analógica. Por ejemplo, para digitalizar un canal de voz de 4000 Hz, se toman 8000 muestras por segundo.
2. **Cuantificación:** Las muestras obtenidas en el paso anterior aún pueden tomar infinitos valores de amplitud. El proceso de cuantificación consiste en "redondear" y transformar esos niveles continuos en un **conjunto finito de niveles discretos previamente establecidos** (llamados niveles cuánticos). Al forzar la señal a encajar en un nivel específico, se introduce una pequeña diferencia entre el valor real y el asignado, lo que se conoce como "error de cuantificación".
3. **Codificación:** Es la etapa final, donde cada uno de los niveles discretos obtenidos en la cuantificación se convierte en un **grupo equivalente de pulsos binarios** de amplitud constante. Por ejemplo, si en la etapa anterior se definieron 256 niveles cuánticos, la codificación asignará a cada muestra una palabra binaria de 8 bits ($2^8 = 256$).

**¿Qué tiene que ver esto con la modulación PCM?**

La relación es directa y absoluta: **el PCM (Modulación por Pulsos Codificados) es precisamente la aplicación técnica de este proceso de digitalización**.

En las telecomunicaciones, se define al PCM como el método de modulación que consiste en transmitir información analógica en forma de señales digitales _mediante el proceso continuo de muestreo, cuantificación y codificación_.

Es decir, el modulador PCM es el hardware encargado de llevar a cabo la digitalización. Un equipo transmisor PCM está compuesto exactamente por estas etapas: primero un filtro pasa bajos (para adecuar la señal), seguido de un **muestreador**, un **cuantificador** y un **codificador**. Por lo tanto, hablar del proceso de digitalización de una señal analógica es, en la práctica, describir el funcionamiento interno de un sistema de modulación PCM.