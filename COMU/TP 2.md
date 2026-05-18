A continuación se presenta la respuesta detallada basada en los textos y materiales proporcionados.

### 1. Señales Analógicas y Digitales

#### Representación y Características

- **Señal Analógica:** Se representa como una función continua que toma un **número infinito de valores** en cualquier intervalo considerado. Suelen estudiarse tomando como referencia la **función senoidal armónica simple**.
- **Señal Digital:** Se representa mediante funciones que toman un **número finito de valores** (discretos) en cualquier intervalo. Un ejemplo típico es la **onda cuadrada** o tren de pulsos.

#### Transporte de Información

- **Sistemas Analógicos:** La información está contenida en la **propia forma de la onda** transmitida. Cualquier deformación de la forma de onda afecta directamente la fidelidad del mensaje.
- **Sistemas Digitales:** La información reside en los **pulsos codificados** (ceros y unos). La inteligencia está en la codificación, no necesariamente en la forma exacta de la onda siempre que se pueda distinguir entre los estados.

---

### 2. Ventajas y Desventajas de la Transmisión Digital

**Cinco ventajas notables:**

1. **Regeneración de señal y eliminación de ruido:** Los repetidores regenerativos reconstruyen los pulsos eliminando el ruido acumulado, a diferencia de los amplificadores analógicos que también amplifican el ruido.
2. **Alcance teóricamente infinito:** Gracias a la regeneración sin arrastre de ruido, la señal puede viajar distancias ilimitadas mediante saltos sucesivos.
3. **Convergencia multimedia:** Permite integrar voz, datos, texto e imágenes en un mismo medio de transmisión.
4. **Compresión de datos:** Facilita el uso de técnicas de compresión para reducir el volumen de información y abaratar costos.
5. **Protección y corrección de errores:** Permite el uso de protocolos (como bloques de bits) para detectar y corregir errores en el destino.

~~**Principal desventaja:** Es altamente sensible a la **distorsión producida por el ancho de banda finito** de los canales. Una señal digital ideal requiere infinitas armónicas; al limitarlas, la señal se deforma y puede causar errores en la detección de los bits, lo que obliga a colocar repetidores a distancias mucho menores que en los sistemas analógicos.~~


Las cinco ventajas más notables de la transmisión digital frente a la analógica, según las fuentes, son las siguientes:

1. **Regeneración de la señal y eliminación de ruido:** Mientras que en los canales analógicos los amplificadores también amplifican el ruido acumulado, en los sistemas digitales se utilizan **repetidores regenerativos**. Estos equipos reconstruyen los pulsos de salida con la misma forma original, **eliminando el ruido** que la señal llevaba antes de ser procesada.
2. **Convergencia y multicanalidad:** La digitalización permite la **integración multimedia**, permitiendo que un mismo medio de comunicación transporte simultáneamente servicios de voz, textos, imágenes y datos.
3. **Compresión de datos:** La transmisión digital permite aplicar técnicas de **compresión**, lo que reduce el volumen de datos a transferir, ahorra espacio de almacenamiento y abarata los costos de comunicación sin necesidad de aumentar el ancho de banda.
4. **Detección y corrección de errores:** Los sistemas digitales permiten el uso de protocolos y bloques de bits destinados específicamente a la **protección y corrección de errores**, lo que garantiza que la información llegue al destino con mayor fidelidad.

**Principal desventaja de la transmisión digital:**

La principal desventaja es que las señales digitales son altamente sensibles a la **distorsión** producida por el **ancho de banda finito** de los medios de comunicación. Dado que una señal digital (como una onda cuadrada) requiere teóricamente de infinitas armónicas para ser reconstruida con fidelidad, la limitación física del canal siempre la deforma, lo que puede provocar errores en la determinación de los "unos" y "ceros". Además, esto obliga a que los repetidores regenerativos deban colocarse a **distancias mucho menores** que los amplificadores analógicos.

---

### 3. El Repetidor Regenerativo

Sus funciones principales son:

