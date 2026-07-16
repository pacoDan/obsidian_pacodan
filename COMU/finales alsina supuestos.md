### Examen Final - 23 de mayo de 2023 / 01 de agosto de 2018

_(Nota: Este formato de examen aparece con mínimas variaciones en las secuencias de bits en distintas fechas)._

- **Condición para aprobar:** Dos puntos teóricos y dos puntos prácticos completos correctos.
    
- **Teoría 1:** Detallar los componentes de un Sistema Satelital Geoestacionario, indicando las funciones de cada uno.
    
- **Teoría 2:** Realice un esquema simple de la trama STM-1.
    
- **Teoría 3:** Dada la siguiente señal unipolar, realizar un gráfico comparativo, con las señales AMI y HDB-3.
    

```
SEÑAL UNIPOLAR
  ___ ___ ___                                       ___       ___
 |   |   |   |                                     |   |     |   |
_|___|___|___|___ ___ ___ ___ ___ ___ ___ ___ ___ _|___|___ _|___|___
   1   1   1   0   0   0   0   0   0   0   0   0     1   0     0   1
```

_(Secuencias evaluadas según la variante de la fecha: `0000000001001`, `1110000000001001`, `110000000001001`)._

- **Práctica 1:** Un módem de datos está transmitiendo a una velocidad de **9600 bps**. ¿Cuál sería el período de la señal si quisiéramos transmitir a **9600 baudios**? ¿Y si usáramos cuatribits?
    
- **Práctica 2:** Para una fibra óptica monomodo cuya atenuación es de **0,25 dB/Km**, calcular la potencia óptica a **100 Km** de una fuente de **0,1 mW** si se instala un amplificador de **35 db** en el enlace.
    
- **Práctica 3:** Calcular en ancho de banda en **GHz/Km**, sabiendo que cuando se genera en el trasmisor un pulso de **1,2 nanosegundos** se obtiene a la llegada del receptor, otro de **1,5 nanosegundos**.
    
----

### Examen Final - Tema 1 (Sin fecha especificada)

- **Nota:** Para aprobar se deben responder correctamente tres problemas (puntos 1 a 4) y un punto teórico (puntos 5 y 6).
    
- **1.** Dada una señal senoidal representada por $S(t)=A \sin(wt)$ donde $A=7$ volts y $w=4.000$ radianes /seg, que debe ser digitalizada mediante un Codec que utiliza 16 niveles de cuantificación uniformes. Hallar:
    
    - a) La frecuencia de muestreo necesaria para reconstruir la señal analógica inicial.
        
    - b) El periodo de la señal moduladora y el de la frecuencia de muestreo.
        
    - c) la velocidad de transmisión de la señal a la salida del Codec y el tiempo de transmisión de un bit.
        
- **2.** Se tiene un modem cuyo tipo de modulación es 4-PSK que transmite la siguiente secuencia de bits `1 1 0 0 0 0 1 0 1 1`. Indicar:
    
    - a) Que señales son analógicas y cuales digitales, considerar las señales, moduladora $s(t)$, modulada $m(t)$ y portadora $p(t)$.
        
    - b) Proponer una asignación de fases para cada secuencia de bits, en la cual para la combinación de ceros no se produzca cambio de fase. Realizar el diagrama de fases y graficar $m(t)$ para la asignación propuesta.
        
    - c) ¿Qué relación existe entre la velocidad de modulación y la velocidad de transmisión? Como se denomina este tipo de transmisión y que ventajas ofrece.
        
- **3.** Dado un enlace de **2 km** de longitud, en el cual se emplea un medio de transmisión cuya atenuación es **2 db/100 metros**, un amplificador que amplifica la señal **1000** veces y se utilizan bobinas de **0,5 Km**. Hallar la potencia mínima que debería tener el transmisor, en dbm y en mw, suponiendo que los empalmes y los conectores tienen una atenuación de **1 db** cada uno (considerar que el amplificador requiere dos conectores), el factor de diseño es de **3 db** y la sensibilidad del receptor es **10 microwatt**.
    
