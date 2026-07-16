### 1. Familia Unipolar

- **Unipolar NRZ (No Retorno a Cero):** Es el más simple y de menor costo.
    - **Puntos débiles:** Posee una componente continua importante que impide el paso por transformadores y aumenta la probabilidad de errores. Pierde el sincronismo ante largas secuencias de bits iguales.
    - **Uso:** Transmisión simple de información del usuario.
- **Unipolar RZ (Retorno a Cero):** Los pulsos duran la mitad del intervalo.
    - **Ventaja:** Permite ganar o mantener sincronismo en los "1" lógicos gracias a la transición central.
    - **Desventaja:** Requiere el **doble de ancho de banda** que el NRZ y mantiene componente continua no nula.

### 2. Familia Polar

- **Polar NRZ:** Utiliza dos niveles de polaridad opuesta (+V y -V).
    - **Punto fuerte:** Mayor **resistencia a errores** que el unipolar debido a la mayor distancia de voltaje entre estados. Requiere poco ancho de banda.
    - **Debilidad:** Pierde sincronismo si hay secuencias largas de ceros o unos seguidos.
- **Polar RZ (Autosincronizante):** Es considerado el código por excelencia para la sincronización.
    - **Ventaja principal:** Es **totalmente autosincronizante**, ya que garantiza una transición en cada bit, independientemente de la secuencia de datos.
    - **Desventaja:** Gran demanda de ancho de banda por la estrechez de sus pulsos.

### 3. Familia Bipolar (Pseudo-ternaria)

- **Bipolar NRZ (o AMI - Alternative Mark Inversion):** Los ceros son nivel nulo y los unos alternan polaridad (+ y -).
    - **Ventaja:** **Elimina la componente continua** (promedio cero), facilitando el paso por transformadores. Usa poco ancho de banda.
    - **Debilidad:** Pierde sincronismo ante largas secuencias de ceros.
- **HDB-3 (Alta Densidad Binaria 3):** Se basa en AMI pero soluciona su punto débil.
    - **Punto fuerte:** No permite más de tres ceros seguidos, insertando "bits de violación" para forzar transiciones y mantener el sincronismo. No tiene componente continua.
    - **Uso:** Muy utilizado en cables de cobre para velocidades de hasta 34 Mbps.

### 4. Códigos de Fase y Especiales

- **Manchester (Bifase):** El "1" es una transición positiva y el "0" una negativa en la mitad del intervalo.
    - **Fortalezas:** Excelente recuperación de reloj y eliminación de componente continua.
    - **Uso:** Estándar en redes **Ethernet (IEEE 802.3)**.
- **Manchester Diferencial:** La información va en las transiciones al inicio del intervalo (solo para el "0").
    - **Ventaja:** No necesita identificar la polaridad del intervalo para decodificar.
    - **Uso:** Redes **Token Ring (IEEE 802.5)**.
- **Miller:** Garantiza transiciones mínimas (una cada dos intervalos).
    - **Punto fuerte:** Concentra la potencia en un **ancho de banda mucho menor** que el Manchester.
- **4B-3T:** Relaciona 4 bits binarios con 3 dígitos ternarios.
    - **Ventaja:** Disminuye el ancho de banda necesario en un 25%.
    - **Uso:** Transmisiones de alta velocidad (140 Mbps) sobre cable coaxial.

### Semejanzas y Diferencias Clave

- **Semejanzas:** Todos son **señales digitales en banda base** que adaptan la señal binaria original para viajar por un medio físico sin modulación.
- **Diferencias:**
    - **Ancho de Banda:** Los códigos **RZ y Manchester** consumen más recursos que los **NRZ, Miller o 4B-3T**.
    - **Sincronismo:** Los códigos **autosincronizantes** (Polar RZ, Manchester) son superiores en fiabilidad temporal frente a los NRZ.
    - **Componente Continua:** Los códigos **unipolares** son los más deficientes, mientras que los **bipolares (AMI, HDB-3)** son los más eficientes para acoplamientos inductivos.