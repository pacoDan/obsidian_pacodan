Basado en los principios de diseño de sistemas, las correcciones de ejercicios previos (como _Que Me Pongo_ y _Mcowins_) y los materiales sobre arquitectura proporcionados, aquí tienes el **Top 20 de requerimientos y temas de diseño** fundamentales para la plataforma **Código a Voluntad**.

### I. Requerimientos de Dominio y Lógica (Modelo Rico)

1. **Validación de Consistencia (Fail Fast):** El sistema debe impedir la creación de objetos en estados inválidos (ej. un `Proyecto` sin título o una `Habilidad` sin código) mediante validaciones en el **constructor** para detectar errores lo antes posible.
2. **Cosificación de Habilidades:** Las `Habilidades` no deben ser simples cadenas de texto (**Primitive Obsession**), sino objetos que garanticen la **uniformidad** (evitar que "Java" y "java" se traten como distintos) y permitan comportamiento futuro.
3. **Polimorfismo en Modalidades de Colaboración:** El cálculo de incentivos económicos o compromisos de horas debe resolverse mediante el patrón **Strategy**, evitando el uso de condicionales (`if/switch`) según el tipo de modalidad.
4. **Cálculo Dinámico de Elegibilidad:** La lógica para saber si una `Colaboradora` puede anotarse en un `Proyecto` debe ser un método del dominio (probablemente en `Proyecto`) que verifique la intersección de habilidades de forma declarativa.
5. **Inmutabilidad de Colaboraciones:** Una vez establecida, una `Colaboracion` debe ser preferentemente **inmutable** para asegurar la transparencia referencial y evitar efectos colaterales indeseados en el historial.
6. **Atributos Calculables vs. Almacenados:** El estado de un proyecto (ej. "Cubierto" si ya tiene suficientes colaboradores) debe ser un **dato calculado** a partir de sus colaboraciones actuales, evitando la redundancia de datos inconsistentes.
7. **Evitar el "Usuario Dios":** La clase `Colaboradora` no debe centralizar todas las acciones del sistema; las responsabilidades deben distribuirse (ej. el `Proyecto` gestiona sus inscripciones) para mantener una **alta cohesión**.

### II. Requerimientos de Arquitectura e Integración

8. **Adaptación de Localización (Pattern Adapter):** Dado que la ubicación de los `Colectivos` es texto libre, se debe utilizar un **Adapter** para integrarse con una API externa de mapas (ej. Google Maps) y normalizar direcciones sin acoplar el dominio a la interfaz externa.
9. **Interfaz REST Basada en Recursos:** Las rutas deben definirse siguiendo el estilo **REST** (ej. `POST /proyectos/:id/colaboraciones`) en lugar de un estilo RPC (ej. `/anotarseAlProyecto`), tratando las colaboraciones como recursos.
10. **Inyección de Dependencias para Servicios Externos:** Los servicios de envío de correo o mapas deben ser **inyectados** (vía constructor o setters), permitiendo cambiar de proveedor fácilmente según condiciones económicas.
11. **Notificaciones Asincrónicas:** El envío de correos de contacto tras una colaboración debe ser **asincrónico** (posiblemente mediante **colas de mensajes**) para no bloquear la experiencia del usuario si el servidor de correo demora.
12. **Capa de Persistencia Separada:** La lógica de dominio debe ser independiente de cómo se guardan los datos en la base de datos relacional, utilizando un mapeo que respete la estructura de objetos.
13. **Escalabilidad Horizontal:** La arquitectura debe prever el uso de un **balanceador de carga** frente a múltiples nodos de la aplicación en caso de picos de tráfico por campañas sociales.
14. **Uso de DTOs para Integración:** Para transferir datos entre el frontend y el backend, o con sistemas externos, se deben usar **Data Transfer Objects (DTO)** que simplifiquen la estructura de datos en la frontera del sistema.

### III. Calidad, Pruebas y Ética (Requerimientos No Funcionales)

15. **Testeabilidad con Objetos Impostores:** El sistema debe permitir el uso de **Mocks o Stubs** (ej. Mockito) para probar la lógica de inscripción sin realizar llamadas reales a APIs externas o enviar emails reales.
16. **Protección de Datos Personales (Soberanía Digital):** El diseño debe garantizar por construcción que los datos de contacto solo se revelen tras la colaboración, rindiendo cuentas sobre la **privacidad** y el manejo ético de la información.
17. **Algoritmos Transparentes:** Los criterios de búsqueda o sugerencia de proyectos deben evitar **sesgos o algoritmos de opresión** que pudieran invisibilizar a colectivos minoritarios.
18. **Robustez ante Fallos Externos:** Si una API externa de mapas o clima falla, el sistema debe **fallar con gracia**, informando al usuario en lugar de propagar un error técnico de bajo nivel (como `NullPointerException`).
19. **Simplicidad Evolutiva (YAGNI):** No se deben implementar estructuras complejas de "niveles de usuario" o "puntos" si no están en los requerimientos actuales, priorizando siempre la **solución más simple**.
20. **Registro de Auditoría (Accountability):** Se debe guardar rastro de las colaboraciones y cambios importantes para poder explicar el estado del sistema en el pasado, similar a la lógica de "historial" vista en otros ejercicios.