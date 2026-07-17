que entendemos por mudulacion, que es que consiste , para que sirve , que elementos hay
señales intervinientes
Esquema basico de modulacion 

métodos de modulación, para que existe
la modulacion por onda continua para que sirve 
que es la moduladora, que es la portadora

clasificacion de las tecnicas de modulacion 

esquema de la clasificación de modulacion 

tipos de modulación

modulación por onda continua , por portadora constante 
modulación AM 
modulación AM FM 
modulación de fase
modulación de onda continua 

asignación de secuencia de bits y estados, diagrama de fases


multiplexación, que es y para que sirve
que son las distintas técnicas de multiplexación


multiplexación FDM, para que aplica, para que sirve 
multiplexación TDM que es , que consiste, como viaja la información

multiplexación STDM 

Digitalización de una señal analógica, los tres pasos o etapas

----



### I. Generalidades de la Modulación

**¿Qué es la modulación?** Es un **proceso** que consiste en transformar una señal que lleva la información en otro tipo de señal más adecuada para que viaje por un medio de transmisión (espacio libre, cable coaxial, fibra óptica, etc.). La información en su formato nativo a menudo no puede viajar directamente; por ejemplo, una onda sonora no viajaría por un cable sin ser adaptada.

**¿Para qué sirve?** Sirve fundamentalmente para **adaptar la información al medio de comunicación**. Sin esta adaptación, la señal no llegaría a su destino o se degradaría rápidamente.

**Elementos y Esquema Básico de Modulación:** El esquema básico se compone de tres señales y dos dispositivos principales:

1. **Señal Moduladora:** Es la señal nativa que contiene la información (ej. la voz o datos de una PC).
2. **Señal Portadora:** Es una señal periódica (como una senoidal o un tren de pulsos) que será modificada por la moduladora.
3. **Señal Modulada:** Es el resultado del proceso, la señal ya adaptada que viaja por el medio.
4. **Modem:** Equipo compuesto por un **modulador** (que realiza la transformación para enviar) y un **demodulador** (que recupera la información al recibir).

---

### II. Clasificación de las Técnicas de Modulación

La clasificación depende de la naturaleza de la portadora y la moduladora:

1. **Modulación Continua:** La portadora es **analógica** (senoidal).
    - **Analógica:** Moduladora analógica (voz). Técnicas: **AM, FM, PM**.
    - **Digital:** Moduladora digital (datos PC). Técnicas: **ASK, FSK, PSK, QAM**.
2. **Modulación Pulsada:** La portadora es un **tren de pulsos** (digital).
    - **Analógica:** Moduladora analógica. Técnicas: **PAM, PDM, PPM**.
    - **Digital:** Moduladora digital. Técnicas: **PCM, Modulación Delta**.

---

### III. Modulación por Onda Continua (AM, FM, PM)

En estas técnicas se actúa sobre uno de los tres parámetros de la función senoidal de la portadora: **amplitud, frecuencia o fase**.

- **Modulación AM (Amplitud):** La moduladora varía la **amplitud** de la portadora. Genera una "envolvente" que sigue la forma de la señal de información.
    - _Ejemplo:_ La radio AM comercial tiene mayor alcance pero menor calidad de audio y es más sensible al ruido.
- **Modulación FM (Frecuencia):** Se varía la **frecuencia** de la portadora. La amplitud permanece constante.
    - _Ejemplo:_ La radio FM comercial ofrece alta calidad de audio (estéreo) pero menor alcance que la AM.
- **Modulación de Fase (PM):** Se actúa sobre la fase de la portadora. Es común en aplicaciones como radares, pero en comunicaciones digitales evoluciona a **PSK**.

**Asignación de bits y estados (Diagrama de Fases):** En modulaciones digitales como **PSK** o **QAM**, se utilizan diagramas fasoriales o **constelaciones**.

- **M-PSK:** La fase salta entre "M" valores. Por ejemplo, **8-PSK** tiene 8 saltos de fase, transportando **3 bits por símbolo** ($2^3=8$).
- **Código de Gray:** Se utiliza para que entre estados adyacentes solo haya **1 bit de diferencia**, minimizando el error en caso de ruido.
- **QAM (Modulación en Cuadratura):** Combina saltos de **fase y amplitud**. Es más eficiente frente al ruido que PSK porque distribuye mejor los estados en el espacio.

