El **CRC (Control de Redundancia Cíclica)** es una técnica de detección de errores de tipo **polinomial** que se basa en tratar las secuencias binarias como polinomios para realizar operaciones matemáticas. Este método es ampliamente utilizado en protocolos de capa de enlace, como Ethernet, debido a su alta eficacia para detectar errores aislados y en ráfaga.

### Polinomios M(x) y G(x)

La mecánica del CRC utiliza dos elementos fundamentales expresados en forma de polinomio:

- **Polinomio Mensaje M(x):** Es la representación del mensaje o secuencia de bits que se desea transmitir. Cada bit de la secuencia corresponde a un coeficiente del polinomio; por ejemplo, una secuencia de 4 bits tiene un orden $n=3$.
- **Polinomio Generador G(x):** Actúa como el **divisor** en la operación. Estos polinomios están **normalizados** por organismos internacionales como la UIT y son diseñados por matemáticos para maximizar la detección de errores. El orden de este polinomio se denomina **$r$**.

### Aplicación del Método (Mecánica)

El proceso para aplicar el CRC en el emisor y receptor sigue estos pasos:

1. **Desplazamiento del mensaje:** El polinomio mensaje $M(x)$ se multiplica por $x^r$, lo que equivale a **desplazar el mensaje $r$ posiciones** a la izquierda para dejar espacio al código de redundancia.
2. **División binaria:** Se realiza la división del mensaje desplazado entre el polinomio generador $G(x)$ utilizando aritmética binaria.
3. **Obtención del Resto:** Del cociente de la división solo interesa el **resto**, que tiene un orden de $r-1$. Este resto es el valor que se coloca en el campo de control del protocolo.
4. **Transmisión:** Se transmite el mensaje original seguido del resto calculado.
5. **Verificación en el receptor:** El receptor recibe la secuencia y la divide por el **mismo polinomio generador $G(x)$**.
    - Si el **resto es cero**, se asume que **no hay error**.
    - Si el **resto es distinto de cero**, se ha detectado un **error** en el bloque.

### Cálculo del Rendimiento

El **rendimiento de la transmisión** mide la eficiencia del sistema considerando la cantidad de datos útiles frente a los datos totales enviados. Se calcula con la siguiente fórmula:

$$\text{Rendimiento} = \frac{\text{bits de información}}{\text{bits totales}} \times 100$$.

Al añadir bits de CRC (redundancia), los bits totales aumentan, lo que genera un **compromiso técnico**: una mayor cantidad de bits de redundancia ofrece una **mayor protección contra errores**, pero resulta en un **menor rendimiento** o eficiencia de la transmisión. Por ejemplo, si se tienen 6 bits de información y se agregan 4 bits de resto, el rendimiento sería del 60%.

El **CRC-16** es uno de los más destacados por su alta performance, logrando detectar el 99% de los errores aislados y el 95% de los errores en ráfaga.