- **4.** Dada la siguiente secuencia de bits `1110000000001001` graficar la codificación de los mismos según el código unipolar positivo NRZ, Miller y el HDB3 (estos dos últimos son códigos RZ). Indicar los códigos que requieren menor ancho de banda y los que eliminan la componente continua, detallar los fundamentos de cada caso.
    
- **5.** Dado un sistema PCM 30 detallar la distribución de los canales de usuario, de señalización y de sincronismo e indicar cuantos bits componen cada trama y como se llega a la velocidad de **2,048Mbps**.
    
- **6.** En la verificación de un cableado UTP detallar el significado y construir un ejemplo de los siguientes parámetros: Delay Skew, paradiafonia, NEXT y FEXT.
    
---

### Examen Final - Tema 2 (Sin fecha especificada)

- **1.** Dada una linea que tiene los siguientes parámetros distribuidos: $L=2$ micro Henrio / Km, $C=0,058$ micro Faradio / Km, se solicita:
    
    - a) Calcular la frecuencia para la cual la línea tiene solo impedancia resistiva.
        
    - b) ¿Qué sucede con la reactancia capacitiva y la reactancia inductiva en este caso?
        
    - c) La impedancia característica de un cable coaxil de que factores depende?
        
    - d) Cite un ejemplo de linea balanceada y otro de linea desbalanceada.
        
- **2.** Se dispone de un canal de comunicaciones cuyo espectro este situado entre **10 y 11 MHz**, por otro lado la relación señal a ruido es de **24 db**. Calcular la capacidad máxima del canal y determinar utilizando la fórmula de Nyquist ¿cuántos niveles de señalización serían necesarios para alcanzar dicha capacidad máxima?
    
- **3.** Demostrar la expresión que permite calcular la capacidad máxima de un canal real cuando el ancho de banda ($\Delta f$) crece ilimitadamente. Graficar la capacidad del canal en función del ancho de banda.
    
- **4.** Dado un tren de pulsos proveniente de una fuente que emite secuencias binarias conformadas por 12 bits de los cuales siempre existen 4 unos en la secuencia, calcular:
    
    - a) la información suministrada con la aparición de un nuevo uno o de cero y la entropía de la fuente.
        
    - b) La tasa de transmisión de la fuente si los pulsos (símbolos) tienen un ancho de **0,1 mseg**.
        
- **5.** Dado un tren de pulsos rectangulares con simetría par demostrar la fórmula que permite graficar el espectro de amplitud de la serie de Fourier de dicho tren de pulsos. Indicar como se calcula el ancho de banda que necesita el medio de transmisión.
    
- **6.** Detallar el espectro electromagnético indicando para cada banda las frecuencias extremas y la longitud de onda de las mismas, además indicar los usos mas comunes para las comunicaciones, como así también, el tipo de transmisión que tiene preponderancia para cada banda (terrestre, ionosférica y directa).
    
----

### Examen Final - (Evaluación de PDH/SDH, sin fecha)

- **Nota:** Para aprobar se debe responder en forma completa dos puntos.
    
- **1.** Dado el sistema PDH indicar el nivel digital E1 cuantos canales tiene, detallar la distribución de los canales de usuario, y los canales de señalización y de sincronismo e indicar cuantos bits componen cada trama y como se obtiene la velocidad de **2,048Mbps**. Si el sistema fuera SDH indicar cual es la estructura de la trama STM1 y que capacidad tiene.
    
- **2.** Se tiene un enlace en el cual la potencia de transmisión es **0,010 watts**, la atenuación del cable $= 0,5~db/100$ metros, el factor de diseño $= 1$ db, la atenuación de los conectores $= 1$ db y la atenuación de los empalmes $= 1$ db. El receptor tiene una sensibilidad de **10 dbm** y se instalara en el enlace un amplificador que amplifica la señal 10 veces. Si las bobinas de cable son de **1 km** hallar:
    
    - a. potencia de transmisión en dbm.
        
    - b. longitud máxima del enlace.
        
    - c. cantidad de empalmes.
        
    - d. la sensibilidad del receptor en watts.
        
- **3.** Dado un tren de pulsos con simetría par, hallar la expresión del espectro de amplitud de la Serie Compleja de Fourier. Demostrar que la mayor concentración de energía está entre $0$ y $\pi$, en consecuencia son las armónicas más importantes para construir el pulso. El ancho de banda del medio debería contemplar las armónicas hasta la frecuencia $n \cdot \omega_0 = \pi$.
    
