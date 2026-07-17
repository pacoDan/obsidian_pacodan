De acuerdo con las fuentes, la **entropía ($H$)** de una fuente es un parámetro de la Teoría de la Información que define el **valor medio o grado de incertidumbre media** de la ocurrencia de los símbolos generados por dicha fuente. En otras palabras, representa la **información promedio por mensaje** que la fuente es capaz de entregar.

### ¿Cómo se calcula?

La entropía se sustenta en el concepto de **esperanza matemática** y se calcula mediante la sumatoria del producto de la probabilidad de cada símbolo por su contenido de información individual.

La expresión matemática general es: **$$H = \sum_{i=1}^{n} P(x_i) \cdot I(x_i)$$**

Donde:

- **$P(x_i)$:** Es la **probabilidad de ocurrencia** del símbolo o suceso $i$.
- **$I(x_i)$:** Es la **cantidad de información** de ese símbolo, calculada como $\log_a(1/P(x_i))$.
- **Unidad de medida:** Generalmente se expresa en **Shannons (o Llanos) por símbolo** (utilizando logaritmo en base 2).

### Características clave de la entropía

- **Relación con la incertidumbre:** La entropía es una característica de la fuente; si una fuente entrega poca información útil, se dice que tiene "poca entropía". En contextos físicos, se asocia al **desorden**, lo cual en informática se traduce en una mayor cantidad de información potencial.
- **Valor Mínimo:** La entropía es **cero** cuando un mensaje tiene probabilidad 1 (es decir, ya se sabe con certeza qué va a ocurrir), por lo que no aporta información nueva.
- **Valor Máximo:** La entropía se **maximiza** cuando la fuente es **equiprobable** (todos los símbolos tienen la misma probabilidad de salir) y de **memoria nula** (el símbolo actual no depende del anterior).
    - Para una **fuente binaria equiprobable** (donde el 0 y el 1 tienen 50% de probabilidad), la entropía máxima es **1 Shannon por símbolo**.
    - En general, para un alfabeto de $n$ símbolos equiprobables, la entropía máxima es $\log_2(n)$.
- **Tasa de información:** A partir de la entropía, se puede calcular la **tasa de información de la fuente ($T$)**, que es la cantidad de información generada por unidad de tiempo ($H$ dividido por la duración promedio del símbolo), medida en **Shannons por segundo**.