A continuación, se presentan los ejercicios y ejemplos resueltos más importantes y repetitivos extraídos del material, abarcando los conceptos centrales de la transmisión de datos, como el cálculo de velocidades, la eficiencia, la detección de errores (Checksum y CRC) y la capacidad de los canales con ruido.

Dado que el libro reserva la sección general de resoluciones exclusivamente para docentes, se recopilan aquí los problemas que el texto desarrolla paso a paso en sus capítulos teóricos y guías de aplicación directa:

### 1. Cálculo de la Velocidad de Transmisión

- **Enunciado:** Calcular la velocidad de transmisión de una señal binaria sabiendo que su período es de 5 mseg.
    
- **Resuelto:**
    
    - El período de la señal es de 5 mseg, por lo que primero se obtiene su valor en segundos: 5 mseg = 0,005 seg.
        
    - Como la señal es binaria, el número de niveles de la señal es `n = 2`.
        
    - Se aplica la fórmula de velocidad de transmisión: `Vt = (1 / T) * log2(n)`.
        
    - `Vt = (1 / 0,005 seg) * log2(2)`.
        
    - El resultado de la velocidad de transmisión es `Vt = 200 bps`.
        

### 2. Cálculo de la Eficiencia de un Enlace Dedicado

- **Enunciado:** Se quiere conocer la eficiencia de un enlace dedicado establecido entre un centro de cómputos y una planta de producción. El enlace tiene una velocidad de transmisión contratada (Vt) de 250 Kbps. Se transmiten archivos mediante el protocolo FTP (usando un código de 8 bits por byte) obteniendo los siguientes tiempos: un archivo de 8 MB en 320 seg, uno de 5 MB en 210 seg, y otro de 9 MB en 450 seg.
    
- **Resuelto:** * Se debe calcular la velocidad real de transferencia (Vrtd) para cada caso.
    
    - Para 8 MB: equivale a 67.108.864 bits (8 * 1024 * 1024 * 8). `Vrtd = 67.108.864 / 320 = 209.715 bps`.
        
    - Para 5 MB: equivale a 41.943.040 bits. `Vrtd = 41.943.040 / 210 = 199.728 bps`.
        
    - Para 9 MB: equivale a 75.497.472 bits. `Vrtd = 75.497.472 / 450 = 167.772 bps`.
        
    - Se calcula el promedio aritmético de estas velocidades reales, lo que arroja un valor medio de `192.405 bps`.
        
    - La eficiencia del enlace es el cociente entre la velocidad real y la velocidad contratada: `Eficiencia = 192.405 / 250.000 = 0,7696`.
        

### 3. Detección de Errores por Suma de Verificación (Checksum)

- **Enunciado:** Se desea enviar un mensaje compuesto por cinco bytes de 8 bits cada uno. Estos son: S1 = 00100110, S2 = 01100100, S3 = 00100101, S4 = 01000100, y S5 = 00100100. Calcular el campo checksum que contendrá los bits de verificación.
    
- **Resuelto:**
    
    - Se realiza la suma binaria de los bytes transmitidos.
        
    - `S1 + S2 = 10001010`.
        
    - Sumando S3: `10001010 + 00100101 = 10101111`.
        
    - Sumando S4: `10101111 + 01000100 = 11110011`.
        
    - Sumando S5: `11110011 + 00100100 = 100010111`.
        
    - Como hay un acarreo (el bit excedente a la izquierda), este se suma al bit menos significativo (LSB): `00010111 + 1 = 00011000` (Suma total).
        
    - El checksum es el complemento a 1 de la Suma total, que consiste en convertir todos los ceros a unos y viceversa: `11100111`.
        

### 4. Detección de Errores por Control de Redundancia Cíclica (CRC)

- **Enunciado:** Se desea enviar un mensaje compuesto por la siguiente secuencia de bits: 1100000111. Sabiendo que el polinomio generador G(x) a utilizar es de grado r=5, calcular el resto y la secuencia completa que se debería transmitir para controlar la presencia de errores en el receptor.
    