----

### Examen Final - 31 de julio de 2019

- **Nota:** Para aprobar el examen se deben responder en forma correcta y completa un punto teórico y tres prácticos.
    

**Teoría**

- **1) Transmisión en banda base:**
    
    - a) Detallar cuáles son los objetivos buscados por esta técnica.
        
    - b) Indique cuáles son los medios de trasmisión donde se aplica.
        
    - c) Describir cuáles son las aplicaciones actuales de esta técnica.
        
- **2) Fibra óptica:**
    
    - a) Detallar las diferencias constructivas entre FO monomodo y multimodo.
        
    - b) Indicar cuáles son las atenuaciones unitarias típicas para ambos casos.
        
    - c) Describir qué son las ventanas ópticas y cuáles son sus valores numéricos.
        

**Práctica**

- **1)** Mediante un canal telefónico normalizado de **3,1 kHz** de ancho de banda y **30 dB** de S/N se conecta dos módems de **9.600 bit/s** para enviar datos asincrónicos. Calcular el tiempo de trasmisión para enviar **5.000 caracteres** en código ASCII.
    
- **2)** Dibujar la forma de onda de una señal que trasmite la siguiente secuencia: `0101 0101 0011 0011 0000 1111 0011 0011 0101`, utilizando los siguientes códigos:
    
    - a) NRZ
        
    - b) Manchester
        
    - c) Manchester diferencial
        
    - d) Miller
        
- **3)** Calcular la velocidad de un canal de datos que transporta una señal digitalizada de un canal telefónico normalizado mediante el procedimiento PCM.
    
- **4)** Calcular la ganancia de las antenas para un enlace por microondas de **20 km** en **8 GHz**, que trasmite **1 mW** y el receptor tiene un umbral de **-85 dBm**, si las antenas están separadas **10 m** de los equipos y el cable atenúa **2 dB/100 m**.
    


----

### Examen Final - 21 de diciembre de 2022

**Teóricos**

- **1.** ¿Dónde se traslada la información en una señal analógica y donde en una señal digital? Explique ¿Cómo soluciona cada una para minimizar el fenómeno de atenuación y distorsión? ¿Cómo se mide la calidad de un canal analógico y en uno digital?
    
- **2.** ¿Cuáles son los parámetros más relevantes a medir en la certificación del cableado estructurado?
    

**Prácticos**

- **3.** Se instala un radioenlace entre dos puntos geográficos ubicados a **30 km**, manteniendo la línea de vista sin obstáculos, con los siguientes datos:
    
    - Ganancia de las antenas: **30 dB** cada una.
        
    - Longitud de la línea de transmisión para cada antena usando coaxil RG 213/U: **10 m**.
        
    - Atenuación de línea de transmisión: según tabla adjunta.
        
    - Potencia de salida del transmisor: **150 W**.
        
    - Frecuencia de operación: **250 MHz**.
        
    - Ancho de banda del canal: **100 KHz**.
        
    - Atenuación en el espacio libre $(dB) = 32,4 + 20 \log f(MHz) + 20 \log d(km)$.
        
    - Sensibilidad del receptor: **1 mW**.
        
    - S/N del canal: **30 dB**.
        

|**Frecuencia de operación [MHz]**|**10**|**50**|**100**|**200**|**400**|**1000**|
|---|---|---|---|---|---|---|
|RG 213/U Atenuación [dB/100m]|2|4.9|7|10.5|15.5|26|
|_(Fuente tabla adjunta)_|||||||

Calcular: La potencia que se recibe en el extremo es detectada por el equipo receptor? Cuánta potencia en mW llega?

- **4.** Se quiere transmitir datos por un canal telefónico que permite una velocidad de modulación de **2400 baudios**, a una velocidad de transmisión de **7200 bps**. Se cuenta con un módem que opera con modulación M-PSK. Cuantas fases se emplean y qué cantidad de bits se necesitan para su codificación. Proponer el diagrama de estados y el cuadro con la mejor asignación de combinación de bits a cada fase. ¿Cómo se Ilama la modulación empleada?
    
