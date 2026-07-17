Basado en las fuentes, las diferencias entre los códigos **AMI** y **Bipolar RZ**, así como sus requerimientos de ancho de banda (AB), se detallan a continuación:

### Diferencias entre AMI y Bipolar RZ

|Característica|Código AMI (_Inversion de Marca Alternada_)|Código Bipolar RZ (_Retorno a Cero_)|
|:--|:--|:--|
|**Formato de Línea**|Es un código **Bipolar NRZ** (No Retorno a Cero).|Es un código **Bipolar RZ** (Retorno a Cero).|
|**Regla de Formación**|El "1" alterna polaridad (positivo/negativo) y ocupa todo el intervalo de bit; el "0" es nivel cero.|El "1" alterna polaridad pero **retorna a cero** en la mitad del intervalo; el "0" es nivel cero.|
|**Sincronismo**|Tiene problemas de **pérdida de sincronismo** ante secuencias largas de ceros.|Ofrece **mejor sincronismo** que el NRZ debido a las transiciones adicionales que genera el retorno a cero.|
|**Componente Continua**|Ayuda a reducir o anular la componente continua si la cantidad de "unos" es equilibrada.|También reduce la componente continua por su naturaleza bipolar.|

### Requerimientos de Ancho de Banda (AB)

Los requerimientos de ancho de banda están directamente relacionados con la duración del pulso ($\tau$) en la línea:

- **Código AMI (Bipolar NRZ):** Al ser un código **NRZ**, el pulso ocupa la totalidad del intervalo de bit ($\tau = T$). Esto resulta en un **menor requerimiento de ancho de banda** en comparación con las versiones de retorno a cero.
- **Código Bipolar RZ:** Al ser un código **RZ**, el pulso hace una transición en la mitad del intervalo ($\tau < T$, típicamente $\tau = T/2$). Por esta razón, el código Bipolar RZ **ocupa más ancho de banda** que el AMI.

En resumen, mientras que el **Bipolar RZ** consume más ancho de banda, lo hace para ganar robustez en el **sincronismo** del receptor; por el contrario, el **AMI** es más eficiente en el uso del espectro (menor AB) pero es vulnerable a perder el ritmo ante largas cadenas de ceros.