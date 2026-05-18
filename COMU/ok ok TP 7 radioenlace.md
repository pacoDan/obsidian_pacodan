los medios de propagación de frente de ondas , que pasa en radio-enlace,que pasa en onda ionosférica, que pasa en onda terrestre, en onda terrestre?  
en enlace satelital, los satélites de comunicaciones en que tipo de órbitas giran y que forma describen  
a que tipo de diagrama de relación corresponden a las antenas de radioenlace

no hay cables sino frente de ondas

modos de propagación, a que frecuencias, a que mas se debe

que pasa con las ondas ionosfericas y las ondas terrestres, cuales son los modos de propagación

capas de la atmósfera

tipos de antenas irradiantes, como es su diagrama , ganancia y directividad

como esta compuesto un sistema satelital, componentes de una comunicación satelital

ver transpondedor, tranceptores, que son y para que se usa

tipos de satelites de comunicaciones

retardos en sistemas satelitales

==Que pasa con la longitud y frecuencia de las antenas de media onda? como es el calculo de longitud y frecuencias? Que pasa con las demás antenas?==

-----

La relación entre la longitud física de una antena y la frecuencia de la señal que transmite o recibe es fundamental, ya que la construcción de la misma debe ser **resonante** con la longitud de onda ($\lambda$) de trabajo.

### 1. Antenas de Media Onda ($\lambda/2$)

- **Concepto:** En este tipo de antena, la longitud total del conductor es igual a la mitad de la longitud de onda de la frecuencia de operación.
- **Distribución:** Es común que estas antenas estén **alimentadas en el centro**, dividiéndose en dos secciones de un cuarto de onda ($\lambda/4$) cada una.
- **Resonancia:** Se diseñan para ser exactas a una frecuencia específica ($f_1$); si la frecuencia cambia mucho, la antena deja de ser adecuada y la señal llega **atenuada**.

### 2. Cálculo de Longitud y Frecuencia

El cálculo se basa en la relación de la velocidad de la luz y la frecuencia:

- **Fórmula base:** $\lambda = \frac{v}{f}$, donde $v$ es la velocidad de la luz y $f$ es la frecuencia.
- **Cálculo práctico para antenas:**
    - Para obtener la longitud en metros usando la frecuencia en MHz, se utilizan expresiones simplificadas.
    - **Cuarto de onda ($\lambda/4$):** $L = \frac{75}{f (MHz)}$.
    - **Media onda ($\lambda/2$):** Siguiendo la lógica anterior, la constante sería 150 ($L = \frac{150}{f (MHz)}$).
- **Ejemplo:** Para una frecuencia mayor, la longitud de onda es menor y, por lo tanto, la antena será más pequeña.

### 3. Otros tipos de Antenas

- **Antenas de Cuarto de Onda ($\lambda/4$):** Son muy comunes y consisten en un conductor de un cuarto de onda conectado al transmisor, mientras que el otro cuarto de onda es **constituido por la Tierra**. Por esta razón, el equipo de radio debe estar conectado a tierra para que esta actúe como conductor y complete la media onda necesaria para el funcionamiento.
- **Antenas de Onda Completa:** Existen, aunque son menos mencionadas como estándar común frente a las de cuarto y media onda.
- **Antenas Parabólicas (Microondas/Satelitales):** Se usan para frecuencias muy altas (VHF, UHF, SHF). Su tamaño no depende solo de la frecuencia, sino de la **ganancia** deseada: a mayor tamaño de parábola, mayor ganancia, aunque se vuelve más difícil de instalar.

### 4. Consideraciones Técnicas Importantes

- **Relación Tamaño-Frecuencia:** Existe una regla visual directa; las antenas más pequeñas trabajan en **frecuencias más altas**, mientras que las antenas más grandes corresponden a **frecuencias más bajas**.
- **Sintonización Eléctrica:** En dispositivos como las radios comerciales, donde no se puede cambiar físicamente el tamaño de la antena para cada estación, se utilizan circuitos que **alargan o acortan eléctricamente** la longitud de la antena para mantener la resonancia.
- **Polarización:** La orientación de la antena (vertical u horizontal) debe coincidir entre transmisor y receptor. Si son diferentes, se produce una **atenuación de 3 dB**.

---

### I. Fundamentos de Antenas e Irradiación

Las antenas son conductores que tienen la capacidad de **liberar (transmisión) o captar (recepción)** ondas electromagnéticas en el espacio libre.

- **Conceptos Clave:**
    - **Ganancia:** Se mide en **dbi**, comparando la antena real con una **antena isotrópica** (una antena ideal puntual que emite en todas las direcciones). A mayor tamaño de parábola, mayor ganancia, aunque esto dificulta su instalación.
    - **Directividad y Diagramas:** Representan cómo se distribuye la energía en el espacio. Pueden ser:
        - **Omnidireccionales:** Distribuyen energía en todas las direcciones.
        - **Direccionales (Unidireccionales/Bidireccionales):** Concentran la energía en lóbulos. Las de radioenlace suelen tener un lóbulo principal muy marcado.
    - **Polarización:** Determinada por la orientación del **campo eléctrico** (que deriva de la corriente eléctrica). Puede ser vertical, horizontal, circular o elíptica.
        - **Pregunta de Examen:** ¿Qué pasa si el transmisor y el receptor tienen distinta polarización? Se produce una **atenuación de 3 dB**.
    - **Resonancia:** La antena debe estar construida en relación con la **longitud de onda ($\lambda$)** de la frecuencia de trabajo. Una antena "sintonizada" es aquella que es resonante a una frecuencia específica para evitar pérdidas.

