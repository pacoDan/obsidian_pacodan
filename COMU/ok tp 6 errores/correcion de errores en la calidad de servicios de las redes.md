La corrección de errores incide directamente en la **calidad de servicio (QoS)** de las redes, actuando como el factor que determina la fiabilidad y la integridad de los datos transmitidos, aunque genera un **compromiso técnico** con la eficiencia del sistema.

A continuación se detalla cómo esta relación afecta el desempeño de las redes según las fuentes:

### 1. El BER como factor de calidad

En los canales digitales, la calidad se evalúa fundamentalmente a través del **BER** (_Bit Error Rate_ o tasa errónea de bits). Este parámetro mide la cantidad de bits recibidos con error respecto al total transmitido; por lo tanto, una política de corrección efectiva busca reducir el BER para mantener un estándar de calidad aceptable.

### 2. El compromiso entre Protección y Eficiencia

La corrección de errores se basa en la **redundancia**, que consiste en agregar datos adicionales al mensaje original. Esto genera un impacto contrapuesto en la calidad de la red:

- **Mayor Protección:** A mayor cantidad de datos redundantes, aumenta la capacidad de detectar y corregir errores, lo que incrementa la **confiabilidad** de la comunicación.
- **Menor Eficiencia:** Al aumentar los bits totales para una misma cantidad de información útil, el **rendimiento** (eficiencia de transmisión) disminuye. El ingeniero debe resolver este compromiso para no sobrecargar el canal con demasiada redundancia que ralentice la red innecesariamente.

### 3. Impacto en la Velocidad y Latencia

Dependiendo de la técnica de corrección empleada, la percepción de calidad del usuario puede variar:

- **Técnicas ARQ (Corrección hacia atrás):** Al basarse en la retransmisión de paquetes, pueden introducir retardos significativos. Por ejemplo, en distancias grandes (como comunicaciones satelitales), el método _Stop and Wait_ es muy ineficiente y lento, afectando negativamente la velocidad percibida.
- **Ventana Deslizante:** Mejora la calidad al permitir enviar múltiples paquetes sin esperar confirmación inmediata, lo que optimiza la rapidez de la transferencia.
- **FEC (Corrección hacia adelante):** Evita la necesidad de retransmitir al enviar el mensaje por duplicado o con códigos correctores, lo cual es ideal para comunicaciones de tiempo real o punto a multipunto, aunque no es 100% infalible si ambos mensajes llegan corruptos.

### 4. Control de Flujo y Estabilidad

Los métodos de corrección, especialmente los basados en confirmaciones (**ACK/NAK**), funcionan también como mecanismos de **control de flujo**. Esto permite que el receptor gobierne la comunicación, evitando que los buffers se saturen y colapsen, lo cual previene caídas en la calidad del servicio por desbordamiento de datos.

### 5. Robustez mediante el Modelo OSI

Para maximizar la calidad, las redes modernas no confían en una sola técnica. Se aplican distintas capas de protección siguiendo el **modelo OSI**: se puede detectar un error en la **capa 2** (enlace) para asegurar el tramo físico y aplicar correcciones de extremo a extremo en la **capa 4** (transporte). Esta acumulación de técnicas incrementa la eficiencia global y la seguridad de que la información llegue íntegra al usuario final.