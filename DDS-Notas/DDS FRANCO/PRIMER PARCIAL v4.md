A continuación se presentan los **15 requerimientos de dominio, modelado y diseño de diagrama de clases** más importantes para la plataforma **Código a Voluntad**, estructurados bajo las cualidades de diseño, patrones GoF y principios orientados a objetos de la materia:

---

### 1. Evitar Clases Fachada "Dios" (Diseño de Responsabilidades)

- **Requerimiento**: El sistema no debe centralizar las acciones en una clase "orquestadora" o divina del estilo `Sistema` o `CodigoAVoluntad`, ni modelar una clase `Usuario` que actúe como fachada (`Façade`) de todos los casos de uso (como `colaborarEn()` o `crearProyecto()`) ****.
- **Modelado y Diagrama de Clases**: El mensaje de negocio `anotarse(Colaboradora)` debe ser recibido directamente por la clase de dominio que posee la información relevante (en este caso, `Proyecto`), impidiendo intermediarios innecesarios (`Middle Man`) o asignaciones de métodos fuera de su lugar (`Misplaced Method`) ****.

### 2. Representación Polimórfica del Compromiso Esperado (Patrón Strategy)

- **Requerimiento**: El tipo de compromiso de un `Proyecto` (X horas totales, semanales, mensuales o nulo) debe poder extenderse fácilmente sin modificar las clases existentes ni anidar estructuras condicionales rígidas (`if/else` o `switch`) sobre cadenas de texto para calcular el impacto temporal ****.
- **Modelado y Diagrama de Clases**: Se aplica el patrón **Strategy**. Se asocia la clase `Proyecto` con una interfaz o clase abstracta llamada `Compromiso` mediante composición ****. Sus implementaciones concretas serán `CompromisoTotal`, `CompromisoSemanal` y `CompromisoMensual` ****.

### 3. Manejo de Opcionalidad de Compromiso (Patrón Null Object)

- **Requerimiento**: El compromiso esperado en un proyecto es opcional. Para evitar el uso sistemático de `null` en el flujo de negocio y prevenir errores en tiempo de ejecución, el sistema debe comportarse de forma segura cuando no se especifica el compromiso ****.
- **Modelado y Diagrama de Clases**: Se implementa un **Null Object** llamado `CompromisoFlexible` (o `CompromisoVacio`) que implemente la interfaz `Compromiso` ****. Este objeto responderá consistentemente a las consultas de horas (por ejemplo, retornando 0 o ignorando cálculos temporales), evitando el lanzamiento de un `NullPointerException` ****.

### 4. Inmutabilidad en Catálogos Administrativos (Value Objects)

- **Requerimiento**: Las **Habilidades** que configuran el catálogo de la plataforma son registradas exclusivamente por el equipo de administración y no deben ser modificadas por colectivos ni colaboradores, asegurando la consistencia histórica ****.
- **Modelado y Diagrama de Clases**: La clase `Habilidad` debe diseñarse como un **Value Object** inmutable ****. Se omiten sistemáticamente los métodos de modificación (_setters_), y sus atributos (`titulo`, `codigo`, `descripcion`) se asignan estrictamente en el momento de la inicialización en su constructor ****.

### 5. Cohesión de las Habilidades (Evitar Anemic Domain Model)

- **Requerimiento**: La entidad `Habilidad` no debe ser diseñada como una clase vacía o contenedor de datos puros sin comportamiento, ya que esto degrada la cohesión y el encapsulamiento ****.
- **Modelado y Diagrama de Clases**: Se prohíbe el uso de clases "anémicas" ****. La clase `Habilidad` debe contener métodos de negocio propios para realizar comparaciones semánticas, tales como `coincideCon(otraHabilidad)` o `cumpleRequisitos()`, manteniendo sus atributos protegidos y encapsulados ****.

### 6. Validación Inmediata en la Creación (Principio Fail Fast)

- **Requerimiento**: El sistema debe impedir la creación de objetos inconsistentes (por ejemplo, un `Proyecto` sin habilidades requeridas o un `Colectivo` sin tipo de organización válido) ****.
- **Modelado y Diagrama de Clases**: Se aplica el principio de **Falla Rápida (Fail Fast)** ****. Todas las validaciones de consistencia de datos deben ejecutarse inmediatamente en el **constructor** de la clase de dominio (`Proyecto`, `Colectivo`, `Habilidad`), lanzando excepciones específicas que hereden de `RuntimeException` ante cualquier estado inválido ****.

### 7. Abstracción del Concepto de Ubicación (Evitar Primitive Obsession)

- **Requerimiento**: La ubicación de los colectivos se expresa inicialmente como texto libre, pero debe estar modelada de manera tal que en un futuro permita validaciones geográficas complejas sin alterar la clase `Colectivo` ****.
- **Modelado y Diagrama de Clases**: Se debe evitar la obsesión por tipos primitivos (`String` para la dirección) ****. Se modela el concepto como una clase propia `Ubicacion` (un objeto de dominio con estructura) para elevar el nivel de abstracción del sistema y facilitar la incorporación de coordenadas geográficas en iteraciones posteriores ****.

### 8. Integración de Ubicaciones con Servicios Externos (Patrón Adapter)

