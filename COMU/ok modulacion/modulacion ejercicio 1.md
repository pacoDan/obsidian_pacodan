La modulación **PCM** (Pulse Code Modulation), conocida en español como **MIC** (Modulación por Impulsos Codificados), es una técnica que consiste en la **digitalización de una señal analógica** para que pueda ser procesada y transmitida de forma digital. Es el único caso de modulación donde tanto la moduladora como la portadora y la señal resultante son digitales.

El proceso para transformar la señal analógica en digital mediante PCM consta de tres pasos fundamentales: **muestreo, cuantificación y codificación**.

### 1. El Muestreo (Muestreo)

En esta etapa, un dispositivo llamado muestreador toma muestras periódicas de la señal analógica.

- **Teorema de Nyquist:** Para que la señal pueda ser reconstruida fielmente, la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima de la señal original.
- **Caso de la voz:** Para un canal telefónico estándar de 4 kHz, se toman **8,000 muestras por segundo**.
- **Intervalo de tiempo:** Esto implica que entre una muestra y otra transcurren exactamente **125 microsegundos**.

### 2. El Proceso de Cuantificación (Cuantificación)

Una vez obtenidas las muestras (que aún tienen valores de amplitud infinitos y se consideran señales analógicas tipo PAM), estas pasan al cuantificador.

- **Aproximación de niveles:** El cuantificador aproxima el valor de cada muestra al nivel discreto "cuántico" más cercano preestablecido.
- **Ruido de cuantificación:** Dado que se trata de una aproximación, se produce una diferencia entre la señal original y la cuantificada, lo que genera un error conocido como **ruido granular o ruido de cuantificación**.
- **Leyes de cuantificación:** Para mejorar la calidad, se utilizan niveles **no uniformes** (más concentrados en ciertas amplitudes). Existen dos estándares principales: la **Ley A** (utilizada en Europa y Argentina) y la **Ley Mu** (utilizada en EE. UU. y Japón).

### 3. La Codificación (Codificación)

En el último paso, a cada nivel de voltaje cuantificado se le asigna un **código binario** específico.

- En la **Norma Europea (Ley A)**, se utilizan **8 bits por muestra**, lo que permite definir 256 niveles de cuantificación.
- **Velocidad de transmisión:** Al multiplicar las 8,000 muestras por segundo por los 8 bits de cada una, se obtiene la velocidad estándar de un canal de voz digital: **64 kbps (64,000 bits por segundo)**.

### Integración en Sistemas de Tráfico (PCM 30 y PCM 24)

La tecnología PCM no solo digitaliza canales individuales, sino que permite agruparlos mediante multiplexación por división de tiempo (TDM):

- **PCM 30 (E1):** Es el estándar europeo que agrupa 30 canales de información, 1 de sincronismo y 1 de señalización (32 canales en total), alcanzando una velocidad de **2.048 Mbps**.
- **PCM 24 (T1):** Es el estándar americano que agrupa 24 canales, operando a una velocidad de **1.544 Mbps**.