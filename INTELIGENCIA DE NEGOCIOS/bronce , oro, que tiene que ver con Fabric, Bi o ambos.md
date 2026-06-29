
Los términos **bronce y oro** (junto con "plata" o _silver_) se refieren a la **Arquitectura Medallion** (Medallion Architecture), un patrón de diseño moderno que está muy de moda en la ingeniería de datos y Business Intelligence (BI) para organizar el procesamiento y almacenamiento de los datos en distintas capas, dependiendo de su nivel de calidad e importancia.

Específicamente, estas capas funcionan de la siguiente manera:

- **Capa de Bronce (Bronze):** Es la capa donde se guarda la **información cruda**, exactamente tal cual como viene de los sistemas de origen (bases de datos, archivos, etc.). Es el equivalente moderno a lo que en la arquitectura tradicional de BI se conocía como el área temporal de _Staging_.
- **Capa de Plata (Silver):** Es el "corazón" o la fábrica de datos de todo el proceso. Aquí se toman los datos crudos de la capa bronce y se les aplican todas las transformaciones, limpieza, conversiones y reglas de negocio para dejarlos estructurados.
- **Capa de Oro (Gold):** Es la capa orientada al **negocio con los datos curados**. Contiene el modelo final (generalmente en esquema de estrella) con los cálculos específicos, métricas y seguridad, listo para que los usuarios finales lo consuman a través de reportes. Muchas veces no almacena datos de forma persistente, sino que funciona como una vista hacia la capa Silver.

**¿Qué relación tiene con Fabric y Power BI?**

Tiene relación con ambos, ya que trabajan en conjunto dentro del mismo ecosistema de Microsoft:

- **Con Fabric:** Fabric es una arquitectura integral en la nube de Microsoft que agrupa herramientas para el almacenamiento y la transformación de datos. Fabric está diseñado para soportar estructuras modernas como el _Lakehouse_, lo que permite implementar de forma nativa esta arquitectura Medallion.
- **Con Power BI:** Power BI es el componente de visualización y explotación de datos que se encuentra dentro de la plataforma de Fabric. La relación directa es que, dentro de este entorno moderno, **la capa de Oro (Gold) está siendo reemplazada directamente por el Modelo Semántico de Power BI**. Esto significa que el modelo semántico de Power BI se conecta de forma directa a la capa de Plata (Silver) para consumir los datos limpios, evitando la necesidad de crear una base de datos adicional exclusiva para la capa Gold.

