En las redes de datos, el tratamiento de errores se agrupa bajo el concepto general de **control de errores**, el cual se divide en dos acciones fundamentales: la **detección** y la **corrección**. Para llevar a cabo estas tareas, los sistemas se basan en la **redundancia**, que consiste en agregar datos adicionales a la información original, lo que implica un compromiso técnico entre el nivel de protección y la eficiencia de la transmisión.

A continuación, se detallan las principales políticas y técnicas empleadas según las fuentes:

### 1. Métodos de Detección de Errores

Estos métodos permiten identificar si un bloque de datos ha llegado corrupto, sirviendo como base para las políticas de corrección posterior:

- **Paridad:** Es el método más simple y consiste en añadir un bit (o varios) para que la suma de "unos" sea siempre par o impar. Existen variantes como la **paridad vertical**, la **longitudinal** (VRC/LRC) y la **cíclica**. Su limitación principal es que solo es apta para detectar una cantidad impar de errores.
- **CRC (Chequeo de Redundancia Cíclica):** Es una técnica avanzada basada en operaciones matemáticas polinomiales. El emisor y el receptor utilizan el mismo "polinomio generador"; si al dividir el mensaje recibido por dicho polinomio el resto es distinto de cero, se confirma la presencia de un error. Se emplea comúnmente en la **capa 2 (enlace de datos)** del modelo OSI.
- **Checksum (Suma de verificación):** Se basa en sumar los valores decimales de las secuencias binarias y enviar el resultado como un campo adicional para su comprobación en el destino.

### 2. Corrección hacia Atrás: ARQ (Automatic Repeat Request)

Es la política más utilizada en redes de datos y consiste en la **retransmisión** de los mensajes que llegaron con error. Su funcionamiento se apoya en tres elementos clave: **ACK** (confirmación positiva), **NAK** (confirmación negativa) y **Time Out** (tiempo de espera agotado).

Presenta dos variantes principales:

- **Stop and Wait (Parar y esperar):** El emisor transmite un paquete y no envía el siguiente hasta recibir el ACK del receptor. Requiere buffers pequeños pero es ineficiente en grandes distancias.
- **Sliding Window (Ventana deslizante):** Permite enviar una cantidad determinada de paquetes (llamada "ventana") sin esperar confirmación inmediata. Es más rápido y permite realizar **control de flujo**. Dentro de esta técnica existe la **repetición selectiva**, donde solo se retransmite el paquete específico que falló.

### 3. Corrección hacia Adelante (FEC - Forward Error Correction)

A diferencia del ARQ, esta técnica no requiere que el receptor solicite una retransmisión. Su funcionamiento se basa en:

- **Envío diferido:** Transmitir dos veces el mismo mensaje en tiempos distintos.
- **Punto a multipunto:** Es ideal para comunicaciones donde un emisor envía datos a varias estaciones simultáneamente.
- **Limitación:** Si ambos mensajes llegan con error, el sistema no puede reconstruir la información.

### 4. Códigos Autocorrectoras

Existen códigos, como el **código Hamming**, que tienen la capacidad inherente de detectar y corregir errores en un bit sin necesidad de retransmitir, basándose puramente en su estructura matemática.

### Aplicación por Niveles (Modelo OSI)

En la práctica, las redes suelen aplicar **varias técnicas simultáneamente** en distintos niveles del modelo OSI para incrementar la eficiencia. Por ejemplo, se puede aplicar detección en la **capa 2** para asegurar el enlace y corrección de extremo a extremo en la **capa 4 (transporte)**.