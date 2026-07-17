De acuerdo con las fuentes, las principales causas de errores en las redes de datos se derivan de la degradación de la señal al viajar por el canal de comunicaciones y de la incompatibilidad entre la señal y el medio físico.

Las causas técnicas más relevantes son:

- **Ruido:** Es uno de los fenómenos principales que degrada la señal mientras se propaga por el canal. Los canales reales siempre presentan algún nivel de ruido que puede alterar los bits transmitidos.
- **Distorsión y Limitación del Ancho de Banda:** Si el canal tiene un ancho de banda acotado o insuficiente para los componentes de frecuencia de la señal, esta se deforma. Una señal muy distorsionada por el filtrado del canal termina produciendo errores de interpretación en el receptor.
- **Atenuación:** A medida que la señal recorre una distancia por el vínculo físico, va perdiendo amplitud y potencia. Si la señal llega al receptor por debajo del "umbral de detección", no puede ser reconstruida correctamente.
- **Interferencia Inter-símbolo (ISI):** Ocurre cuando los pulsos de la señal se "pisan" o interfieren entre sí debido a la distorsión o al uso de un ancho de banda inadecuado.
- **Pérdida de Sincronismo:** Si el emisor y el receptor no mantienen sus bases de tiempo alineadas (frecuencia del clock), los bits se desplazan temporalmente, provocando que se lean valores incorrectos. Esto es común en códigos como el **AMI** cuando hay secuencias largas de ceros.
- **Exceso de Velocidad de Transmisión:** Si se intenta transmitir a una velocidad binaria mayor a la capacidad del canal (determinada por el ancho de banda y la relación señal-ruido), se incrementa automáticamente la probabilidad de error.
- **Componente de Corriente Continua (CC):** La presencia de "frecuencia cero" puede causar pérdidas de información al atravesar transformadores o acoplamientos inductivos en la línea, lo que se traduce en errores en la recepción.

Para evaluar la magnitud de estos problemas, se utiliza el parámetro **BER** (_Bit Error Rate_ o tasa errónea de bits), que mide la cantidad de bits recibidos con error respecto al total transmitido y sirve como factor de calidad del canal digital.