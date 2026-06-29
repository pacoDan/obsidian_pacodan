Aquí tienes el árbol en formato de comando `tree` con las preguntas extraídas y formuladas a partir de todas las temáticas y consultas explícitas que se discutieron en las fuentes proporcionadas:

Plaintext

```
Fuentes_de_Estudio
├── RI_dns_preparcial_2024_09_23.txt
│   ├── ¿Cómo funcionaría el tema del multicast en un switch de capa 2?
│   ├── ¿El switch sabe los grupos de multicast o el tráfico se trata como si fuera broadcast?
│   ├── ¿Qué es el IGMP Snooping y cómo permite interpretar mensajes multicast?
│   ├── ¿Se transmite la secuencia entera de la dirección MAC al revés o el octavo bit se transmite primero?
│   ├── ¿Cómo serían los ejercicios sobre capturas de Wireshark en el parcial?
│   ├── ¿Esta trama de Ethernet seleccionada entra o sale de la red y cómo se deduce a partir de las IPs locales y públicas?
│   ├── ¿Cuál es la dirección MAC destino en la captura y a qué dispositivo (router) pertenece?
│   ├── ¿Cómo entraría un datagrama IP de 65.000 bytes en un campo de datos de Ethernet que tiene un límite de 1.500 bytes de MTU?
│   ├── ¿Se debe fragmentar siempre el datagrama o la capa IP ya conoce el MTU máximo que soporta la capa 2?
│   ├── ¿Se aprovecha alguna vez el tamaño máximo sin fragmentar mediante Jumbo Frames?
│   ├── ¿Puede haber colisiones cuando se transmite en redes Wireless utilizando DCF sin RTS/CTS?
│   ├── ¿Cómo se entera el emisor de que ocurrió una colisión en Wireless si no puede sensar el medio mientras transmite (nodo oculto)?
│   ├── ¿En una conexión VPN, se conecta a un usuario en particular (Remote User) o a toda una organización (Site-to-Site)?
│   └── ¿Qué significan y en qué se diferencian el modo transporte y el modo túnel en IPsec?
├── RI_K4051_1Q_2024_TCP.txt
│   ├── ¿Qué características principales tiene el protocolo TCP?
│   ├── ¿Por qué el modelo plantea hacer un control de errores en dos capas distintas (Capa 2 y Capa 4)?
│   ├── ¿Qué concepto representan el "Puerto origen" y "Puerto destino" en TCP y qué identifican en el Host?
│   ├── ¿Se puede saber a nivel de sistema operativo qué proceso tiene abierta cierta conexión o puerto?
│   ├── ¿Para qué sirven los campos "Número de secuencia" y "Número de confirmación"?
│   ├── ¿Cómo se elige el número de secuencia inicial (ISN) y por qué no siempre empieza en cero?
│   ├── ¿Qué información exacta contiene el campo de confirmación (Acknowledge) enviado al emisor?
│   ├── ¿Para qué sirve el campo "Longitud de la cabecera" en TCP?
│   ├── ¿Qué valida el Checksum en TCP y en qué se diferencia de la comprobación que hace IP?
│   ├── ¿Qué indica el flag y el "Puntero a urgente" dentro de la cabecera TCP?
│   ├── ¿Qué utilidad tiene el flag PUSH en aplicaciones interactivas como Telnet?
│   ├── ¿Qué permite la opción MSS (Maximum Segment Size) y en qué momento se declara?
│   ├── ¿Por qué se utiliza el MSS y cómo se calcula restando bytes de cabecera al MTU?
│   ├── ¿Qué es la opción Timestamp y para qué se usa en la medición de tiempos (Round Trip)?
│   ├── ¿Cómo funciona el establecimiento de conexión mediante el Handshake de tres vías (SYN, SYN-ACK, ACK)?
│   └── ¿Por qué es importante entender el estado de las conexiones (Stateful) para la configuración de un Firewall?
├── RI_K4571_2Q_2024_ATM_MPLS.txt
│   ├── ¿Por qué el protocolo ATM utiliza una celda de longitud fija y tan pequeña (53 bytes)?
│   ├── ¿De qué manera el uso de celdas pequeñas y de longitud fija reduce la latencia en la conmutación?
│   ├── ¿Qué significa soportar "Calidad de Servicio" (QoS) para tráfico de voz y datos integrados?
│   ├── ¿Por qué ATM requiere una subcapa de adaptación (AAL) para no saturar la red con el overhead de datagramas IP pequeños?
│   ├── ¿Qué representan el identificador de camino virtual (VPI) y el identificador de canal virtual (VCI)?
│   ├── ¿Cuál es la diferencia entre la interfaz usuario-red (UNI) y la interfaz red-red (NNI) en la cabecera ATM?
│   ├── ¿Para qué sirve el bit CLP (Cell Loss Priority) en situaciones de congestión?
│   ├── ¿Qué es la señalización en ATM y cómo se utiliza para levantar Circuitos Virtuales Conmutados (SVC)?
│   ├── ¿Qué sucede si la red no puede satisfacer los parámetros de Calidad de Servicio solicitados por un usuario?
│   └── ¿Qué función cumple el campo HEC (Header Error Control) y cuántos bits errados de la cabecera permite corregir?
├── RI_K4573_K4671_2Q_2024_LAN_II.txt
│   ├── ¿El algoritmo Spanning Tree (STP) se ejecuta por sí solo al conectar los cables o requiere intervención manual?
│   ├── ¿Cómo decide el switch qué puerto específico bloquear para evitar el bucle en la red?
│   ├── ¿El Spanning Tree opera por cada VLAN o trae una configuración predeterminada global (VLAN 1)?
│   ├── ¿Si un puerto está bloqueado por STP, cómo hace un dispositivo para enviarle tráfico al switch contiguo?
│   ├── ¿Los comandos de configuración por línea de comandos (CLI) mostrados son exclusivos de Cisco o se aplican a otras marcas?
│   ├── ¿Qué pasa con el rendimiento de la red si se llena la memoria CAM del switch por una inundación de direcciones MAC?
│   ├── ¿Cuándo se sobrepasa el límite de memoria, la dirección MAC que se elimina mediante FIFO es la más vieja?
│   ├── ¿Qué son las VLANs (Redes de Área Local Virtuales) y cómo dividen los dominios de broadcast?
│   └── ¿Qué sentido o ventaja económica tiene implementar VLANs por puerto en un escenario como un Data Center?
├── RI_K4573_2Q_2024_IP_II_subnetting_ARP.txt
│   ├── ¿Qué es la interfaz Loopback (127.0.0.1) y cómo responde independientemente del estado físico de la red?
│   ├── ¿Por qué no es posible usar IPs de una red existente (ej. .100 y .110) en una sucursal separada por un router sin hacer subnetting?
│   ├── ¿Qué es exactamente el subnetting y cómo permite reasignar bits de Host a bits de Subred?
│   ├── ¿Por qué se decide partir el espacio tomando 2 bits (cuatro subredes) en lugar de tomar solo 1 bit?
│   ├── ¿Cuál es el identificador de red y el identificador de broadcast de cada uno de los fragmentos generados?
│   ├── ¿Por qué los documentos históricos (RFCs) desaconsejaban utilizar la subred "todos ceros" y la subred "todos unos"?
│   ├── ¿Qué parte de la configuración del Host se debe modificar obligatoriamente tras implementar el subnetting?
│   ├── ¿Por qué la nueva máscara de subred (ej. 255.255.255.192) se forma añadiendo "unos" desde la izquierda del octeto?
│   ├── ¿Qué tan costoso operativamente es modificar máscaras de red fijas en equipos geográficamente distribuidos?
│   └── ¿Existe el concepto de subred anidada (VLSM) para seguir dividiendo una subred previamente particionada?
├── RI_K4671_2Q_2024_IP.txt
│   ├── ¿Por qué es fundamental que todos los dispositivos de la internet hablen un protocolo de Capa 3 común?
│   ├── ¿Qué significa que IP sea un protocolo "no orientado a la conexión" (Connectionless)?
│   ├── ¿A qué se refiere el modelo cuando describe a IP como un servicio de entrega "no confiable" (Best Effort)?
│   ├── ¿Por qué se afirma que los routers no guardan un estado o memoria de cómo enrutaron paquetes de un mismo flujo anteriormente?
│   ├── ¿Qué información esencial transporta la cabecera estándar de IPv4 (20 bytes mínimos)?
│   ├── ¿Por qué la cabecera IP posee un campo "Header Length" y en qué casos su longitud es variable (Opciones)?
│   ├── ¿Para qué sirve el campo Time To Live (TTL) y cómo previene que paquetes queden atrapados en bucles infinitos?
│   ├── ¿Qué define el valor del TTL inicial asignado al enviar un paquete desde el origen?
│   ├── ¿Qué mecanismo de detección de errores utiliza IPv4 en su cabecera y qué pasa si falla?
│   ├── ¿Por qué IP utiliza un Checksum simple en lugar de usar un cálculo polinómico CRC como ocurre en Capa 2?
│   └── ¿Qué indica el campo "Protocolo" (ej. valores 1, 6 o 17) para facilitar el demultiplexado en el destino?
├── RI_K4671_2Q_2024_Routing_Protocols.txt
│   ├── ¿Cómo encuentra un router el camino para llegar a un destino remoto leyendo el datagrama IP?
│   ├── ¿De dónde proviene la información que puebla las tablas de ruteo en los sistemas operativos y routers?
│   ├── ¿Cómo se resuelve el ruteo mediante el concepto de la "coincidencia más larga" (Longest Match)?
│   ├── ¿El "cero" en una máscara de subred se interpreta como un comodín al buscar coincidencias en la tabla?
│   ├── ¿El procesador debe barrer obligatoriamente todas las entradas de la tabla de ruteo para hallar el Longest Match?
│   ├── ¿Qué es la ruta por defecto (Default Route, 0.0.0.0) y por qué se utiliza como último recurso?
│   ├── ¿Qué significa el término "On-link" o "directamente conectado" en las entradas del Gateway de una PC?
│   ├── ¿Las PCs convencionales realizan encaminamiento al igual que lo hace un router comercial?
│   ├── ¿Qué problemas de escalabilidad y tolerancia a fallos produce depender únicamente de ruteo estático?
│   ├── ¿Se podría solucionar la falta de conectividad poniendo una ruta por defecto apuntando a ciegas a otra sucursal?
│   ├── ¿Para qué sirven los protocolos de ruteo dinámico y cómo intercambian información de topología automáticamente?
│   └── ¿Cuáles son los objetivos ideales de un protocolo de ruteo (convergencia rápida, flexibilidad, optimización)?
├── RI_K4671_2Q_2024_Seguridad_e_IPv6.txt
│   ├── ¿En qué consisten los conceptos de Autenticación, Integridad, Confidencialidad y No Repudio en criptografía?
│   ├── ¿Cuál es la diferencia de funcionamiento y performance entre la encripción Simétrica y la Asimétrica?
│   ├── ¿Cómo se garantiza que un mensaje sea confidencial utilizando únicamente la clave pública del destinatario?
│   ├── ¿Cómo se implementa la "doble encripción" para poder validar al mismo tiempo el origen y mantener el secreto?
│   ├── ¿Qué características definen a una buena función de Hash (consistencia, no colisión, irreversibilidad)?
│   ├── ¿Qué es una Firma Digital y cómo se construye combinando un digesto Hash y el cifrado asimétrico?
│   ├── ¿Si alguien intercepta o altera la clave pública, es posible descifrar y comprometer el mensaje original?
│   ├── ¿Cómo opera el protocolo TLS (ex SSL) y cómo establece el canal seguro sobre un Handshake de TCP?
│   ├── ¿Qué parámetros se negocian durante los mensajes "Client Hello" y "Server Hello" entre navegador y servidor?
│   ├── ¿Qué es un Certificado Digital, quién actúa como Autoridad Certificante (CA) y cómo se confía en un Root CA?
│   └── ¿El candado de seguridad garantiza que la página no contenga malware, o solamente certifica su identidad?
└── RI_K4671_2Q_2024_VPN_DHCP_DNS.txt
    ├── ¿Qué es una Red Privada Virtual (VPN) y cómo emula circuitos dedicados sobre internet?
    ├── ¿Cuáles son las diferencias de aplicación entre una VPN Remote User y un túnel Site-to-Site?
    ├── ¿Qué es la suite IPsec y cuáles son las diferencias entre la cabecera AH (solo autenticación) y ESP (encripción)?
    ├── ¿En qué difiere aplicar IPsec en "Modo Transporte" frente a hacerlo en "Modo Túnel" para interconectar redes?
    ├── ¿Qué acciones se llevan a cabo durante la Fase 1 (IKE) y la Fase 2 en la negociación de un túnel?
    ├── ¿Cuáles son los mecanismos de autenticación entre pasarelas (Pre-shared key vs Certificados digitales)?
    ├── ¿Qué es el "Dominio de Encripción" y qué ocurre si las propuestas de ambos extremos de la VPN no son espejadas?
    ├── ¿Qué es un "Transform Set" y cómo se agrupan en él las políticas de integridad y cifrado?
    ├── ¿Qué función cumple aplicar el "Crypto Map" a una interfaz perimetral de un router Cisco?
    ├── ¿Cómo determina el equipo qué tráfico entra al túnel utilizando las listas de control de acceso (Access List)?
    └── ¿Qué es una "Wildcard mask" en la configuración del IOS de Cisco y cómo se relaciona con la máscara convencional?
```