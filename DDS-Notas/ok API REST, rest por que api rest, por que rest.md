Una **API** (Application Programming Interface) es una interfaz documentada que permite la comunicación y colaboración entre dos sistemas de software. A diferencia de una página web tradicional diseñada para humanos (HTML), una API entrega estructuras de datos como **JSON o XML** que están pensadas para ser procesadas automáticamente por un programa.

**REST** es un estilo arquitectónico o protocolo de comunicación de alto nivel que se construye sobre el protocolo **HTTP**. Una **API REST** es aquella que emplea las convenciones de REST y el protocolo HTTP para realizar la integración entre los servicios de un sistema.

Lo que define y hace que una API sea **REST** son los siguientes lineamientos:

- **Uso del protocolo HTTP:** Utiliza este protocolo estándar para conectar diferentes nodos a través de la red, permitiendo la comunicación con el mundo de Internet.
- **Basado en Endpoints:** Expone puntos de entrada específicos (URLs) para consultar disponibilidad o realizar operaciones sobre los recursos del dominio.
- **Contrato agnóstico a la interfaz:** El diseño de la API debe ser totalmente independiente de los canales de visualización, lo que significa que el contrato de datos no cambia si el cliente es una página web o una aplicación móvil.
- **Estructura de Pedido-Respuesta:** Sigue un modelo donde un cliente envía una petición al servidor y este le devuelve una respuesta con el resultado del procesamiento.
- **Transferencia de estado mediante DTOs:** Utiliza objetos de transferencia de datos (**DTO**) para desacoplar el formato de la información que viaja por la red de las entidades internas del modelo de dominio.
- **Formato de respuesta estándar:** Típicamente retorna datos en formato **JSON**, lo que permite que el cliente reciba la información necesaria y realice su propio renderizado de las vistas.

En la actualidad, las API REST son el mecanismo preferido para la integración con **sistemas externos** y la comunicación dentro de arquitecturas de **microservicios**, garantizando que componentes independientes puedan colaborar de forma sincrónica o asincrónica.