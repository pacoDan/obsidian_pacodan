Una **URL de callback** es una dirección o endpoint que tu sistema expone para permitir que un componente externo le notifique un resultado o un cambio de estado una vez que ha finalizado una tarea.

Esta técnica es una implementación específica del patrón de comunicación conocido como **Continuaciones o Callbacks**, y se caracteriza por los siguientes puntos fundamentales:

- **Asincronismo:** Se utiliza principalmente para gestionar operaciones que no son instantáneas. En lugar de que tu sistema se quede en una "espera activa" (bloqueado esperando la respuesta), realiza el pedido al servicio externo y se desentiende de la tarea para seguir procesando otras cosas.
- **Instrucciones de "qué hacer después":** Al enviar la solicitud, el emisor no solo pide algo, sino que también indica el camino de retorno. Es como decirle a un tercero: "Realiza este trabajo y, cuando termines, avísame llamando a esta dirección".
- **Notificación de resultados:** El servicio externo, al completar su proceso, realiza una llamada a esa URL proporcionada por tu sistema para entregarle los datos resultantes (por ejemplo, el éxito o fracaso de una operación o una lista de rutas generadas).

### Ejemplo en el contexto de tu sistema de Donaciones

Según tus requerimientos de implementación, para la integración con el planificador de rutas externo, tu sistema debe exponer una **URL de callback**. El proceso funcionaría así:

1. Tu sistema envía un lote de hasta 100 donaciones al planificador externo.
2. Como la planificación puede ser lenta o compleja, el sistema no espera la respuesta en la misma conexión.
3. El componente externo procesa los datos y, al terminar, **"llama de vuelta"** a la URL de callback que tú le indicaste.
4. Tu plataforma recibe esa llamada, registra las rutas generadas y actualiza el estado de las entregas automáticamente.

En resumen, la URL de callback es el mecanismo técnico que permite resolver la **falta de sincronismo** entre sistemas, asegurando que tu aplicación sepa cuándo y cómo retomar un flujo de trabajo que fue delegado a un tercero.