- **Reconstrucción:** Recibe pulsos distorsionados y los emite con su **forma y amplitud original**.
- **Limpieza de ruido:** Elimina el ruido que la señal captó en el trayecto anterior.
- **Mantenimiento del umbral:** Debe estar situado a una distancia tal que la señal llegue con un **umbral de detección** mínimo para ser reconocida y procesada.

---

### 4. Funciones Periódicas y Cálculos

#### Definiciones

- **Ciclo:** Recorrido completo de la señal hasta volver al estado inicial (360° o $2\pi$ radianes).
- **Período (T):** Tiempo mínimo necesario para completar un ciclo.
- **Frecuencia (f):** Número de ciclos por segundo ($f = 1/T$).
- **Pulsación angular ($\omega$):** Velocidad de rotación del vector ($\omega = 2\pi f$).
- **Longitud de onda ($\lambda$):** Distancia que recorre la señal en un período ($\lambda = v \cdot T = v/f$).
- **Valor instantáneo:** El valor de la función en un instante $t$ determinado.
- **Valor medio ($Y_m$):** Promedio de los valores de la función en un intervalo ($Y_m = \frac{1}{T} \int f(t) dt$).
- **Valor eficaz ($Y_e$):** Valor de una corriente continua equivalente que disiparía la misma potencia ($Y_e = \sqrt{\frac{1}{T} \int f(t)^2 dt}$).

#### Resolución del ejercicio: $V(t) = 300 \sin(100\pi \cdot t + \pi/2) [V]$

- **Amplitud máxima ($A$):** $300 V$.
- **Pulsación angular ($\omega$):** $100\pi \text{ rad/s}$.
- **Frecuencia ($f$):** Como $\omega = 2\pi f \Rightarrow 100\pi = 2\pi f \Rightarrow f = \mathbf{50 Hz}$.
- **Fase inicial ($\phi$):** $\pi/2$ (o $90^\circ$).
- **Valor eficaz ($V_e$):** Para una senoidal, $V_e = A / \sqrt{2} \approx 300 \cdot 0,707 = \mathbf{212,1 V}$.
- **Valor medio ($V_m$):** El valor medio de un ciclo completo es 0. Si se considera el semiperíodo (como suele hacerse en telecomunicaciones para estas señales), es $0,637 \cdot A = \mathbf{191,1 V}$.

---

### 5. Rango de Longitud de Onda en FM (88 a 108 MHz)

Utilizando la fórmula $\lambda = c/f$, donde $c \approx 3 \times 10^8 m/s$:
1M= 1 millón = 1*10^6
- Para $88 MHz$: $\lambda = \frac{300.000.000}{88.000.000} \approx \mathbf{3,41 m}$.
- Para $108 MHz$: $\lambda = \frac{300.000.000}{108.000.000} \approx \mathbf{2,78 m}$. 
El rango de variación es de **2,78 m a 3,41 m**.

---

### 6. Tren de Pulsos

_(El gráfico se visualiza como una sucesión de pulsos rectangulares de amplitud A y ancho $\tau$ repartidos en un período T)_

- **FRP (Frecuencia de Repetición de Pulsos):** Es la cantidad de pulsos por segundo, $FRP = 1/T$.
- **Ancho de pulso ($\tau$):** Duración del pulso activo.
- **Período (T):** Tiempo entre el inicio de un pulso y el inicio del siguiente.
- **Amplitud (A):** Nivel máximo de tensión o corriente del pulso.

---

### 7. Corriente Continua Equivalente

La expresión de la corriente continua equivalente que disipa la misma potencia $P$ que una corriente periódica $i(t)$ es el **Valor Eficaz ($I_e$)**: $$I_{DC} = I_e = \sqrt{\frac{1}{T} \int_0^T i(t)^2 dt}$$

---

### 8. Valor Medio de un Pulso Rectangular

Datos: $A = 10V$, ancho ($\tau$) $= 250 \mu s$, $T = 1 ms$. 
El valor medio ($V_m$) de un tren de pulsos rectangulares es: 
$$V_m = \frac{A \cdot \tau}{T}$$ 
$$V_m = \frac{10V \cdot 250 \times 10^{-6} s}{1 \times 10^{-3} s} = \mathbf{2,5 V}$$

