Basado en la estructura y los patrones recurrentes de los 50 exámenes finales de Diseño de Sistemas (DDS) analizados, he diseñado un enunciado complejo y detallado para el sistema **"Patrimonio360"**.

Este enunciado incorpora los desafíos típicos de la materia: latencia en servicios externos, lógica de estados, integración con APIs de terceros, algoritmos de recomendación y soporte multicanal.

---

### **Patrimonio360: Plataforma de Identidad y Memoria Urbana**

**Contexto** El municipio requiere una solución tecnológica para centralizar la identificación, registro y seguimiento de elementos que constituyen el patrimonio tangible no convencional (ej: tapas de luz históricas, relojes en altura, grafitis con valor cultural). El sistema debe servir tanto para la gestión técnica del mantenimiento de estos bienes como para la promoción turística y el fortalecimiento de la identidad local.

#### **Servicio I - Gestión del Inventario Patrimonial**

Es el servicio central de datos de los "Elementos Identitarios". De cada elemento se debe registrar: nombre, descripción histórica, año de origen (o estimación), materialidad y coordenadas geográficas (latitud/longitud). Los elementos se agrupan en **Categorías** (Mobiliario, Arte Urbano, Ingeniería de Época) y **Subcategorías**. Cada elemento posee un estado de conservación (Excelente, Bueno, Deteriorado, En Restauración) y una valoración de "Relevancia Histórica" (0 a 100). El servicio debe permitir la gestión completa (ABM) de estos elementos y sus metadatos.

#### **Servicio II - Validación de Estado por IA (Monitoreo)**

Para asegurar el seguimiento, el sistema permite que inspectores o ciudadanos envíen fotos actuales desde la calle.

- **Integración con IA:** El sistema envía la imagen a un motor externo de **Inteligencia Artificial** que compara la foto con la imagen original de referencia. Este proceso es costoso y presenta una latencia elevada (puede demorar más de un minuto en responder).
- **Gestión de Alertas:** Si la IA detecta una "Probabilidad de Daño" superior al 40%, se debe generar automáticamente una **Alerta de Mantenimiento** que quedará en estado "Pendiente de Revisión" para un restaurador municipal.

#### **Servicio III - Generador de "Rutas de Identidad"**

El sistema crea rutas turísticas optimizadas para los usuarios. Se proponen dos modalidades de generación:

- **Modalidad A (Manual):** El usuario selecciona hitos específicos y el sistema rutea el camino más corto.
- **Modalidad B (Algorítmica):** El usuario indica un interés (ej: "Historia Ferroviaria") y un tiempo disponible (ej: 2 horas). Un algoritmo de **Matchmaking** selecciona los elementos más relevantes y cercanos para armar la ruta. Para la optimización, se utiliza una API de Mapas externa que cobra por cada solicitud realizada.

#### **Servicio IV - Participación Ciudadana y "Relatos"**

Los usuarios pueden "Validar" su visita estando físicamente en el lugar mediante geofencing o escaneo de un código QR impreso en el elemento. Al validar, el usuario puede subir un "Relato de Identidad" (texto o audio). Los usuarios ganan puntos por cada visita validada, lo que les permite subir de nivel en la comunidad (Bronce, Plata, Oro), desbloqueando acceso a fotos históricas inéditas de los elementos.

---

### **Alcance y Requerimientos (Similares a los finales DDS)**

El sistema deberá permitir:

1. **Gestión de Inventario:** Alta, baja y modificación de elementos patrimoniales y sus categorías.
2. **Trazabilidad de Estados:** Mantener una bitácora completa de la evolución de cada elemento (ej: cambios de estado de conservación tras inspecciones).
3. **Registro de Usuarios y Validación:** Los ciudadanos se registran vía **SSO** y sus datos se validan contra el **RENAPER**.
4. **Procesamiento Asincrónico de IA:** Gestionar el envío de fotos al motor de IA sin bloquear la experiencia del usuario, manejando el tiempo de respuesta elevado.
5. **Generación de Rutas:** Implementar las dos modalidades (A y B) utilizando patrones de diseño que permitan sumar nuevos algoritmos a futuro.
6. **Notificaciones Multicanal:** Enviar alertas a restauradores (ante daños) y notificaciones a usuarios (rutas listas o puntos ganados) por Email o WhatsApp.
7. **Soporte Multicanal (Web y Mobile):** Diseño de una arquitectura de frontend que permita reutilizar la lógica de negocio para la plataforma administrativa (Web) y la experiencia en calle (Mobile).
8. **Moderación de Relatos:** Un panel para que los administradores aprueben o rechacen los comentarios y audios de los ciudadanos antes de su publicación.
9. **Optimización de Costos:** Mecanismos para minimizar las llamadas a la API de Mapas y a la IA, dada la naturaleza arancelada de estos servicios.
10. **Seguridad de Datos:** Garantizar la confidencialidad de la ubicación de los inspectores y datos personales de los usuarios.

### **Consideraciones para el Examen (Consignas típicas)**

- **Arquitectura:** Realizar diagrama de componentes y despliegue. Explicar cómo evitar el **Timeout** en el Servicio II y qué estrategia de integración (Push/Pull) es mejor para los sensores/fotos.
- **Dominio:** Diseñar el diagrama de clases para los Servicios III y IV. Justificar el uso de patrones para el manejo de los estados de los elementos y los distintos algoritmos de rutas.
- **Persistencia:** Diseñar el **DER físico**. Justificar qué elementos persistir (ej: coordenadas e historial) y cuáles no (ej: datos calculados de IA), y cómo resolver los **Impedance Mismatches**.