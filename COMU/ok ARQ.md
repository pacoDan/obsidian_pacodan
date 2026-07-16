Dentro del tratamiento de errores, el **ARQ** se clasifica como una **técnica especial de transmisión para la corrección de errores**. Específicamente, se la conoce como una técnica de **corrección hacia atrás** (_backward error correction_), ya que requiere de un diálogo con el emisor para subsanar la falla.

A continuación, se explica a fondo su funcionamiento y utilidad según las fuentes:

### ¿Qué es el ARQ y para qué sirve?

El nombre proviene de las siglas en inglés _**Automatic Repeat Request**_ (Requerimiento de Repetición Automático). Su función principal es **corregir errores mediante la retransmisión** de los bloques de datos.

Además de la corrección, el ARQ sirve como un método de **control de flujo**. Esto es posible porque los mensajes de confirmación "gobiernan" la comunicación, permitiendo al receptor indicar al emisor si puede o no seguir enviando información.

### Funcionamiento detallado (La mecánica del diálogo)

A diferencia de otras técnicas, el ARQ se lleva a cabo estrictamente entre **dos estaciones** (punto a punto) debido a que requiere un diálogo constante. El proceso sigue estos pasos:

1. **Transmisión y detección:** El sistema emisor envía un paquete. Al recibirlo, el sistema receptor aplica un método de **detección de errores** (como paridad, CRC o suma de verificación) para verificar la integridad del bloque.
2. **Confirmación:** El receptor contesta con un mensaje específico según el resultado:
    - **ACK** (_Acknowledgment_): Conformidad positiva; el paquete llegó bien.
    - **NAK/NAC** (_Non-Acknowledgment_): Conformidad negativa; se detectó un error.
3. **Acción correctiva:** Si el emisor recibe un NAK, procede a la **retransmisión** del paquete fallido; si recibe un ACK, continúa con el siguiente mensaje.

### Elementos fundamentales del sistema

Para que el método sea efectivo, el ARQ incorpora varios conceptos técnicos:

- **Time-out (Tiempo de espera):** Es un parámetro que evita que el emisor espere indefinidamente. Si no recibe ninguna confirmación en un tiempo determinado (por ejemplo, si el mensaje se perdió en el camino), el sistema asume automáticamente que debe **retransmitir el paquete**.
- **Buffers:** Son espacios de memoria necesarios para almacenar los paquetes enviados. El emisor debe retener la información en el _buffer_ hasta recibir el ACK, por si fuera necesario repetir el envío ante un error.
- **Piggybacking (PB):** Es una técnica de optimización donde el mensaje de confirmación (ACK/NAK) no se envía solo, sino que se incluye en un "campito" dentro del encabezado de un paquete de datos que viaja en sentido contrario.

### Variantes de aplicación

Las fuentes distinguen dos modalidades principales de ARQ:

- **Stop and Wait (Parar y Esperar):** El emisor transmite un bloque y **se detiene a esperar** la confirmación antes de enviar el siguiente. Es sencillo pero ineficiente en grandes distancias o altas velocidades.
- **Sliding Window (Ventana Deslizante):** Permite enviar una **ventana** (cantidad determinada de paquetes) sin esperar confirmación inmediata, lo que hace la transferencia mucho más rápida. Dentro de esta, existen variantes como la **repetición selectiva** (solo se repite el paquete con error) o el **Go-Back-N** (se repite todo desde el error).