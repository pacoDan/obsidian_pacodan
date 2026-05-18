Para calcular la cantidad de información de una **palabra**, el procedimiento general consiste en determinar primero la información que aporta cada uno de sus componentes (letras o caracteres) de forma individual y luego **sumar esos valores**.

De acuerdo con la **Teoría de la Información** detallada en las fuentes, los pasos y conceptos clave son los siguientes:

### 1. Cálculo de la información de un símbolo individual

La cantidad de información ($I$) de un suceso o mensaje (en este caso, una letra) depende de su **probabilidad de ocurrencia** ($P$). Se utiliza la siguiente expresión matemática:

$$I(x_i) = \log_a \left( \frac{1}{P(x_i)} \right)$$

- **Relación con la probabilidad:** Cuanto **menor es la probabilidad** de que aparezca una letra, **mayor es el contenido de información** que suministra al aparecer.
- **Unidades de medida:** La unidad depende de la **base del logaritmo** utilizada:
    - **Shannon (o Llano):** Base 2 (es la unidad por defecto si no se aclara otra).
    - **Nat:** Logaritmo natural (base $e$).
    - **Hartley:** Base 10.

### 2. Cálculo de la palabra completa

Una vez obtenida la información de cada carácter, se realiza la **sumatoria** del aporte de cada uno para obtener el total de la palabra.

**Ejemplo práctico de las fuentes (Palabra de 4 caracteres):** Si se tiene un alfabeto de **32 símbolos equiprobables** (cada uno con probabilidad $1/32$) y se quiere calcular la información de una palabra de **4 caracteres**:

1. **Información de un carácter:** $\log_2(32) = 5$ Shannons.
2. **Información de la palabra:** $5 \text{ Shannons} \times 4 \text{ caracteres} = 20 \text{ Shannons}$.

### 3. Diferencia con la Entropía

Es importante no confundir la información de una palabra específica con la **entropía**. Mientras que la información se calcula sobre un mensaje o palabra concreta, la **entropía ($H$)** representa la **información promedio** o el grado de incertidumbre media de la fuente que genera esos símbolos.

En resumen, para una palabra como "cable", se calcularía el aporte en Shannons de la 'c', la 'a', la 'b', la 'l' y la 'e' (basándose en sus respectivas probabilidades en el idioma) y se sumarían todos los resultados.