---

### IV. Digitalización de una Señal Analógica (PCM)

Para convertir una señal analógica (como la voz) en digital (PCM), se siguen tres pasos:

1. **Muestreo:** Se toman muestras periódicas. Según el **Teorema de Nyquist**, la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima de la señal.
    - _Ejemplo:_ Para voz (4 kHz), se toman **8,000 muestras por segundo**.
2. **Cuantificación:** Se aproximan los niveles infinitos de las muestras a niveles **cuánticos discretos** preestablecidos. Esto genera el **ruido de cuantificación**. Se usan leyes no uniformes como la **Ley A** (Europa/Argentina) o **Ley Mu** (EE.UU.) para mejorar la calidad.
3. **Codificación:** Se asigna un código binario a cada nivel cuantificado.
    - _Resultado:_ En la Ley A, con 8 bits por muestra y 8,000 muestras/seg, se obtiene la velocidad estándar de **64 kbps**.

---

### V. Multiplexación

**¿Qué es?** Es una técnica para transmitir **simultáneamente varias comunicaciones** sobre un mismo medio físico sin que se interfieran. **¿Para qué sirve?** Para aprovechar al máximo el ancho de banda y reducir costos de infraestructura.

**Técnicas de Multiplexación:**

- **FDM (División de Frecuencia):** Divide el ancho de banda del canal en subcanales de frecuencia para cada usuario. Es de naturaleza **analógica** y requiere bandas de resguardo para evitar interferencias.
    - _Ejemplo:_ Tecnología **ADSL**, donde el cable de cobre lleva voz, datos de subida y datos de bajada en diferentes frecuencias.
- **TDM (División de Tiempo):** El recurso compartido es el **tiempo**. Cada usuario dispone de una ranura o "slot" de tiempo en una trama periódica. Es de naturaleza **digital**.
    - _Ejemplo:_ **PCM 30 (E1)**, que agrupa 30 canales de información en una trama de 2.048 Mbps.
- **STDM (Estadística):** Variante de TDM que asigna slots solo a quienes tienen datos para transmitir, optimizando el uso del canal.
- **WDM:** División por longitud de onda (usado en fibra óptica).
- **CDM:** División por código (cada usuario tiene un código único).

---

### VI. Recopilación para Exámenes: Preguntas y Respuestas

- **¿Cuál es el único caso donde la señal modulada es digital?** Cuando tanto la moduladora como la portadora son digitales (ej. PCM).
- **¿Qué diferencia hay entre 16-PSK y 16-QAM?** Ambas tienen la misma velocidad binaria (4 bits/símbolo), pero **16-QAM es más resistente al ruido** porque sus estados están más separados. Para igualar el rendimiento, 16-PSK requeriría mucha más potencia.
- **¿De dónde sale el número "mágico" de 64 kbps?** De multiplicar 8,000 muestras/seg (Nyquist para voz) por 8 bits por muestra (Codificación Ley A).
- **¿Qué es el ruido granular?** Es el error que se produce durante la **cuantificación** al aproximar el valor real de una muestra al nivel cuántico más cercano.
- **¿Qué es una trama E1?** Es el estándar europeo de primer orden (PCM 30) que transmite a **2.048 Mbps** y contiene 32 canales (30 de voz, 1 de sincronismo y 1 de señalización).
- **¿Qué es la señalización?** Toda la información necesaria para el establecimiento, mantenimiento y liberación de una comunicación (ej. tonos de marcado, ocupado).
- **Diferencia entre Topología Malla y Estrella:** La malla es muy confiable por su redundancia pero costosa en enlaces ($n(n-1)/2$). La estrella usa un concentrador central; es más barata pero si el concentrador falla, toda la red cae.

---

### VII. Lecciones Aprendidas y Ejemplos de Tráfico

- **Ingeniería de Tráfico:** Las redes no se dimensionan para el 100% de los usuarios simultáneos, sino en base a un análisis de la **intensidad de tráfico (Erlang)**.
- **Ejemplo de saturación:** En un estadio de fútbol o una manifestación, la red celular suele colapsar porque hay más usuarios de los que la infraestructura local puede atender (sobrecarga).
- **Migraciones:** Los cambios en sistemas de comunicaciones (vuelcos) deben hacerse en horarios de **tráfico mínimo**, usualmente de madrugada.