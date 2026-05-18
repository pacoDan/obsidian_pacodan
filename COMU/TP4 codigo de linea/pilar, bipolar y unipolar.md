De acuerdo con las fuentes, la graficación y las características de los códigos de línea **Polar RZ**, **Bipolar RZ** y **Unipolar RZ** se definen mediante la combinación de su **polaridad** (unipolar, polar o bipolar) y su **ancho de pulso** (Retorno a Cero).

### 1. Unipolar RZ (Retorno a Cero)

- **Cómo se grafica:**
    - El **bit 1** se representa con un nivel de tensión (positivo o negativo) que ocupa solo la **mitad del intervalo de bit** ($T/2$), regresando a cero en la mitad del tiempo asignado.
    - El **bit 0** se mantiene en nivel **cero** durante todo el intervalo.
- **Principales características:**
    - **Sincronismo:** Al tener una transición en la mitad del pulso para cada "1", facilita el mantenimiento del sincronismo en comparación con su versión NRZ.
    - **Componente Continua (CC):** Posee una componente de corriente continua (valor medio no nulo), lo que puede causar problemas en líneas con transformadores.
    - **Ancho de Banda (AB):** Requiere un **mayor ancho de banda** que los códigos NRZ debido a que el pulso es más estrecho ($tau < T$).

### 2. Polar RZ (Retorno a Cero)

- **Cómo se grafica:**
    - Ambos símbolos tienen polaridad: el **bit 1** suele ser positivo y el **bit 0** negativo.
    - Tanto el pulso positivo del "1" como el pulso negativo del "0" **retornan a cero en la mitad del intervalo** de bit ($T/2$).
- **Principales características:**
    - **Autosincronizante:** Es considerado el código con la **mejor performance de sincronismo**, ya que garantiza una transición en cada bit, independientemente de la secuencia.
    - **Componente Continua:** Si la cantidad de "ceros" y "unos" está equilibrada, la componente de CC se anula o reduce al mínimo.
    - **Desventaja:** Su principal costo es el **alto requerimiento de ancho de banda** (aproximadamente el doble que un código NRZ).

### 3. Bipolar RZ (Retorno a Cero)

- **Cómo se grafica:**
    - El **bit 0** se mantiene siempre en nivel **cero**.
    - El **bit 1** tiene polaridad, pero esta **alterna** entre positivo y negativo para cada "1" consecutivo (similar al código AMI).
    - A diferencia del AMI estándar (que es NRZ), en el Bipolar RZ el pulso del "1" **retorna a cero a la mitad del intervalo** ($T/2$).
- **Principales características:**
    - **Eliminación de CC:** Gracias a la alternancia de polaridades en los "unos", ayuda eficazmente a reducir o eliminar la componente continua.
    - **Sincronismo:** Ofrece un sincronismo robusto para secuencias de "unos" debido a las transiciones adicionales del retorno a cero, aunque sigue siendo vulnerable a la pérdida de ritmo ante cadenas largas de "ceros".
    - **Ancho de Banda:** Al ser un código RZ, demanda un **ancho de banda elevado** en comparación con el Bipolar NRZ (AMI).