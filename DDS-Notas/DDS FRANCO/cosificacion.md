La **cosificación** (o reificación, que conceptualmente son sinónimos) consiste en **convertir un concepto, comportamiento, resultado, relación u operación en un objeto con entidad propia** (una clase). Este acto es una de las herramientas más valiosas en el diseño orientado a objetos, ya que nos permite representar y manipular ideas complejas de forma explícita en nuestro código, elevando el nivel de abstracción del sistema.

La cosificación está íntimamente ligada al **acto de nombrar**. "Aquello que no tiene nombre no existe"; si no podemos pensar en un nombre expresivo y con sentido para esa abstracción dentro de nuestro lenguaje ubicuo, sencillamente no podremos convertir la idea en un objeto sólido.

---

### Ejemplos comunes de cosificación

1. **Cosificar un comportamiento u operación**: En lugar de aplicar cambios destructivos directamente sobre las estructuras de datos (como duplicar un árbol de objetos completo para poder volver atrás en el tiempo), se reifica la operación misma. Esto se observa en la estructura del patrón **Command** (como en _Hitbug_), donde cada tipo de modificación posible se convierte en una clase polimórfica. Esto encapsula la acción, permitiendo ejecutar, deshacer (_undo_) o agrupar modificaciones fácilmente. Otro ejemplo es la cosificación del comportamiento de un `Recordatorio`.
2. **Cosificar un resultado**: Para evitar retornar estructuras de bajo nivel como tuplas, arreglos o códigos de error numéricos (que son propensos a confusión en el flujo normal), se puede cosificar el resultado. De este modo, creamos un objeto con estado propio que encapsula el éxito, el error y la información de la respuesta de manera clara.
3. **Cosificar una relación**: En lugar de manejar asociaciones crudas, podemos crear una clase relación intermedia. Por ejemplo, en _Copia.me_, la comparación entre documentos se cosifica en una clase `Revision`. Así, el documento conoce sus revisiones y puede calcular su estado en tiempo de ejecución de manera cohesiva y encapsulada.
4. **Cosificar conceptos clave del dominio**: En "Qué Me Pongo", en lugar de desarmar abstracciones y manipular listas de prendas sueltas, se cosifica la idea bajo los conceptos estructurados de un `Ítem` de venta o una `Sugerencia`, asegurando que las clases de negocio no pierdan su semántica.

---

### Errores que NO se deben cometer al aplicar la cosificación

- **Crear "clases vacías" o meras estructuras de datos sin comportamiento**: Uno de los errores más graves es cosificar un concepto que no aporta valor real al dominio (por ejemplo, crear una clase que solo implementa un método de comparación trivial que no es polimórfico con nada más). Cuando se le quita la lógica a un concepto cosificado, se incurre en una violación del paradigma de objetos y la clase pasa a ser una **estructura de datos pasiva (modelo de dominio anémico)** en lugar de un objeto propiamente dicho.
- **Asignar responsabilidades de coordinación global al comportamiento cosificado**: Cuando convertimos un comportamiento en un objeto (como en un comando), ese nuevo componente debe poseer una **visión y conocimiento sumamente limitados del sistema**. No debe saber para qué existe globalmente, cómo se controla el flujo de ejecución de la aplicación, ni qué otras opciones hay. Tampoco debe coordinar nada ni saber qué comandos se ejecutan antes o después; su único rol es encapsular e implementar su pequeño "cachito de algoritmo" cuando se le invoque.
- **Forzar la cosificación sin un nombre representativo**: El diseño de sistemas requiere de un pensamiento crítico y de la construcción de un vocabulario consistente con el cliente (_Ubiqutous Language_). Si no logras ponerle un nombre claro a tu clase, significa que el concepto que estás intentando cosificar tiene un **error de diseño o de abstracción subyacente**.

---
