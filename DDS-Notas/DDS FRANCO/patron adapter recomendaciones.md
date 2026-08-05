El patrón **Adapter** se utiliza principalmente cuando nuestro sistema necesita integrarse con un **componente, servicio o API externa** de un tercero. Su propósito fundamental es **desacoplar el core de nuestro dominio de los cambios, estructuras de datos y tecnicismos** de ese proveedor externo.

Nos permite elevar el nivel de abstracción de nuestro sistema, convirtiendo una interfaz de bajo nivel (como llamadas crudas HTTP, tipos de datos primitivos o un SDK ajeno) en una interfaz de alto nivel que habla el lenguaje ubicuo de nuestro negocio. Además, es crucial para la **testeabilidad**, ya que nos permite reemplazar el adaptador real por un objeto impostor (_Mock_ o _Stub_) en las pruebas unitarias.

---

### ¿Cómo es siempre la secuencia del Adapter?

La interacción en un diagrama de secuencia de este patrón sigue un flujo de **traducción lineal** en cinco pasos:

1. **Invocación de alto nivel**: El objeto de dominio (Cliente) envía un mensaje expresivo a la interfaz del adaptador (ej: `ServicioMeteorologico`) pidiendo un dato de negocio.
2. **Recepción del Adapter**: El adaptador concreto recibe el mensaje polimórfico.
3. **Traducción y delegación**: El adaptador traduce los parámetros de nuestro dominio a los formatos requeridos por el tercero y realiza la llamada real al SDK o cliente HTTP externo (el _Adaptee_).
4. **Respuesta cruda**: El sistema externo procesa el pedido y retorna su estructura nativa (que suele ser genérica, cruda o un mapa de datos complejos).
5. **Mapeo y retorno limpio**: El adaptador toma esa respuesta del tercero, la procesa (ej: extrae el valor del JSON, convierte unidades de medida o maneja excepciones) y devuelve un valor limpio y tipado para nuestro dominio.

---

### Ejemplos de Diagramas de Secuencia Recomendados

#### Ejemplo 1: "Qué Me Pongo" (Servicio Meteorológico con la API de clima)

En este ejercicio, el dominio necesita la temperatura para generar sugerencias. En lugar de acoplar la combinatoria de ropa al SDK de clima, se introduce el adaptador:

```
 [Guardarropas]       [ServicioMeteorologico]      [AdapterAccuWeather]       [AccuWeatherAPI]
       |                        |                           |                        |
       |-- obtenerClima(ciudad) |                           |                        |
       |   (polimórfico) ------>|                           |                        |
       |                        |--- obtenerClima(ciudad) ->|                        |
       |                        |    (concreto)             |                        |
       |                        |                           |-- getWeather(string) ->|
       |                        |                           |   (llamada al SDK)     |
       |                        |                           |<-- retorna List<Map> --|
       |                        |                           |    (datos crudos)      |
       |                        |                           |                        |
       |                        |                           |-- [Parsea el Map]
       |                        |                           |-- [Convierte unidades]
       |                        |                           |                        |
       |                        |<-- retorna EstadoClima ---|                        |
       |                        |    (objeto limpio) |                        |
       |<-- retorna EstadoClima |                           |                        |
```

_(Nota: El `Guardarropas` le habla a la interfaz polimórfica, la cual es implementada por el adaptador concreto `AdapterAccuWeather`, traduciendo los datos de la librería externa de bajo nivel)._

#### Ejemplo 2: "Noodle" (Envío de notificaciones por Email)

El modelo de dominio avisa sobre cambios en los grupos. Se evita que las clases de negocio interactúen directamente con la librería de correos de bajo nivel:

```
  [Grupo]                  [Notificador]             [AdapterMailSender]       [MailSenderSDK]
     |                           |                            |                       |
     |-- notificarAlta() ------->|                            |                       |
     |                           |-- enviarMail(user, body) ->|                       |
     |                           |   (polimórfico)            |                       |
     |                           |                            |-- send(emails, title, |
     |                           |                            |   body) ------------->|
     |                           |                            |<-- (success/void) ----|
     |                           |<-- (void) -----------------|                       |
     |<-- (void) ----------------|                            |                       |
```

---

### ¿Qué pasa con las clases DTO y Service? ¿Nunca van?

#### Las clases DTO (Data Transfer Objects)

- **Sí aparecen, pero de forma pasiva**. Los DTOs son contenedores planos de datos que sirven para transferir información de un sistema a otro y carecen de comportamiento de negocio.
- En un diagrama de secuencia, **no tienen su propia línea de vida activa** (un "cuadradito" que inicie o reciba mensajes de negocio para ejecutar lógica), ya que no tienen comportamiento.
- Se representan **viajando dentro de las flechas**, ya sea como **parámetros** de los mensajes o como el **objeto de retorno** de una llamada (dentro de la línea de retorno punteada).

#### Las clases "Service"

- **No deben existir en tu modelo de dominio**. En Diseño de Sistemas (DDS), **se desaconseja sistemáticamente el uso de clases "Service"**. La palabra "servicio" es sumamente ambigua y en el 99% de los casos su uso deriva en modelos de dominio anémicos y controladores puramente procedimentales. Diseñamos orientados a responsabilidades y cohesión, no a capas procedimentales de tipo Spring (`UserService`, `ProjectService` etc.).
- La única excepción permitida son los conceptos del mundo real consolidados por el lenguaje ubicuo, como un `ServicioMeteorologico`.
- En consecuencia, las clases "Service" procedimentales **nunca van en el diagrama de secuencia de dominio** porque son un _code smell_. El único "Service" que puede aparecer en el extremo derecho del diagrama es el **sistema externo o API de un tercero** (_Adaptee_), que representa la infraestructura física final con la que interactuamos.

---

📊 ¿Te gustaría que apliquemos esta misma secuencia de Adapter para modelar la integración con la API de geolocalización que propusimos para **Código a Voluntad**?