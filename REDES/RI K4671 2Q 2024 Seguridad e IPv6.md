## Resumen Estructurado: IPv6 vs. IPv4 y Seguridad (TLS y Criptografía)

### **I. Conceptos Fundamentales de Seguridad (Criptografía)**

La criptografía es la ciencia de leer y escribir mensajes codificados, fundamental para la seguridad de las comunicaciones. Sus componentes clave son:

- **Autenticación:** Establecer la identidad del cliente y/o servidor.
- **Integridad:** Asegurar que los datos no sean alterados en el camino.
- **Confidencialidad:** Garantizar que solo los participantes autorizados accedan al contenido.

Para lograr esto, se utilizan:

- **Algoritmo:** Función matemática para encriptar el mensaje.
- **Clave:** Valor secreto utilizado por el algoritmo.

**A. Tipos de Encriptación:**

1. **Encriptación Simétrica (Clave Secreta Única):**
    
    - **Mecanismo:** Usa una única clave compartida y el mismo algoritmo para encriptar y desencriptar.
    - **Ventaja:** Alta performance (más eficiente).
    - **Inconveniente Principal:** Distribución segura de la clave. Es trivial para dos interlocutores, pero un desafío en sistemas a escala con múltiples comunicaciones.
    - **Ejemplo:** Alice y Bob comparten una clave. Bob encripta, Alice desencripta con la misma clave.
    - **Algoritmos Conocidos:**
        
        - **Obsoletos:** DES, Triple DES (inaceptables hoy día).
        - **Actual:** AES (Advanced Encryption Standard) con longitudes de clave de 192 o 256 bits (256 es el mínimo confiable).
        
    - **Punto Clave:** La fortaleza depende de la longitud de la clave. Los requisitos de seguridad aumentan continuamente.
    
2. **Encriptación Asimétrica (Clave Pública/Privada):**
    
    - **Mecanismo:** Usa un par de claves (pública y privada). Una encripta, la otra desencripta. La privada se mantiene oculta, la pública se divulga libremente.
    - **Ventajas:**
        
        - Logra **integridad**, **confidencialidad**, **no repudio** (no se puede negar la autoría) y **autenticación**.
        
    - **Escenarios:**
        
        - **Confidencialidad:** Bob encripta con la clave pública de Alice. Solo Alice puede desencriptar con su clave privada. (No garantiza autenticación, cualquiera puede usar la clave pública de Alice).
        - **Autenticación y No Repudio (Doble Encriptación):** Bob encripta primero con su **clave privada** y luego con la **clave pública de Alice**.
            
            - Alice desencripta con su clave privada (garantiza confidencialidad).
            - El resultado es un mensaje cifrado que solo puede ser descifrado con la clave pública de Bob (garantiza autenticación y no repudio, ya que solo Bob pudo haber encriptado con su clave privada).
            
        
    - **Inconveniente:** Menor performance que la simétrica.
    - **Uso Práctico:** Se usa la asimétrica para establecer un canal seguro y autenticar, y luego se pasa a la simétrica para la comunicación de datos por su eficiencia.
    

**B. Integridad (Funciones Hash):**

- **Mecanismo:** Una función hash toma una entrada de longitud arbitraria y genera una salida de longitud fija llamada "digesto" o "fingerprint".
- **Proceso:** El mensaje se procesa con una función hash, se adjunta el digesto. El receptor recalcula el hash y compara los digestos para validar que el mensaje no fue alterado.
- **Requisitos de un Algoritmo Hash:**
    
    - **Consistencia:** Misma entrada = misma salida.
    - **Aleatoriedad:** Impide adivinar el mensaje original a partir del digesto.
    - **Unicidad:** Prácticamente imposible encontrar dos mensajes con el mismo digesto (no colisión).
    - **One-way (Unidireccional):** Imposible descifrar el mensaje de entrada a partir del digesto.
    
- **Algoritmos Conocidos:**
    
    - **Obsoletos:** MD4, MD5.
    - **Actual:** SHA (en versiones modernas como SHA-256, SHA-384, SHA-512).
    