- **Requerimiento**: El sistema debe validar y normalizar la ubicación de los colectivos conectándose con una API externa (por ejemplo, mapas de terceros). Sin embargo, nuestro dominio no debe acoplarse a los tipos de datos ni al SDK del proveedor externo ****.
- **Modelado y Diagrama de Clases**: Se aplica el patrón **Adapter** ****. Se define en el dominio la interfaz saliente `ProveedorDeMapas` y se implementa una clase adaptadora concreta (`AdapterGoogleMaps`) que traduzca nuestras llamadas de dominio a la API del tercero, protegiendo la zona de confianza de nuestra aplicación ****.

### 9. Uso de Atributos Calculados para Evitar Inconsistencias

- **Requerimiento**: Se debe poder consultar el historial o total de colaboraciones en las que participa o ha participado un colaborador para construir su perfil profesional ****.
- **Modelado y Diagrama de Clases**: La cantidad de colaboraciones o de horas aportadas no debe persistirse como un atributo mutable en la clase `Colaboradora` ****. El método `obtenerCantidadColaboraciones()` debe computar este valor en tiempo de ejecución filtrando dinámicamente su colección interna de colaboraciones para evitar inconsistencias de estado ****.

### 10. Reificación de la Relación Intermedia (Clase Relación)

- **Requerimiento**: Las personas colaboradoras que participen o hayan participado de los proyectos deben registrar su historial, conteniendo datos específicos como fecha de alta, estado de la postulación y fecha de finalización ****.
- **Modelado y Diagrama de Clases**: La relación muchos-a-muchos entre `Colaboradora` y `Proyecto` no debe modelarse como una asociación directa de colecciones ****. Se reifica la relación intermedia mediante la clase `Colaboracion`, permitiendo almacenar estos atributos históricos de la postulación de forma limpia y mantenible ****.

### 11. Disparo de Acciones ante Nuevas Colaboraciones (Patrón Observer)

- **Requerimiento**: Cuando una colaboradora se "anota" en un proyecto, el sistema debe disparar dinámicamente múltiples acciones (ej: enviar un correo automático de contacto al colectivo, alertar a los administradores o auditar la postulación) ****.
- **Modelado y Diagrama de Clases**: Se aplica el patrón **Observer** ****. El `Proyecto` actúa como Sujeto (Observable) y notifica a una colección polimórfica de observadores que implementen la interfaz `InteresadoEnColaboracion` mediante un método asincrónico con retorno de tipo `void` ****.

### 12. Configuración de Alertas por Colectivo (Suscripción Dinámica)

- **Requerimiento**: Cada colectivo debe poder configurar individualmente en tiempo de ejecución cuáles de las acciones automáticas ante postulaciones desea activar o desactivar ****.
- **Modelado y Diagrama de Clases**: La colección de observadores no debe registrarse globalmente de forma estática en la aplicación ****. Cada instancia de `Colectivo` (o `Proyecto`) mantendrá su propia lista de interesados (`InteresadoEnColaboracion`), exponiendo los métodos dinámicos de suscripción `registrarInteresado(observador)` y `removerInteresado(observador)` ****.

### 13. Algoritmo de Búsqueda Desacoplado (Inyección de Dependencias)

- **Requerimiento**: El sistema debe permitir cambiar el algoritmo o criterios con los que las colaboradoras buscan y filtran proyectos (ej: priorizar por cercanía, priorizar coincidencia de habilidades o proyectos con incentivo económico) sin modificar el dominio central ****.
- **Modelado y Diagrama de Clases**: Se implementa el patrón **Strategy** para la búsqueda ****. El componente buscador no instancia directamente una estrategia fija con un `new` ni accede mediante un singleton inmutable, sino que recibe la interfaz `AlgoritmoFiltro` mediante su constructor o a través de **Inyección de Dependencias** para flexibilizar la testing unitario ****.

### 14. Aislamiento e Impostores para Pruebas Unitarias (Mocking / Stubs)

- **Requerimiento**: El proceso de testeo unitario que verifica si una colaboradora puede anotarse a un proyecto debe ejecutarse de forma veloz e independiente, sin realizar envíos de correos reales ni consumir créditos del servicio externo de geolocalización ****.
- **Modelado y Diagrama de Clases**: En el entorno de pruebas, se deben inyectar objetos impostores (**Mocks** o **Stubs**) que simulen el comportamiento esperado de las interfaces salientes de infraestructura (ej: `EnviadorDeMails`, `ProveedorDeMapas`), garantizando pruebas unitarias rápidas y determinísticas ****.

### 15. Tareas Programadas mediante Planificación Externa

- **Requerimiento**: Todas las mañanas el sistema debe buscar automáticamente a colaboradoras sugeridas para proyectos activos y enviarles recomendaciones automáticas ****.
- **Modelado y Diagrama de Clases**: El diseño del recomendador periódico no debe implementar hilos concurrentes internos o planificadores bloqueantes en el servidor de la aplicación (Planificación Interna) ****. Se diseña un punto de entrada atómico (clase ejecutable con un método `main` como `MainRecomendador` que invoque llamadas `void` sin argumentos) y se delega la periodicidad de su ejecución a un planificador del sistema operativo externo (como un archivo de _Crontab_) ****.

---

🏁 **Sugerencia de siguiente paso**: ¿Te gustaría que escribamos el **pseudocódigo en Java** de las dos clases principales del dominio (`Proyecto` y `Colaboracion`), incluyendo el constructor con validaciones (_Fail Fast_) y el método de notificación para el patrón _Observer_?