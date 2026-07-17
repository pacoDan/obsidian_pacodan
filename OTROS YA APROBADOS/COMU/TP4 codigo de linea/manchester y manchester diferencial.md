De acuerdo con las fuentes, los códigos **Manchester** y **Manchester Diferencial** (ambos considerados códigos "bifase") son de naturaleza **polar**, lo que significa que efectivamente **ocupan valores tanto positivos como negativos** de manera simétrica.

Esta característica es fundamental por las siguientes razones:

- **Equilibrio de niveles:** Debido a que ambos códigos siempre presentan una transición en la **mitad del intervalo de bit**, la señal pasa exactamente la **misma cantidad de tiempo** en el nivel positivo que en el nivel negativo para cada bit transmitido.
- **Aporte de CC nulo:** Al alternar constantemente entre polaridades positivas y negativas dentro de un mismo intervalo de bit, el **valor medio de la señal es cero**. Esto garantiza que el aporte de corriente continua (CC) sea nulo, permitiendo que la información atraviese transformadores o acoplamientos inductivos sin degradarse.
- **Diferencia en la interpretación:**
    - En el **Manchester estándar (IEEE 802.3)**, el nivel positivo y negativo se usa para definir la dirección de la transición (por ejemplo, de alto a bajo para un "0" y de bajo a alto para un "1").
    - En el **Manchester Diferencial**, aunque la señal sigue oscilando entre valores positivos y negativos, la información no depende del valor absoluto del voltaje, sino de si ocurre una **transición adicional al inicio del intervalo** del bit.

En resumen, el uso de ambos polos (positivo y negativo) es lo que permite que estos códigos sean **autosincronizantes** y mantengan una línea de transmisión libre de componentes de corriente continua.

---

De acuerdo con las fuentes, los códigos **Manchester** y **Manchester Diferencial** (también conocidos como bifase) son métodos de codificación de línea donde la información se transporta a través de **transiciones** de la señal en lugar de niveles de voltaje absolutos.

### Código Manchester (Bifase)

Es el estándar utilizado en redes **Ethernet (IEEE 802.3)**. Su principal característica es que **siempre** existe una transición en la **mitad del intervalo de bit**.

- **Regla de formación (IEEE 802.3):**
    - **Bit 0:** Transición **descendente** (de nivel alto a bajo) en la mitad del intervalo.
    - **Bit 1:** Transición **ascendente** (de nivel bajo a alto) en la mitad del intervalo.
- **Nota:** Existe una variante (Thomas) que invierte estas reglas (1 es descendente y 0 es ascendente).


### Diferencias Principales

|Característica|Manchester|Manchester Diferencial|
|:--|:--|:--|
|**Ubicación de la información**|En la **dirección** de la transición central (ascendente o descendente).|En la **presencia o ausencia** de una transición al inicio del intervalo.|
|**Estándar asociado**|IEEE 802.3 (Ethernet).|IEEE 802.5 (Token Ring).|
|**Interpretación**|Depende del nivel absoluto al inicio para determinar la dirección.|Es diferencial; solo importa si hubo un cambio respecto al estado anterior al inicio del bit.|

### Principales Características (Comunes a ambos)

- **Autosincronismo:** Al garantizar al menos una transición por cada bit (en la mitad del intervalo), el receptor puede mantener su reloj alineado sin perder el ritmo, incluso ante secuencias largas de ceros o unos.
- **Eliminación de componente continua:** Debido a las constantes transiciones y al equilibrio de polaridades, presentan una excelente respuesta frente a la "frecuencia cero", evitando pérdidas en transformadores o acoplamientos inductivos.
- **Mayor ancho de banda:** Al tener transiciones frecuentes (mínimo una por bit), requieren un **mayor ancho de banda** en comparación con códigos simples como el NRZ.
- **Detección de errores:** Permiten una detección de errores básica, ya que si en la mitad de un intervalo de bit no se detecta una transición, el sistema sabe que ha ocurrido una falla en la línea.