### II. Modos de Propagación y la Atmósfera

En las comunicaciones por radio no hay cables, sino un **frente de ondas** propagándose por el espacio.

- **Capas de la Atmósfera:**
    1. **Tropósfera:** La más baja, donde ocurre el modo troposférico.
    2. **Estratósfera:** Capa intermedia.
    3. **Ionósfera:** Entre 60 y 350 km de altura. Es vital para las comunicaciones porque sus moléculas de gas se cargan eléctricamente (iones). Posee las capas **D, E y F**.
- **Modos de Propagación:**
    - **Onda Terrestre:** Compuesta por:
        - **Onda Directa:** Línea de vista entre antenas (VHF/UHF). Limitada por el horizonte y la curvatura de la Tierra.
        - **Onda Reflejada:** Se refleja en la superficie terrestre.
        - **Onda de Superficie:** La onda viaja "pegada" a la Tierra, que actúa como conductor. Prevalece en **Media Frecuencia (MF)**.
    - **Onda Ionosférica:** La señal sufre **refracciones sucesivas** en la ionósfera que parecen una reflexión (efecto espejo), permitiendo alcances de miles de kilómetros.
        - **Detalle Crítico:** La ionósfera es **más densa de día** (capas F1 y F2) y **menos densa de noche** (solo capa F2), lo que obliga a cambiar las frecuencias de corte para mantener el enlace.

### III. Sistemas de Radioenlace y Comunicaciones Satelitales

#### Radioenlaces Terrestres

Se basan en la **onda directa** y operan usualmente en VHF y UHF.

- **Componentes:** Transmisor, receptor, antena y línea de transmisión (como guías de onda en frecuencias muy altas).
- **Repetidores:** Se usan para sortear obstáculos o la curvatura terrestre.
    - **Activos:** Amplifican y retransmiten la señal.
    - **Pasivos:** Superficies conductoras que actúan como espejos.
- **Antenas:** Utilizan reflectores parabólicos con un **iluminador** en el foco para concentrar la señal en forma de haz. En lugares con nieve se protegen con un **radomo**.

#### Enlace Satelital

Un satélite es esencialmente un **repetidor activo en el espacio**.

- **Componentes del Sistema:**
    - Estaciones terrenas (terminales).
    - El satélite (segmento espacial).
    - Estación de telemetría, telecomando y control (gobierno del satélite).
- **El Transpondedor:** Es el corazón del satélite. Recibe la señal, la amplifica y la retransmite en otra frecuencia (transmisor-receptor).
- **Órbitas Satelitales:**
    - **LEO (Órbita Baja):** 150-450 km. Giran muy rápido (~1.5 horas por vuelta).
    - **MEO (Órbita Media):** 9,000-18,000 km. Usada por sistemas GPS.
    - **GEO (Geoestacionaria):** A 36,000 km sobre el Ecuador. Son **geosincrónicos** (periodo de 24 horas), por lo que parecen fijos desde la Tierra.
    - **HEO (Órbita Elíptica Alta):** Describen una elipse, alejándose y acercándose a la Tierra.
- **Retardos (Latencia):** Dependen de la altura. LEO tiene 1-7 ms; MEO 35-85 ms; y **GEO llega a los 270 ms** solo de ida (más de medio segundo ida y vuelta).

### IV. Preguntas y Respuestas para Exámenes (Lecciones Aprendidas)

1. **¿Por qué las frecuencias de subida (Up-link) y bajada (Down-link) son distintas?**
    - **Respuesta:** Para evitar la **interferencia mutua** entre el receptor y el transmisor del satélite.
2. **¿Dónde se aplica la frecuencia más alta en un enlace satelital, en la subida o en la bajada?**
    - **Respuesta:** Se aplica en la **subida (f1)**. La frecuencia más alta tiene mayor atenuación, pero en tierra tenemos potencia eléctrica disponible para compensarlo. En el satélite (bajada, f2) se usa la frecuencia más baja para ahorrar energía de las baterías y paneles solares, ya que tiene menos atenuación.
3. **¿Qué factores afectan la disponibilidad del enlace satelital?**
    - **Respuesta:** Los **eclipses** (especialmente en satélites GEO al alinearse con el sol) y la **lluvia**, que atenúa la señal.
4. **¿Qué es la "pisada" de un satélite?**
    - **Respuesta:** Es el área de cobertura geográfica en la superficie terrestre donde se puede recibir la señal.
5. **¿Qué define la vida útil de un satélite?**
    - **Respuesta:** El **combustible** disponible para realizar correcciones de posición y mantener el control desde tierra. Al agotarse, el satélite se vuelve basura espacial.

### V. Multiplexación (Breve Resumen Técnico)

Es la técnica para transmitir simultáneamente varias comunicaciones por un único vínculo.

- **FDM (División de Frecuencia):** Técnica analógica. Divide el ancho de banda en subcanales mediante modulación (traslación de frecuencia).
- **TDM (División de Tiempo):** Técnica digital. Asigna ranuras de tiempo (_time slots_) a cada usuario. Requiere un **sincronismo perfecto**.
- **Jerarquías Digitales:**
    - **PDH (Plesiócrona):** Relojes independientes, poco flexible.
    - **SDH (Síncrona):** Más moderna, flexible, permite el concepto de _Add-Drop_ (bajar canales sin demultiplexar todo).