- **Resuelto:**
    
    - Se define el polinomio a transmitir M(x) y se le agregan `r` ceros a la derecha (equivalente a multiplicar por x^r).
        
    - Se efectúa la división de la nueva secuencia generada por el polinomio generador G(x), utilizando álgebra de módulo 2 (OR Exclusiva).
        
    - Tras ejecutar la división módulo 2, el resto obtenido es: `11010`.
        
    - La secuencia final a transmitir consiste en la secuencia original seguida del resto obtenido: `110000011111010`.
        

### 5. Cálculo de la Relación Señal a Ruido (S/N)

- **Enunciado:** Calcular la relación señal a ruido (S/N) necesaria en un canal para obtener una capacidad de transmisión C = 10.000 bps disponiendo de un ancho de banda Δf = 3.000 Hz.
    
- **Resuelto:**
    
    - Se aplica el teorema de Shannon-Hartley: `C = Δf x log2 (1 + S/N)`.
        
    - Se despeja la relación S/N: `S/N = 2^(C/Δf) - 1`.
        
    - Pasando a decibelios: `S/N [dB] = 10 x log10 [2^(C/Δf) - 1]`.
        
    - Reemplazando los valores: `S/N = 10 x log10 [2^(10.000/3.000) - 1]`.
        
    - `S/N = 10 x log10 [2^(3,33) - 1]`.
        
    - El resultado aproximado es `S/N ≈ 9,58 dB`.
        

### 6. Velocidad Máxima de Transmisión de un Canal

- **Enunciado:** Calcular la máxima velocidad de transmisión en bps de un canal telefónico que tiene un ancho de banda Δf = 3.000 Hz y una relación señal a ruido S/N = 30 dB. Indicar además qué capacidad de módem se asimila a esta velocidad.
    
- **Resuelto:**
    
    - Primero, se debe transformar el valor de S/N de decibelios a magnitud absoluta: 30 dB equivalen a `10^3 = 1.000`.
        
    - Se utiliza nuevamente la fórmula de capacidad del canal de Shannon: `C = Δf x log2 (1 + S/N)`.
        
    - Se reemplazan los valores: `C = 3.000 x log2 (1 + 1.000)`.
        
    - `C = 3.000 x log2 (1.001)`.
        
    - El cálculo arroja `C ≈ 29.900 bps`.
        
    - Como la velocidad máxima de transmisión (VTX) teórica es igual a la capacidad C, el límite superior del canal será de `29,9 Kbps sincrónico`.


---


Basado en el **Capítulo 3: Técnicas de la transmisión de la información** (que aborda de lleno la transmisión de datos), he recopilado y categorizado los ejercicios resueltos ("Ejemplos" en el texto) más representativos e importantes. Estos ejercicios cubren los conceptos fundamentales y más repetitivos de la materia: cálculo de velocidades (modulación y transmisión), tasas de error (BER), eficiencia de enlaces y métodos de detección de errores (Checksum y CRC).

A continuación, presento los enunciados y sus respectivas resoluciones:

### 1. Velocidades de Modulación y Transmisión

Estos ejercicios son fundamentales para entender la diferencia entre la velocidad a la que cambian los estados en la línea (baudios) y la cantidad de información enviada (bps), tanto para señales binarias como multinivel.

**Ejercicio A: Velocidad de modulación (Servicio Télex)**

- **Enunciado:** Calcular la velocidad de modulación utilizada en el servicio télex, sabiendo que el período de la señal es T = 20 mseg = 0,02 seg.
- **Resolución:** La velocidad de modulación ($V_m$) es la inversa del período ($T$). $V_m = \frac{1}{T}$. Siendo $T = 0,02$ seg, resulta: $V_m = \frac{1}{0,02} = 50$ baudios.

**Ejercicio B: Velocidad de transmisión en señales binarias y multinivel**

- **Enunciado:** Calcular la velocidad de transmisión de una señal binaria ($n=2$) y una señal multinivel ($n=4$), si ambas tienen un período de $5$ mseg ($0,005$ seg).
- **Resolución:** Primero se calcula la velocidad de modulación para ambas, que es idéntica al tener el mismo período: $V_m = \frac{1}{0,005} = 200$ baudios. Para la **señal binaria** ($n=2$): La velocidad de transmisión ($V_t$) es $V_t = 200 \text{ bps}$. Para la **señal multinivel** ($n=4$): La cantidad de información transmitida es distinta porque cada pulso transporta más de un bit. La velocidad resulta: $V_t = 400 \text{ bps}$.

