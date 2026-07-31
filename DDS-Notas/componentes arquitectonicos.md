### I. Componentes Arquitectónicos de Diseño e Integración

La arquitectura moderna se apoya en componentes especializados que permiten **reutilizar** funcionalidades críticas en lugar de desarrollarlas desde cero.

1. **Gestión de Identidad y Acceso (IAM / IDM):**
    
    - **IAM (Identity and Access Management):** Se concentra en el **acceso** y define las estrategias del "cómo" se ingresa al sistema (ej. contraseñas, biometría, MFA).
    - **IDM (Identity Management):** Se centra en la **identidad** del usuario (quién es), gestionando sus nombres, roles, permisos y grupos a los que pertenece.
    - **SSO (Single Sign-On):** Componente complementario que permite acceder a múltiples aplicaciones con una **única cuenta** y sesión válida, mejorando la experiencia del usuario y centralizando la gestión.
2. **Red de Distribución de Contenido (CDN):**
    
    - Es una red de servidores distribuidos geográficamente que trabajan en conjunto para entregar **contenido estático** (HTML, JS, CSS, imágenes, videos) de forma rápida.
    - Funciona como un mecanismo de integración que se sitúa delante de los servidores de origen para absorber tráfico y mejorar la disponibilidad.
3. **Memoria Caché:**
    
    - Mecanismo para guardar respuestas a solicitudes HTTP. Si la solicitud es la misma, se entrega la respuesta guardada sin reprocesar en el servidor.
    - Puede implementarse en múltiples niveles: **privada** (en el navegador del usuario) o **compartida** (CDN, Proxies reversos).

---

### II. Preguntas y Respuestas para Exámenes

**1. ¿Cuál es la diferencia conceptual entre Autenticación y Autorización?**

- **Respuesta:** La **autenticación** confirma que los usuarios son quienes dicen ser (valida la identidad). La **autorización** otorga a esos usuarios ya autenticados el permiso para acceder a un recurso específico.

**2. ¿Por qué se recomienda el uso de componentes de terceros para IAM en lugar de desarrollos propios?**

- **Respuesta:** Por el **principio de reuso**. Estos componentes ya funcionan, están en el mercado y se actualizan constantemente con nuevas funcionalidades y parches de seguridad, lo cual es más eficiente que el mantenimiento interno.

**3. ¿Qué problemas soluciona una CDN en una arquitectura web?**

- **Respuesta:** Reduce la carga de los servidores de origen, disminuye la latencia al entregar contenido desde el servidor más cercano al usuario, reduce costos de ancho de banda y protege contra ataques de denegación de servicio (DDoS).

**4. ¿Qué sucede si un sistema de SSO se cae?**

- **Respuesta:** Se pierde el acceso a todos los sistemas y servicios referenciados con esa cuenta, ya que es el único punto de validación.

**5. ¿Qué importancia tiene el "tiempo de expiración" en una caché?**

- **Respuesta:** Es crítico para asegurar que el contenido sea reemplazado. Sin un tiempo de expiración o una estrategia de invalidación, el usuario podría ver contenido viejo o desactualizado.

**6. ¿Cómo puede una caché afectar la capa de datos?**

- **Respuesta:** Herramientas como Hibernate implementan una caché contra el motor de base de datos para evitar consultas repetitivas, mejorando la velocidad de respuesta de la capa de negocio.

---

### III. Lecciones Aprendidas y Principios de Diseño

- **Transversalidad:** Componentes como el IAM son transversales a todas las aplicaciones; no deben replicarse, sino centralizarse.
- **Eficiencia de Recursos:** El uso de caché y CDN maximiza la eficiencia tanto en el uso de hardware/infraestructura como en los tiempos de respuesta para el cliente.
- **Seguridad por Capas:** Las CDN actúan como una barrera de seguridad adicional al detectar y mitigar ataques antes de que lleguen al servidor principal.
- **Compromiso de Actualización:** El uso de caché implica un riesgo de visualización de datos obsoletos; el diseño debe contemplar mecanismos para invalidar o refrescar la información cuando el origen cambia.

---

### IV. Catálogo de Ejemplos y Soluciones

- **Ejemplos de Componentes IAM/IDM:**
    
    - **Keycloak:** Para despliegue en ambientes propios (on-premise).
    - **Open IAM:** Otra opción de gestión de identidad.
    - **Auth0:** Servicio gestionado directamente en la nube que permite integración ágil con redes sociales y cuentas corporativas (Gmail, Outlook).
- **Ejemplos de Integración SSO:**
    
    - **Login Social:** Usar la sesión activa de una red social para entrar en diversas aplicaciones sin crear nuevos usuarios.
- **Ejemplos de Implementación de Caché:**
    
    - **Caché Local (F12):** Al presionar F5 en un navegador, el contenido estático se carga desde el disco local, no del servidor.
    - **Proxy Reverso (HAProxy):** Servidor intermedio que cachea rutas y envía respuestas directamente sin hablar con la aplicación.
    - **Caché de Datos (Hibernate):** Caché que evita peticiones redundantes al motor de base de datos.
    - **Caché por Cabeceras:** Modificar el contenido cacheado en el servidor según parámetros del header HTTP, como el **idioma** del usuario.
- **Ejemplos de Contenido Estático para CDN:**
    
    - Archivos HTML, hojas de estilo CSS, scripts de JavaScript, imágenes y videos pesados.