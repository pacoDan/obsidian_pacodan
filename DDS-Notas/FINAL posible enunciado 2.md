Basado en la estructura y el estilo de los exámenes finales de Diseño de Sistemas (DDS) presentes en las fuentes, he diseñado un enunciado extendido y complejo para el sistema **"IdentidadLocal 360"**.

Este enunciado incorpora los desafíos más frecuentes: latencia de servicios externos, algoritmos de selección, trazabilidad de estados, integración con servicios de terceros y multiplicidad de canales.

---

### **IdentidadLocal 360: Plataforma de Resguardo y Recorrido Patrimonial**

**Contexto** El municipio ha decidido modernizar la gestión de su patrimonio tangible no convencional (relojes antiguos, fachadas con esgrafiados, tapas de servicios de principios de siglo, grafitis históricos, etc.). El sistema debe ser una herramienta tanto para la conservación técnica por parte de especialistas como para la promoción turística y cultural.

#### **Servicio I - Gestión del Catálogo de "Hitos Históricos"**

Este servicio es el corazón de datos del sistema. Cada "Hito" cuenta con nombre, descripción, fecha de origen (o estimación), materialidad dominante y coordenadas geográficas (latitud/longitud). Los hitos se organizan en **Categorías** (Mobiliario Urbano, Arte de Fachada, Ingeniería Civil Antigua). Un administrador puede dar de alta nuevos hitos, editarlos o darlos de baja. El sistema debe permitir asociar a cada hito un **Grado de Fragilidad** (de 1 a 10) que condiciona la frecuencia de las inspecciones técnicas.

#### **Servicio II - Monitoreo y Validación de Estado (IA)**

Para garantizar el seguimiento, los ciudadanos pueden reportar el estado de un hito subiendo una foto actual desde la calle.

- **Integración con IA:** El sistema envía la imagen a un motor de **Inteligencia Artificial** que compara la foto con el registro histórico. Este proceso de comparación y detección de grietas o vandalismo es complejo y puede demorar **más de 90 segundos** en retornar un diagnóstico.
- **Flujo de Revisión:** Si la IA detecta un deterioro superior al 30%, la alerta queda en estado "Pendiente de Revisión" para que un restaurador municipal valide el daño. Se requiere una bitácora completa de los estados del hito (Activo, Dañado, En Restauración, Restaurado) indicando fecha y responsable.

#### **Servicio III - Generador de "Rutas de Identidad"**

El sistema ofrece rutas personalizadas para recorrer los hitos. La generación de la ruta utiliza dos modelos:

- **Modelo A (Por Interés):** El usuario elige una categoría y el sistema selecciona los 5 hitos mejor rankeados por la comunidad.
- **Modelo B (Matchmaking Inteligente):** El usuario indica cuánto tiempo tiene (ej: 45 min) y su ubicación actual. Un algoritmo selecciona los hitos basándose en la cercanía y la "Relevancia del Día" (calculada según clima y eventos locales). Para optimizar el camino, se consulta a una **API de Mapas Externa** que cobra por cada solicitud de ruteo.

#### **Servicio IV - Experiencia del Ciudadano y Gamificación**

Para fomentar el uso, los habitantes pueden "validar" su visita estando físicamente en el lugar (vía GPS o escaneo de QR).

- **Relatos y Multimedia:** Al validar la visita, el usuario puede subir un "Relato de Identidad" (texto o audio) contando una anécdota familiar asociada al lugar. Estos relatos deben ser moderados para evitar contenido ofensivo antes de publicarse.
- **Niveles de Usuario:** Los usuarios ganan puntos por visitas y relatos. Existen categorías: "Visitante Casual", "Protector de la Historia" e "Historiador Local". Cada categoría desbloquea contenido exclusivo (fotos antiguas inéditas del hito).

---

### **Alcance y Requerimientos (Muchos)**

El sistema deberá permitir:

1. **Gestión de Hitos:** Registro completo de los elementos patrimoniales, incluyendo su geolocalización y fotos base.
2. **Gestión de Categorías y Tags:** Administrar los tipos de hitos y etiquetas (ej: #ArtDeco, #Ferroviario) para facilitar búsquedas.
3. **Seguimiento de Trazabilidad:** Mantener un registro histórico de todos los cambios de estado del hito y las intervenciones realizadas.
4. **Validación de Identidad de Usuarios:** Integración con **SSO (RENAPER u otros)** para asegurar que quienes suben relatos son personas reales.
5. **Análisis de Deterioro Asincrónico:** Gestionar el envío de imágenes a la IA y la recepción diferida del diagnóstico sin bloquear la interfaz del usuario.
6. **Optimización de Costos de API:** Implementar mecanismos para minimizar las llamadas a la API de Mapas y a la IA Generativa (Caching de rutas frecuentes).
7. **Generación de Rutas Multimodales:** Permitir la creación de recorridos manuales o sugeridos por algoritmos de matchmaking.
8. **Gestión de Preguntas Dinámicas:** Permitir que los administradores configuren "Trivias" por hito para que los usuarios contesten al visitar.
9. **Administración de Gamificación:** Configurar los umbrales de puntos y las insignias para los distintos niveles de usuario.
10. **Notificaciones Inteligentes:** Enviar alertas (Push o Email) a los restauradores ante daños críticos y a los usuarios sobre nuevos hitos cerca de su casa.
11. **Moderación de Relatos:** Panel para que operadores aprueben o rechacen los comentarios de los ciudadanos.
12. **Visualización Multicanal:** Interfaz Web para administración y consultas pesadas; y App Mobile para la experiencia en calle.
13. **Reportes Estadísticos:** Visualizar el hito más visitado, la zona con más deterioros y el ranking de "Historiadores Locales".
14. **Persistencia Multimedia:** Gestión eficiente de fotos y audios de alta resolución (almacenamiento y disponibilización).
15. **Seguridad y Confidencialidad:** Garantizar que los datos de los usuarios y las coordenadas de inspección técnica estén protegidos.

### **Consignas de Examen Típicas para este Contexto**

1. **Arquitectura:** Realizar el diagrama de **Despliegue y Componentes**. Explicar cómo manejaría la latencia de 90 segundos del Servicio II y si optaría por un cliente pesado o liviano para la App Mobile.
2. **Dominio:** Diseñar el diagrama de clases para el **Servicio III (Rutas)**. Justificar el uso de patrones para manejar los distintos algoritmos de matchmaking y las diferentes categorías de usuario.
3. **Persistencia:** Diseñar el **DER Físico** para el Servicio I y IV. Justificar qué elementos no persistiría (ej: cálculos de puntos en tiempo real) y si usaría una base NoSQL para los relatos masivos de los ciudadanos.****