Los **code smells** (hedores de código) son heurísticas o síntomas en el código que indican un potencial problema de diseño. No son necesariamente errores de programación, sino evidencias de que el diseño planteado tiene falencias que dificultan el mantenimiento o la extensión del sistema.

A continuación se detallan los principales code smells identificados en las fuentes, con sus respectivos ejemplos:

### 1. Type Test (Prueba de Tipos)

Ocurre cuando se utiliza una sentencia condicional (`if`, `switch` o `instanceof`) para preguntar por el tipo de un objeto y decidir qué comportamiento ejecutar.

- **Ejemplo:** En un sistema bancario, preguntar mediante un booleano `esDeposito` si un movimiento es un depósito o una extracción para decidir si se suma o resta el saldo.
- **Solución recomendada:** Aplicar **polimorfismo**, creando subclases o implementaciones distintas que sepan cómo comportarse por sí mismas.

### 2. Primitive Obsession (Obsesión por los Primitivos)

Consiste en utilizar tipos de datos básicos (como `String`, `Integer` u `Object`) para representar conceptos que tienen una entidad propia en el dominio.

- **Ejemplo 1:** Representar el "Tipo de Prenda" (saco, camisa, pantalón) como un simple `String` en lugar de una clase o un **Enum**.
- **Ejemplo 2:** Manejar las condiciones climáticas de una API externa como una `List<Map<String, Object>>` en lugar de crear una clase `Clima` o `EstadoDelTiempo`.
- **Solución recomendada:** **Cosificar** (reificar) el concepto en una clase o Enum para ganar abstracción y uniformidad.

### 3. God Object (Objeto Dios)

Se refiere a una clase que asume demasiadas responsabilidades o centraliza la lógica de todo el sistema, volviéndose un "gran hermano" de los objetos.

- **Ejemplo:** Una clase `Validador` que se encarga de validar absolutamente todos los datos de todas las entidades del sistema (prenda, material, color, usuario), en lugar de que cada objeto valide su propio estado.
- **Solución recomendada:** Distribuir las responsabilidades siguiendo el principio de **alta cohesión**.

### 4. Empty Class (Clase Vacía)

Es una clase que no posee comportamiento propio y solo sirve como contenedor de datos (getters y setters), lo que suele derivar en un modelo de dominio anémico.

- **Ejemplo:** Crear la clase `Atuendo` solo para agrupar una lista de prendas, pero sin que tenga lógica de negocio asociada en la iteración actual.
- **Solución recomendada:** No modelar la clase hasta que se identifique una responsabilidad clara en el dominio.

### 5. Long Method (Método Largo)

Sucede cuando un método es excesivamente extenso y realiza múltiples tareas que podrían estar separadas.

- **Ejemplo:** Un método `sacar` en una cuenta bancaria que realiza simultáneamente la validación de montos negativos, el chequeo de saldo suficiente, el cálculo del nuevo saldo y el registro del movimiento.
- **Solución recomendada:** Utilizar el refactor **Extract Method** (extraer método) para dividir la lógica en partes más pequeñas y legibles.

### 6. Duplicated Logic (Lógica Duplicada)

Es la repetición de fragmentos de código idénticos o muy similares en diferentes partes del sistema.

- **Ejemplo:** Tener exactamente el mismo código para validar que un monto no sea negativo tanto en el método `depositar` como en el método `extraer`.
- **Solución recomendada:** Extraer la lógica común a un método auxiliar o a una superclase (usando **Template Method**).

### 7. Magic Numbers (Números Mágicos)

Uso de valores numéricos hardcodeados (fijos) en el código sin una explicación clara de su significado.

- **Ejemplo:** Multiplicar un importe por `0.01` en una fórmula de recargo de tarjeta sin definir ese valor como una constante o parámetro configurable.

### 8. Feature Envy (Envidia de Funcionalidad)

Ocurre cuando un método parece estar más interesado en los datos de otra clase que en los de la propia.

- **Ejemplo:** Un método en la clase `Movimiento` que modifica directamente el saldo de la clase `Cuenta`, en lugar de que la cuenta gestione su propio saldo tras recibir un movimiento.
- **Solución recomendada:** Mover el comportamiento a la clase que posee los datos (delegación).