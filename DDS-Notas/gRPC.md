**gRPC** es un framework de llamadas a procedimientos remotos (_Remote Procedure Call_) moderno, de código abierto y de alto rendimiento. Fue desarrollado originalmente por **Google en el año 2015**.

Pertenece a la categoría de **APIs a nivel de programa**, lo que significa que proporciona la capacidad de ejecutar código de forma remota directamente entre componentes distribuidos.

---

### ¿Para qué sirve?

- **Comunicación de alto rendimiento en microservicios:** Está centrado principalmente en la comunicación eficiente entre servicios y microservicios dentro de centros de datos, optimizando los recursos de red.
- **Integración Multilenguaje:** Al ser compatible con múltiples lenguajes de programación, permite que sistemas escritos en stacks tecnológicos totalmente diferentes se comuniquen de forma transparente.
- **Soporte nativo de Streaming:** Está especialmente diseñado para gestionar transmisiones de datos en tiempo real mediante conexiones persistentes. Soporta cuatro modalidades de flujo:
    1. **Unario:** El cliente envía una única solicitud y recibe una única respuesta.
    2. **Streaming del lado del servidor:** El cliente hace una petición y el servidor le transmite de a poco los datos en un flujo continuo.
    3. **Streaming del lado del cliente:** El cliente envía un flujo continuo de datos y el servidor responde una sola vez al finalizar.
    4. **Streaming bidireccional:** Ambos extremos envían flujos de datos de forma simultánea.
- **Conexión de dispositivos móviles y navegadores:** Permite realizar integraciones directas desde aplicaciones móviles (como Android) hacia servicios de backend, haciendo que la comunicación sea mucho más rápida y fluida que con APIs REST tradicionales.

---

### ¿Cómo funciona técnicamente?

A diferencia de REST (que usa JSON) o SOAP (que usa XML), gRPC se apoya sobre dos pilares fundamentales:

1. **Protocolo HTTP/2:** Utiliza esta versión del protocolo HTTP como transporte, lo que habilita la bidireccionalidad nativa (el servidor puede llamar al cliente de forma espontánea).
2. **Protocol Buffers (Protobuf):** Son archivos de texto plano con extensión `.proto` que definen de forma estricta y legible los contratos y esquemas de los mensajes. Al transmitirse por la red, estos datos se **serializan en un formato binario compacto**, lo que reduce drásticamente el tamaño de los paquetes y los hace ilegibles al ojo humano, maximizando la velocidad.

---

### Todos los ejemplos mencionados en las fuentes

1. **Integración de Servidor C++ con clientes Ruby y Android/Java:** Las fuentes describen un escenario donde un servidor central escrito en **C++** expone la interfaz de gRPC, y clientes desarrollados en **Ruby** o en **Android (Java)** consumen sus métodos directamente a través de peticiones (`proto request`) y respuestas (`proto response`) binarias.
2. **Librerías internas de GraphQL (Apollo):** Se detalla que los frameworks modernos de consulta de datos como **Apollo** (una biblioteca utilizada para GraphQL) utilizan gRPC por debajo para lograr que sus integraciones internas de backend sean sumamente eficientes.
3. **Transmisión de contenido (Streaming estilo Netflix):** Se ejemplifica el streaming de servidor utilizando el caso de uso de una plataforma de video como **Netflix**; en lugar de que el cliente realice miles de peticiones individuales por cada fragmento, se abre una única conexión gRPC y el servidor le va sirviendo secuencialmente los paquetes del archivo a medida que los necesita.
4. **Ejemplo de esquema de datos `.proto` (Protocol Buffers):** Las fuentes exponen la estructura de un archivo de definición de contrato para una entidad `Person` utilizando la sintaxis `proto3`:
    
    ```
    syntax = "proto3";
    package tutorial;
    
    message Person {
      string name = 1;
      int32 id = 2;
      string email = 3;
    
      enum PhoneType {
        MOBILE = 0;
        HOME = 1;
        WORK = 2;
      }
    
      message PhoneNumber {
        string number = 1;
        PhoneType type = 2;
      }
    
      repeated PhoneNumber phones = 4;
    }
    ```
    
    Este contrato define campos obligatorios (`name`, `id`), opcionales (`email`), un enumerado (`PhoneType`) y campos repetidos (`repeated PhoneNumber`) que permiten estructurar listas de teléfonos de forma compacta y portable.

📊 Si te interesa, podemos armar un cuadro comparativo detallado entre REST, SOAP, gRPC y GraphQL para analizar a fondo cuándo conviene elegir cada uno según las restricciones de performance de tu sistema.