- **5.** Dado el siguiente mensaje a transmitir $M(x) = 1~0~1~0~1~1~0~1~1$ teniendo como polinomio generador $G(x)=x^4+x+1$. Aplicar el método para detección de errores CRC determinando:
    
    - a. Información a transmitir.
        
    - b. Calcular la eficiencia de la transmisión.
        
    - c. Repetir el procedimiento del lado del receptor.
        
----

### Examen Final - 20 de julio de 2022

- **Condición para aprobar:** tres puntos correctos = 6 (seis).
    
- **1.** Explique el significado de los **600 Ohms**, en la relación entre dBm y dBu.
    
- **2.** Hallar la relación S/N para un amplificador con voltaje de señal de salida de **4 V**, voltaje de ruido de salida de **0,005 V** y resistencia de entrada y de salida de **50 $\Omega$**.
    
- **3.** Para una fibra óptica monomodo con atenuación de **0,25 dB/Km**, calcular la potencia óptica a **100 Km** de una fuente de **0,1 mW**.
    
- **4.** Calcular en ancho de banda en **GHz/Km** sabiendo que cuando se genera en el trasmisor un pulso de **1,2 nanosegundos** se obtiene a la llegada del receptor, otro de **1,5 nanosegundos**.
    
- **5.** Para un canal que transmite en modo serie, calcular la velocidad de transmisión para el caso de utilizar CUATRIBITS y tener pulsos de ancho $\tau = 1$ microsegundo y FRP = 100.000 PPS. ¿Qué valor toma la velocidad de modulación (Vm) y cuál es el ancho de banda que deberia tener el canal según análisis de la serie compleja de Fourier?
    
- **6.** a. Detallar los 5 niveles de multiplexación PDH, e indicar la velocidad de transmisión y la cantidad de bits por trama. b. Detallar como está constituida la trama E1.
    
-----

### Examen Final - Agosto 2022 (Tema 1)

- **Nota:** Para aprobar se requiere responder correctamente 2 problemas (puntos 1 a 4) y un punto teórico (puntos 5 y 6).
    
- **1.** Para el siguiente enlace calcular la Potencia del transmisor para que el enlace funcione correctamente. La potencia a la salida del amplificador es de **1mW** (antes del conector). ¿Cuál es la atenuación del medio expresada en db/km?
    
    - $PTx = ? dBm$, $SRx = 0.5~mW$
        
    - $L1 = 1000m$, $L2 = 500m$
        
    - Conector $At = 0.75~dB~c/u$
        
    - At F.O. = ?
        
    - $GA \text{ (Ganancia del Amp)} = 5~dB$
        

Plaintext

```
              L1                          L2
  +----+              +-----------+               +----+
  | Tx |------[]------|    Amp    |------[]-------| Rx |
  +----+              +-----------+               +----+
```

_(Esquema del enlace transcrito a ASCII)._

- **2.** Para un canal que transmite en modo serie, calcular la velocidad de transmisión para el caso de utilizar TRIBITS y tener pulsos de ancho $\tau = 10$ microsegundos y FRP = 100.000 PPS. Qué valor toma la velocidad de modulación $(vm)$ y cuál es el ancho de banda que debería tener el canal según análisis de la serie compleja de Fourier?
    
- **3.** Se quiere transmitir imágenes de TV que tienen **625 líneas** con **500 puntos** por linea. Cada punto tiene 128 niveles equiprobables de brillo. Si se transmiten 20 imágenes por segundo. Calcular la tasa de información y el ancho de banda que debería tener el canal de comunicaciones si la relación señal a ruido $(S/N)$ del mismo es de **30 db**.
    
- **4.** Dada la siguiente secuencia de bits, graficar las señales resultantes utilizando los códigos Manchester, Manchester Diferencial y Miller. Indicar sus principales características. Secuencia binaria: `01100110111110000`.
    
- **5.** Detalle cinco ventajas de la transmisión digital en comparación con la analógica?
    
- **6.** Demostrar que cuando el ancho de banda de un medio tiende a infinito la capacidad de transmisión de un canal real no tiende a infinito.