**C. Firma Digital:**

- **Mecanismo:** Un digesto encriptado con la **clave privada del emisor** que se adiciona al documento.
- **Propósito:** Confirmar la identidad del emisor y garantizar la integridad del documento.
- **Proceso:**
    
    1. Se calcula el hash del mensaje.
    2. El hash se encripta con la clave privada del emisor (esto es la firma digital).
    3. El mensaje y la firma se envían.
    4. El receptor desencripta la firma con la clave pública del emisor (obtiene el digesto original).
    5. El receptor calcula el hash del mensaje recibido.
    6. Compara ambos digestos. Si coinciden, el mensaje no fue alterado y proviene del emisor autenticado.
    

### **II. TLS (Transport Layer Security)**

- **Origen:** Evolución de SSL (Secure Sockets Layer), inventado por Netscape en 1996.
- **Versiones:** SSLv3 (última versión segura de SSL), TLS 1.0, 1.1, 1.2 (mínimo actual para sitios seguros), 1.3 (más reciente, no tan ampliamente adoptado aún).
- **Uso:** Proteger el tráfico web entre un servidor HTTP y un navegador (HTTPS).
- **Mecanismo:** Utiliza una combinación de encriptación simétrica y asimétrica.
- **Funcionamiento (Handshake):**
    
    1. **Conexión:** Cliente se conecta al servidor en el puerto 443 (en lugar del 80 para HTTP).
    2. **Client Hello:** Cliente envía una lista de "cipher suites" (algoritmos de encriptación y hash soportados) y capacidades.
    3. **Server Hello + Certificado:** Servidor responde con su "cipher suite" elegida (la mejor opción compatible) y su certificado digital.
    4. **Intercambio de Claves y Establecimiento de Sesión:** Cliente y servidor negocian y establecen una clave simétrica para la sesión.
    5. **Comunicación Encriptada:** Una vez establecido el canal seguro, la comunicación HTTP se realiza sobre TLS.
    
- **Impacto en la Performance:**
    
    - El handshake TLS implica múltiples "round trips" (ida y vuelta) entre cliente y servidor.
    - Esto introduce un retardo significativo (ej. 3 round trips * 250ms = 750ms para una conexión intercontinental).
    - **Solución TLS 1.3:** Corre sobre UDP (QUIC) y optimiza el handshake para reducir el número de round trips.
    

**A. Certificados Digitales:**

- **Propósito:** Autenticar la identidad del servidor (y opcionalmente del cliente).
- **Contenido:**
    
    - **Subject (CN Field):** Nombre del host (ej. `google.com`). El navegador verifica que coincida con la URL.
    - **Subject Alternate Name (SAN):** Permite múltiples nombres alternativos para un certificado (útil para "wildcard certificates" como `*.google.com`).
    - **Issuer:** Quién emitió el certificado (Autoridad Certificante - CA).
    - **Período de Validez:** Fecha de inicio y fin.
    
- **Cadena de Confianza:**
    
    - Un certificado es válido si el cliente confía en la CA que lo emitió.
    - Las CAs tienen sus propios certificados (Root CA).
    - Los dispositivos (PC, móvil) tienen un repositorio de Root CAs confiables preinstalados (ej. Digicert en Windows porque pagó a Microsoft).
    - Puede haber certificados intermedios en la cadena.
    - Si el Root CA no está en el repositorio del cliente, el navegador mostrará una alerta de "sitio no seguro".
    
- **Certificados Auto-firmados (Self-Signed):**
    
    - Generados por el propio servidor o una CA privada.
    - Son seguros criptográficamente, pero no son confiables para navegadores públicos a menos que el Root CA se instale manualmente en el cliente (lo que genera una alerta).
    - Útiles para entornos internos o de desarrollo.
    
- **Autenticación Mutua (Opcional):**
    
    - El servidor puede solicitar un certificado al cliente.
    - **Inconveniente:** Tedioso de mantener, ya que los certificados expiran (1-2 años) y requieren renovación en ambos extremos.
    