---

### 2. Tasa de Errores (BER - Bit Error Rate)

Este cálculo es repetitivo a la hora de dimensionar la calidad de un canal de comunicaciones frente al ruido.

**Ejercicio C: Cálculo de la Tasa de Errores**

- **Enunciado:** Una computadora recibe desde una fuente remota un total de 120 MB correspondientes a un archivo y a los datos de control. Si durante la transmisión se produjeron 480 bits con errores, ¿cuál es la tasa de errores, en BER, de esa transmisión? Se trabaja con un código de 8 bits por byte.
- **Resolución:** Primero se calcula la cantidad total de bits transmitidos: $120.000.000 \text{ bytes} \times 8 \text{ bits/byte} = 960.000.000 \text{ bits}$. Luego, se aplica la fórmula del BER: $BER = \frac{480}{960.000.000} = 0,0000005$. Por lo tanto, el $BER = 5 \times 10^{-6}$.

---

### 3. Eficiencia de un Enlace de Datos

Mide el rendimiento real de un canal en función del tiempo y la cantidad de información útil enviada.

**Ejercicio D: Rendimiento del sistema**

- **Enunciado:** Se quiere conocer la eficiencia de un enlace dedicado que tiene una velocidad de transmisión contratada de 250 Kbps. Como parte de las pruebas, se transfiere un archivo de 8 MB con un tiempo de transmisión de 320 seg, considerando 8 bits por byte.
- **Resolución:** Se calcula primero la cantidad exacta de bits: 8 MB son iguales a 8.388.608 bytes y, por lo tanto, representan $67.108.864$ bits. Esto da una velocidad real de transferencia ($V_{rtd}$) específica para ese archivo. Suponiendo un promedio calculado con varias transferencias que arroja una $V_{rtd}$ media de $192.405$ bps, la eficiencia ($\varepsilon$) se obtiene del cociente: $\varepsilon = \frac{192.405 \text{ bps}}{250.000 \text{ bps}} = 0,7696$. La eficiencia de este enlace es del **$76,96%$**.

---

### 4. Detección y Corrección de Errores

Estos son los ejercicios lógicos más importantes y reiterados en el área de protocolos de transmisión para asegurar la integridad de la información.

**Ejercicio E: Detección de errores por Checksum (Suma de verificación)**

- **Enunciado:** Se desea mandar un mensaje compuesto por cinco bytes de 8 bits cada uno. Estos serán los siguientes: $S1 = 00100110$, $S2 = 01100100$, $S3 = 00100101$, $S4 = 01000100$, $S5 = 00100100$. Calcular el campo _checksum_ que contendrá los bits de verificación.
- **Resolución:** El método consiste en sumar los bytes adyacentes y al resultado final aplicarle el complemento a 1. $S1 + S2 = 10001010$. $+ S3 = 10101111$. $+ S4 = 11110011$. $+ S5 = \textbf{10010111}$. Al aplicar el complemento a 1 (convertir todos los unos a ceros, y los ceros a unos), el campo de _checksum_ resultante será **$01101000$**.

**Ejercicio F: Control por Redundancia Cíclica (CRC)**

- **Enunciado:** Se desea mandar un mensaje compuesto por la secuencia de bits **1100000111**. Este se controlará por medio del polinomio generador: $G(x) = X^5 + X^4 + 1$ (cuyos coeficientes representan **11001**). Calcular el resto y la secuencia completa que se debería transmitir.
- **Resolución:**
    1. Se define un polinomio auxiliar del mismo grado que el generador ($r=5$), es decir, se agregan cinco ceros al final del mensaje original, quedando: $110000011100000$.
    2. Se efectúa la división de este nuevo polinomio por el polinomio generador ($11001$) empleando el álgebra de módulo 2 (operación XOR continua sin acarreo).
    3. El resultado de esta división arroja un **resto = 11010**.
    4. La secuencia completa a transmitir se forma uniendo el mensaje original y el resto: **$1100000111-11010$**.