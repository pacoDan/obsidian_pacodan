navenegocios.com.ar
https://navenegocios.ar/home/comisiones---nave


## 1. El enfoque corporativo: API de Open Galicia (Polling)

Si tienes una cuenta de empresa (Pyme o Corporativa) y acceso a [Office Banking](https://ayudaempresas.galicia.ar/AyudajuridicaSPA/ini/n4/cuales-son-los-requisitos-para-acceder-al-servicio-de-open-galicia-para-empresas), el banco provee la API Movimientos de Cuenta en su plataforma [Open Galicia](https://www.galicia.ar/content/dam/galicia/banco-galicia/empresas/open-galicia/catalogoopengalicia.pdf). [1, 2]

- Mecanismo: No es un webhook. Funciona mediante Polling (peticiones REST recurrentes).
- Cómo implementarlo: Debes programar un _worker_ o una función Serverless (ej. AWS Lambda con un disparador Cron) que consulte el endpoint de movimientos cada 1 o 2 minutos. El JSON devuelto expone los créditos en tiempo real. Al detectar un ID de movimiento nuevo, disparas internamente tu evento de notificación. [2]

## 2. El enfoque Open Banking: Agregadores de APIs de terceros

Si necesitas conectar una cuenta de individuo o buscas una integración REST unificada, la infraestructura financiera local cuenta con agregadores de Open Banking que conectan con Galicia (como [Prometeo Open Banking](https://prometeoapi.com/) o similares).

- Mecanismo: Estas plataformas se encargan de conectarse a la banca en línea de forma segura y exponen una API limpia para ti.
- Ventaja: Al centralizar la conexión, puedes usar el mismo código si en el futuro necesitas añadir otros bancos argentinos (como BBVA o Santander).

## 3. El enfoque "Hacker" (Cuentas Personales): Parseo de Emails

Para cuentas de personas físicas que no califican para las APIs corporativas, la solución estándar en la comunidad de desarrolladores es el Email Parsing.

- Mecanismo: Configuras en el Home Banking el envío de [Notificaciones por Email](https://www.galicia.ar/personas/app-galicia/notificaciones-tiempo-real) ante transferencias entrantes. [3]
- Arquitectura:
    
    1. Rediriges esos correos automáticos del banco a un servicio como Mailgun, SendGrid (Inbound Parse) o un script en un servidor propio con protocolo IMAP.
    2. Estos servicios transforman el correo electrónico entrante en un `POST` Webhook (JSON) dirigido a tu servidor backend.
    3. Creas un script que parsee el cuerpo del mail con expresiones regulares (Regex) para extraer el monto, la fecha y el CBU/Alias de origen.
    

## 4. Alternativa de Arquitectura: Redirección mediante alias virtual (CVU)

Si tu desarrollo necesita obligatoriamente Webhooks nativos e instantáneos, una práctica común es no dar el CBU de Galicia directamente a tus clientes.

- Estrategia: Creas una cuenta puente en una Fintech (como Mercado Pago, Pomelo u Openpay) que sí ofrezca infraestructura de [Webhooks para transacciones](https://documents.openpay.mx/docs/webhooks.html). Cuando impacta la notificación del webhook en tu sistema, ejecutas una transferencia automática (mediante API de la fintech) hacia tu CBU del Banco Galicia para resguardar los fondos en la banca tradicional. [4]

¿Qué entorno estás utilizando para tu backend (ej. Node.js, Python, Go)? Te puedo ayudar a armar un ejemplo de estructura de datos o un script para procesar cualquiera de estas alternativas.

  

[1] [https://ayudaempresas.galicia.ar](https://ayudaempresas.galicia.ar/AyudajuridicaSPA/ini/n4/cuales-son-los-requisitos-para-acceder-al-servicio-de-open-galicia-para-empresas)

[2] [https://www.galicia.ar](https://www.galicia.ar/content/dam/galicia/banco-galicia/empresas/open-galicia/catalogoopengalicia.pdf)

[3] [https://www.galicia.ar](https://www.galicia.ar/personas/app-galicia/notificaciones-tiempo-real)

[4] [https://documents.openpay.mx](https://documents.openpay.mx/docs/webhooks.html)