### **III. IPv6 (Internet Protocol Version 6)**

- **Contexto:** Agotamiento de direcciones IPv4 (IANA entregó el último bloque /8 en 2011).
- **Soluciones al Agotamiento IPv4:**
    
    - Comprar bloques de direcciones.
    - CGNAT (Carrier-Grade NAT): Solución a gran escala para ISPs (ej. Telecentro).
    - Migrar a IPv6.
    
- **Longitud:** 128 bits (vs. 32 bits de IPv4). Ofrece un espacio de direcciones inmenso.
- **Asignación:** IANA -> Registros Regionales (ej. LACNIC) -> Registros Locales/ISPs -> Usuarios Finales.
- **Notación:** CIDR (ej. /64, /48).
- **Esquema de Asignación:**
    
    - **IANA:** Asigna bloques /12 a los RIRs.
    - **RIRs/ISPs:** Asignan bloques /32 a ISPs o /48 a clientes que necesitan subredes.
    - **Usuario Final:**
        
        - **Un solo dispositivo:** /128 (una dirección IP).
        - **Una red LAN:** /64 (los primeros 64 bits para la red, los últimos 64 para el identificador de interfaz).
        - **Subredes:** /48 (permite 65536 subredes /64).
        
    
- **Diferencia Conceptual con IPv4:**
    
    - En IPv4, una IP se asocia a un Host.
    - En IPv6, una IP se asocia a una **interfaz**. Un Host puede tener múltiples interfaces, y una interfaz puede tener múltiples direcciones IPv6.
    

**A. Cabecera IPv6:**

- **Longitud Fija:** 40 bytes (vs. 20-60 bytes de IPv4).
- **Campos Reducidos:** Elimina campos como ID, Offset, Flags de fragmentación (se mueven a cabeceras extensibles).
- **Campos Clave:**
    
    - **Version (4 bits):** Siempre 6.
    - **Traffic Class (8 bits):** Equivalente al Type of Service/DSCP de IPv4 (QoS).
    - **Flow Label (20 bits):** **Novedoso.** Permite identificar una serie de paquetes como parte de un "flujo". El router puede tomar la decisión de enrutamiento una vez para el primer paquete y aplicarla a los siguientes, reduciendo el costo de procesamiento.
    - **Payload Length (16 bits):** Longitud de la carga útil (hasta 65535 bytes).
    - **Next Header (8 bits):** Equivalente al campo "Protocol" de IPv4. Indica el tipo de cabecera siguiente (TCP, UDP, ICMPv6 o una cabecera extensible).
    - **Hop Limit (8 bits):** Equivalente al TTL de IPv4. Límite de saltos.
    - **Source Address (128 bits):** Dirección IP de origen.
    - **Destination Address (128 bits):** Dirección IP de destino.
    

**B. Cabeceras Extensibles:**

- Reemplazan las "opciones" de IPv4.
- Permiten añadir funcionalidades adicionales sin aumentar la complejidad de la cabecera base.
- **Ejemplos:**
    
    - **Routing Header:** Para Source Routing (especificar la ruta).
    - **Fragment Header:** Para fragmentación y reensamblado.
    

**C. Sintaxis de Direcciones IPv6:**

