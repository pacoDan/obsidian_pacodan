De acuerdo con las fuentes, el **BER** (_Bit Error Rate_ o tasa errónea de bits) es el parámetro fundamental para medir la **calidad de los canales y señales digitales**.

### Fórmula del BER

Aunque las fuentes no presentan una expresión matemática con símbolos específicos, definen el concepto de la siguiente manera:

**$$BER = \frac{\text{Cantidad de bits recibidos con error}}{\text{Cantidad total de bits transmitidos}}$$**

### Consideraciones pertinentes

- **Unidad de medida:** Generalmente se expresa mediante **notación científica** (por ejemplo, $10^{-6}$ o $10^{-9}$), lo que indica cuántos bits erróneos aparecen por cada millón o mil millones de bits enviados.
- **Factor de calidad:** Es el indicador que determina si un enlace digital es aceptable o no. Cuanto menor sea el valor del BER, mayor es la calidad de la red.
- **Compromiso técnico (Protección vs. Eficiencia):** Para mejorar el BER (reducir los errores), es necesario agregar **redundancia** (datos adicionales). Sin embargo, cuanta más redundancia se añade para proteger la información, menor es el **rendimiento o eficiencia** de la transmisión, ya que se ocupan recursos del canal en bits que no son de información útil.
- **Relación con el Ancho de Banda y la Velocidad:**
    - Si se intenta transmitir a una **velocidad mayor** que la capacidad del canal (determinada por el teorema de Shannon-Hartley), el BER aumenta significativamente.
    - Una limitación o acotamiento excesivo del **ancho de banda** de la línea deforma los pulsos, provocando errores de interpretación en el receptor.
- **Causas que afectan el BER:** Los fenómenos que degradan el BER incluyen el **ruido** en canales reales, la **atenuación**, la **distorsión** y la **interferencia inter-símbolo (ISI)**, esta última a menudo causada por la pérdida de sincronismo.
- **Relación Señal/Ruido (SNR):** Existe una dependencia directa entre la potencia de la señal útil frente al ruido y la probabilidad de error; un canal con mucha potencia de ruido tendrá un BER elevado.