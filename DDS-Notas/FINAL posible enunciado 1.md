Basado en la estructura de los enunciados de los exámenes finales de Diseño de Sistemas (DDS) analizados, he elaborado una propuesta integral para el nuevo sistema, al que llamaremos **"PatrimonioVivo"**.

Este enunciado sigue el estilo de los exámenes más recientes (como _La Scalo App_, _ProtegePro_ o _Libros Circulares_), dividiendo la lógica por servicios y planteando desafíos técnicos específicos.

---

### **PatrimonioVivo: Sistema de Identificación y Seguimiento de Identidad Local**

**Contexto** El municipio requiere una plataforma para centralizar la identificación, registro y seguimiento de elementos con valor histórico, cultural o identitario situados en el espacio público (fachadas, calles, plazas). El sistema busca no solo preservar la memoria local a través de un seguimiento técnico, sino también fomentar el turismo y el sentido de pertenencia de los habitantes.

#### **Servicio I - Gestión del Inventario Patrimonial**

Administra el registro detallado de los elementos. De cada "Elemento Identitario" se conoce: nombre, descripción histórica, año estimado de origen, materialidad y una valoración subjetiva de su "Relevancia Histórica" (de 0 a 100). Además, se debe registrar su ubicación exacta mediante coordenadas (latitud y longitud). Los elementos se categorizan (ej: "Arte Urbano", "Mobiliario Antiguo", "Arquitectura de Fachada"). Un elemento puede estar "Activo" para visitas, "En Restauración" o "Removido/Perdido". Este servicio expone un catálogo para que los habitantes puedan navegar los elementos por categoría o relevancia.

#### **Servicio II - Validación Inteligente de Estado**

Para asegurar que los elementos no sufran degradación sin aviso, el sistema permite que inspectores o ciudadanos envíen fotos actuales. El sistema integra un **Agente de IA Externo** que compara la foto nueva con la foto original del registro.

- **Desafío técnico:** El análisis de IA es complejo y puede demorar más de un minuto en retornar un JSON con el "Índice de Deterioro". Si el índice supera el 40%, el sistema debe generar automáticamente una "Alerta de Mantenimiento" para el área de restauración del municipio.

#### **Servicio III - Rutas Turísticas y Recomendaciones**

El sistema ofrece a los usuarios (locales y turistas) la generación de rutas temáticas. Existen dos modalidades de generación:

- **Modalidad A (Manual):** El usuario selecciona específicamente qué elementos desea visitar y el sistema optimiza el camino.
- **Modalidad B (Automática):** El usuario indica un tiempo disponible (ej: 2 horas) y un interés (ej: "Historia Ferroviaria"). El sistema utiliza un algoritmo de matchmaking que selecciona los elementos con mayor puntaje de relevancia y cercanía para armar la ruta. Para la optimización del recorrido, se utiliza una API externa de geolocalización que cobra por cada solicitud realizada.

#### **Servicio IV - Interacción y Comunidad**

Los usuarios pueden registrar su visita escaneando un código QR físico en el lugar o mediante geofencing si están a menos de 10 metros del elemento. Al registrar la visita, pueden dejar un "Relato de Identidad" (comentario) o subir una foto. Los usuarios ganan puntos de "Ciudadano Ilustre" por cada 5 visitas validadas, lo que les permite acceder a contenido histórico exclusivo o insignias en su perfil.

---

### **Alcance y Requerimientos**

El sistema deberá permitir:

1. **Gestión de Elementos:** Alta, baja y modificación de elementos patrimoniales con sus características técnicas e históricas.
2. **Seguimiento de Trazabilidad:** Mantener una bitácora completa de los estados del elemento (ej: de "Activo" a "En Restauración") y quién autorizó el cambio.
3. **Análisis de Deterioro:** Integración con la IA para procesar imágenes y detectar cambios significativos en las fachadas o elementos.
4. **Generación de Rutas:** Creación de recorridos optimizados en modalidad manual o automática según preferencias del usuario.
5. **Validación de Identidad:** Integración con un servicio externo (como RENAPER) para registrar a los ciudadanos que deseen subir "Relatos de Identidad" oficiales.
6. **Gestión de Notificaciones:** Enviar alertas automáticas a los restauradores cuando un elemento es marcado como "Sospechoso de Daño" por la IA.
7. **Visualización Multicanal:** La plataforma debe ser accesible vía Web para la administración y poseer una interfaz optimizada para dispositivos móviles para los turistas en la calle.

---

### **Consideraciones Técnicas Relevantes (Top Enunciados Pedidos)**

- **Arquitectura:** Se espera que la solución sea diseñada bajo un enfoque de **Microservicios** (Plan 23) o **Monolito** (Plan 08) según corresponda, justificando la elección.
- **Latencia:** Se debe proponer una estrategia para que el usuario no sufra un _timeout_ mientras el servicio de IA analiza las imágenes de las fachadas.
- **Persistencia:** Los datos de las rutas y las coordenadas deben persistirse en un modelo relacional, pero se sugiere evaluar si los "Relatos de Identidad" (comentarios masivos) convienen en una base de datos NoSQL Documental.
- **Atributos de Calidad:** Se dará especial importancia a la **Usabilidad** (por el público turista) y a la **Performance** (ante picos de tráfico en eventos culturales del municipio).
- **Seguridad:** Implementar un mecanismo de inicio de sesión único (SSO) para que los empleados municipales accedan con sus credenciales institucionales.