- **Representación:** Hexadecimal, grupos de 16 bits separados por dos puntos (ej. `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).
- **Técnicas de Compactación:**
    
    - **Supresión de Ceros:** Eliminar ceros no significativos (ej. `0db8` -> `db8`).
    - **Compresión de Ceros:** Reemplazar una secuencia larga de ceros con `::` (doble dos puntos). Solo se puede usar una vez por dirección. (ej. `fe80:0000:0000:0000:8a2e:0370:7334` -> `fe80::8a2e:0370:7334`).
    
- **Direcciones Notables:**
    
    - **Loopback:** `::1` (equivalente a `127.0.0.1`).
    - **Default Route:** `::/0` (equivalente a `0.0.0.0/0`).
    

**D. Tipos de Direcciones IPv6:**

1. **Global Unicast Address (GUA):**
    
    - **Equivalente:** Dirección IPv4 pública.
    - **Propósito:** Direcciones únicas, enrutables globalmente en Internet.
    - **Estructura:** Prefijo de ruteo global (48 bits), ID de subred (16 bits), ID de interfaz (64 bits).
    - **Características:** Diseñadas para ser sumarizadas (agrupadas por región).
    
2. **Link-Local Address (LLA):**
    
    - **Prefijo:** Siempre comienzan con `fe80::/10`.
    - **Propósito:** Comunicación solo dentro del segmento de red local (LAN). No son enrutables fuera del enlace.
    - **Generación:** Se auto-configuran automáticamente en cada interfaz IPv6 activa.
    - **Uso:** Para comunicación con vecinos directos, descubrimiento de routers (Router Advertisement), y reemplazo de ARP (Neighbor Discovery Protocol).
    - **Conflicto de Ambigüedad:** Si un host tiene múltiples interfaces (ej. Wi-Fi, Ethernet, VMs), cada una tendrá una LLA. Para comunicarse con una LLA específica, se debe especificar la interfaz de salida (ej. `ping fe80::xxxx%interfaz_id`).
    
3. **Unique Local Address (ULA):**
    
    - **Prefijo:** Comienzan con `fd00::/8`.
    - **Propósito:** Direccionamiento privado para uso dentro de una organización, sin necesidad de conexión a Internet. Equivalente a las direcciones privadas IPv4 (RFC 1918).
    - **Generación:** Se recomienda generar el Global ID de 40 bits de forma aleatoria siguiendo una RFC para asegurar unicidad global (aunque no son enrutables globalmente, esto permite una posible futura unificación).
    - **Características:** No están diseñadas para ser sumarizadas.
    
4. **Multicast Address:**
    
    - **Prefijo:** Comienzan con `ff00::/8`.
    - **Equivalente:** Direcciones Clase D de IPv4.
    - **Importante:** **No existe la dirección de broadcast en IPv6.** El multicast reemplaza el broadcast.
    - **Ventaja sobre Broadcast IPv4:** Un datagrama multicast se encapsula en una trama Ethernet con una dirección MAC de destino reservada para multicast. Esto significa que solo los dispositivos interesados en ese grupo multicast procesarán la trama, evitando molestar a todos los dispositivos de la LAN (incluidos los que solo usan IPv4).
    - **Ejemplo:** `ff02::1` (link-local all nodes) es el equivalente funcional al broadcast de IPv4.
    

**E. Identificador de Interfaz (64 bits):**

- **Propósito:** Identificar de forma única una interfaz dentro de una subred /64.
- **Métodos de Generación:**
    
    1. **EUI-64 (Extended Unique Identifier):**
        
        - Derivado de la dirección MAC (48 bits) de la interfaz.
        - Se inserta `ff:fe` en el medio de la MAC para extenderla a 64 bits.
        - **Inconveniente:** Revela la MAC del dispositivo, lo que podría ser un riesgo de privacidad/seguridad.
        
    2. **Generación Aleatoria (Privacy Extensions):**
        
        - El sistema operativo genera un ID aleatorio de 64 bits.
        - **Ventaja:** Protege la privacidad al no exponer la MAC.
        - **Windows:** Por defecto, prefiere generar IDs aleatorios y temporales (cambian periódicamente).
        
    3. **DHCPv6:**
        
        - Un servidor DHCPv6 puede asignar direcciones IPv6.
        - Menos común que la autoconfiguración.
        
    4. **Manual:** Asignación manual (poco práctico).
    

**F. Autoconfiguración (SLAAC - Stateless Address Autoconfiguration):**

- **Mecanismo:** Los hosts IPv6 pueden auto-configurar sus direcciones IP (incluyendo las GUA) sin necesidad de un servidor DHCPv6.
- **Proceso:**
    
    1. El router de la red envía mensajes **Router Advertisement (RA)**.
    2. Los RA contienen el prefijo de red (ej. `/64`) y otra información.
    3. El host combina este prefijo con un ID de interfaz generado (ej. aleatorio o EUI-64) para formar su dirección IPv6 completa.
    
- **Dual Stack:** La mayoría de los sistemas operativos y redes actuales operan en "dual stack", soportando tanto IPv4 como IPv6 simultáneamente. El sistema operativo prefiere IPv6 si está disponible.
- **Incompatibilidad Directa:** No hay forma directa de comunicar un nodo IPv4 con un nodo IPv6. Se requiere que ambos extremos soporten el mismo protocolo (IPv4 o IPv6) o el uso de mecanismos de transición (túneles, proxies, etc., no cubiertos en la transcripción).

---

## Posibles Preguntas y Respuestas para Exámenes

### **Preguntas sobre Criptografía y Seguridad:**

1. **Pregunta:** ¿Cuáles son los tres pilares fundamentales de la seguridad en las comunicaciones y cómo los aborda la criptografía?
    
    - **Respuesta:**
        
        - **Confidencialidad:** Nadie excepto los autorizados puede acceder al contenido. Se logra con encriptación (simétrica o asimétrica).
        - **Integridad:** Los datos no han sido alterados. Se logra con funciones hash.
        - **Autenticación:** Establecer la identidad de las partes. Se logra con encriptación asimétrica (firmas digitales) y certificados.
        
    
2. **Pregunta:** Compare la encriptación simétrica y asimétrica en términos de eficiencia y principal desafío. ¿Cómo se combinan en la práctica?
    
    - **Respuesta:**
        
        - **Simétrica:** Más eficiente/rápida. Desafío principal: distribución segura de la clave.
        - **Asimétrica:** Menos eficiente. Desafío principal: no tiene, ya que la clave pública se distribuye libremente.
        - **Combinación:** La asimétrica se usa para establecer un canal seguro y autenticar (intercambiar claves), y luego se pasa a la simétrica para la comunicación de datos por su mayor eficiencia.
        
    
3. **Pregunta:** Explique el concepto de "no repudio" y cómo se logra con la encriptación asimétrica.
    
    - **Respuesta:** El no repudio significa que el emisor no puede negar haber enviado un mensaje. Se logra cuando el emisor encripta el mensaje (o su hash) con su **clave privada**. Solo él posee esa clave, por lo que si el mensaje puede ser desencriptado con su clave pública, se prueba su autoría.
    
4. **Pregunta:** ¿Qué es una función hash y qué propiedades debe cumplir para ser considerada segura?
    
    - **Respuesta:** Una función hash es un algoritmo que transforma una entrada de cualquier longitud en una salida de longitud fija (digesto). Propiedades: Consistencia, Aleatoriedad, Unicidad (no colisiones), y One-way (unidireccional).
    
5. **Pregunta:** Describa el proceso de una firma digital y su propósito.
    
    - **Respuesta:** Se calcula el hash del mensaje, se encripta con la clave privada del emisor (creando la firma). El receptor desencripta la firma con la clave pública del emisor para obtener el hash original, y lo compara con el hash del mensaje recibido. Propósito: Autenticar al emisor y garantizar la integridad del documento.
    

### **Preguntas sobre TLS:**

1. **Pregunta:** ¿Cuál es la relación entre SSL y TLS? ¿Qué puerto utiliza HTTPS y por qué?
    
    - **Respuesta:** TLS es la evolución y el sucesor de SSL. HTTPS utiliza el puerto 443 para establecer una conexión segura sobre TLS, a diferencia del puerto 80 de HTTP.
    
2. **Pregunta:** Explique el rol de un certificado digital en TLS y qué información clave contiene.
    
    - **Respuesta:** Un certificado digital autentica la identidad del servidor. Contiene el nombre del host (Subject/CN), el emisor (Issuer/CA), y el período de validez.
    
3. **Pregunta:** ¿Cómo se establece la "cadena de confianza" para validar un certificado TLS? ¿Qué sucede si un certificado auto-firmado se presenta a un navegador público?
    
    - **Respuesta:** La cadena de confianza se establece si el cliente confía en la Autoridad Certificante (CA) que emitió el certificado. Los dispositivos tienen un repositorio de Root CAs confiables. Si un certificado auto-firmado se presenta, el navegador mostrará una alerta de seguridad porque la CA no es confiable para el cliente.
    
4. **Pregunta:** ¿Por qué el handshake de TLS puede introducir un retardo significativo en la comunicación, y cómo TLS 1.3 busca mitigar esto?
    
    - **Respuesta:** El handshake implica múltiples intercambios (round trips) entre cliente y servidor antes de que los datos puedan ser enviados. TLS 1.3, al correr sobre UDP (QUIC), optimiza el handshake para reducir el número de round trips, disminuyendo el retardo.
    

### **Preguntas sobre IPv6:**

1. **Pregunta:** ¿Cuál es la principal razón para la creación de IPv6 y cuál es su longitud de dirección?
    
    - **Respuesta:** La principal razón es el agotamiento de direcciones IPv4. La longitud de una dirección IPv6 es de 128 bits.
    
2. **Pregunta:** Describa dos técnicas para compactar la representación de direcciones IPv6.
    
    - **Respuesta:**
        
        - **Supresión de ceros:** Eliminar ceros no significativos (ej. `0db8` a `db8`).
        - **Compresión de ceros:** Reemplazar una secuencia contigua de ceros con `::` (solo una vez por dirección).
        
    
3. **Pregunta:** Compare las direcciones Global Unicast (GUA), Link-Local (LLA) y Unique Local (ULA) en IPv6, indicando su propósito y prefijo.
    
    - **Respuesta:**
        
        - **GUA:** Equivalente a IPv4 pública, enrutable globalmente. Prefijo variable (ej. `2001::/16`).
        - **LLA:** Para comunicación solo en el segmento de red local. Prefijo `fe80::/10`. Se auto-configura.
        - **ULA:** Para direccionamiento privado dentro de una organización. Prefijo `fd00::/8`.
        
    
4. **Pregunta:** ¿Por qué se dice que en IPv6 no existe el broadcast y cómo se reemplaza su funcionalidad? ¿Qué ventaja tiene este enfoque?
    
    - **Respuesta:** En IPv6 no existe el broadcast. Su funcionalidad se reemplaza por direcciones **multicast**. La ventaja es que los datagramas multicast se encapsulan en tramas Ethernet con direcciones MAC específicas para multicast, lo que significa que solo los dispositivos interesados en ese grupo procesarán la trama, reduciendo la carga en otros dispositivos de la red.
    
5. **Pregunta:** Explique el concepto de "Flow Label" en la cabecera IPv6 y su beneficio.
    
    - **Respuesta:** El Flow Label es un campo de 20 bits que permite identificar una secuencia de paquetes como parte de un mismo "flujo". Esto permite a los routers tomar una decisión de enrutamiento para el primer paquete del flujo y aplicarla a los siguientes, optimizando el procesamiento y mejorando la eficiencia del reenvío.
    
6. **Pregunta:** ¿Cómo se genera el Identificador de Interfaz de 64 bits en IPv6? Mencione al menos dos métodos y sus implicaciones.
    
    - **Respuesta:**
        
        - **EUI-64:** Derivado de la dirección MAC de la interfaz. Implicación: revela la MAC, lo que puede ser un riesgo de privacidad.
        - **Generación Aleatoria (Privacy Extensions):** El sistema operativo genera un ID aleatorio. Implicación: protege la privacidad al no exponer la MAC, y Windows lo usa por defecto, generando IDs temporales.
        
    
7. **Pregunta:** ¿Qué es la autoconfiguración (SLAAC) en IPv6 y cómo funciona?
    
    - **Respuesta:** SLAAC permite a los hosts IPv6 auto-configurar sus direcciones IP sin un servidor DHCPv6. El router de la red envía mensajes Router Advertisement (RA) con el prefijo de red, y el host combina este prefijo con un ID de interfaz generado (ej. aleatorio) para formar su dirección IPv6 completa.