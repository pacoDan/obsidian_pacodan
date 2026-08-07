### Parte 1: Resolución de los 4 Requerimientos Iniciales de "Código a Voluntad"

Para sentar bases sólidas de **Modelado de Objetos** que faciliten la posterior adición de requerimientos complejos (típicos de parcial), se propone el siguiente diseño conceptual en pseudocódigo de dominio:

```java
// === ENUMS Y VALUE OBJECTS ===

enum TipoColectivo {
    FUNDACION, ASOCIACION_BARRIAL, ORGANIZACION_SOCIAL, ONG, ASAMBLEA
}

// Representa el compromiso de tiempo (Value Object)
class Compromiso {
    private int horas;
    private TipoCompromiso tipo; // TOTALES, SEMANALES, MENSUALES

    public Compromiso(int horas, TipoCompromiso tipo) {
        this.horas = horas;
        this.tipo = tipo;
    }
}

enum TipoCompromiso {
    TOTALES, SEMANALES, MENSUALES
}

enum ModalidadColaboracion {
    GRATUITA, CON_INCENTIVO
}

// === ENTIDADES PRINCIPALES ===

class Habilidad {
    private String titulo;
    private String codigo; // Título normalizado para unicidad
    private String descripcion;

    public Habilidad(String titulo, String descripcion) {
        this.titulo = titulo;
        this.codigo = normalizar(titulo);
        this.descripcion = descripcion;
    }

    private String normalizar(String texto) {
        return texto.trim().toLowerCase().replaceAll("\\s+", "-");
    }
}

class Colectivo {
    private String nombre;
    private String descripcion;
    private String ubicacion;
    private TipoColectivo tipo;
    private List<Proyecto> proyectos = new ArrayList<>();

    public Colectivo(String nombre, String descripcion, String ubicacion, TipoColectivo tipo) {
        this.nombre = nombre;
        this.descripcion = descripcion;
        this.ubicacion = ubicacion;
        this.tipo = tipo;
    }

    public void registrarProyecto(Proyecto proyecto) {
        this.proyectos.add(proyecto);
    }

    // REQUERIMIENTO 4: Saber si un colectivo puede acceder a los datos de contacto de una colaboradora
    public boolean puedeVerDatosDe(PersonaColaboradora colaboradora) {
        return proyectos.stream().anyMatch(proyecto -> proyecto.tieneColaboradora(colaboradora));
    }
}

class Proyecto {
    private String titulo;
    private String descripcion;
    private Set<Habilidad> habilidadesRequeridas = new HashSet<>();
    private Compromiso compromiso; // Opcional
    private ModalidadColaboracion modalidad;
    private List<Colaboracion> colaboraciones = new ArrayList<>();

    public Proyecto(String titulo, String descripcion, ModalidadColaboracion modalidad, Compromiso compromiso) {
        this.titulo = titulo;
        this.descripcion = descripcion;
        this.modalidad = modalidad;
        this.compromiso = compromiso;
    }

    public void agregarHabilidadRequerida(Habilidad habilidad) {
        this.habilidadesRequeridas.add(habilidad);
    }

    // REQUERIMIENTO 2: Saber si una colaboradora cumple con al menos una habilidad requerida
    public boolean aceptaA(PersonaColaboradora colaboradora) {
        return habilidadesRequeridas.stream()
                .anyMatch(habilidadReq -> colaboradora.poseeHabilidad(habilidadReq));
    }

    public void registrarColaboracion(Colaboracion colaboracion) {
        this.colaboraciones.add(colaboracion);
    }

    public boolean tieneColaboradora(PersonaColaboradora colaboradora) {
        return colaboraciones.stream()
                .anyMatch(c -> c.getColaboradora().equals(colaboradora));
    }
}

class PersonaColaboradora {
    private String nombre;
    private String apellido;
    private boolean esAnonimo;
    private String correoContacto;
    private Set<Habilidad> habilidades = new HashSet<>();
    private List<Colaboracion> historialColaboraciones = new ArrayList<>();

    public PersonaColaboradora(String nombre, String apellido, boolean esAnonimo, String correoContacto) {
        this.nombre = nombre;
        this.apellido = apellido;
        this.esAnonimo = esAnonimo;
        this.correoContacto = correoContacto;
    }

    public void agregarHabilidad(Habilidad habilidad) {
        this.habilidades.add(habilidad);
    }

    public boolean poseeHabilidad(Habilidad habilidad) {
        return this.habilidades.contains(habilidad);
    }

    // REQUERIMIENTO 3: Como colaboradora, poder anotarse en un proyecto, estableciendo una colaboración
    public void anotarseEn(Proyecto proyecto) {
        if (!proyecto.aceptaA(this)) {
            throw new HabilidadesInsuficientesException("La colaboradora no cuenta con ninguna de las habilidades requeridas.");
        }
        Colaboracion nuevaColaboracion = new Colaboracion(this, proyecto);
        proyecto.registrarColaboracion(nuevaColaboracion);
        this.historialColaboraciones.add(nuevaColaboracion);
    }

    // Getter controlado para simular privacidad
    public String getContactoDesdeContextoAutorizado() {
        return this.correoContacto;
    }
}

class Colaboracion {
    private PersonaColaboradora colaboradora;
    private Proyecto proyecto;
    private LocalDate fechaInicio;

    public Colaboracion(PersonaColaboradora colaboradora, Proyecto proyecto) {
        this.colaboradora = colaboradora;
        this.proyecto = proyecto;
        this.fechaInicio = LocalDate.now();
    }

    public PersonaColaboradora getColaboradora() {
        return colaboradora;
    }
}
```

---

### Parte 2: Top 10 de Requerimientos Avanzados y Tentativos para la plataforma "Código a Voluntad"

Si este contexto se presentara en un examen, los docentes buscarían evaluar capacidades avanzadas de modelado, desacoplamiento y patrones de diseño utilizando escenarios similares a otros parciales del historial.

A continuación, se presenta un **Top 10 de requerimientos tentativos de diseño** que podrían tomarse para extender el sistema, detallando el desafío técnico de diseño, el patrón de diseño a aplicar y su justificación:

#### 1. Ciclo de Vida del Proyecto y Restricción de Operaciones

- **El requerimiento**: Un proyecto debe transicionar por varios estados a lo largo de su ciclo de vida (`Borrador`, `ConvocatoriaAbierta`, `EnDesarrollo`, `Pausado`, `Finalizado` o `Cancelado`). Las colaboradoras solo pueden anotarse si el proyecto está en estado `ConvocatoriaAbierta`, y los colectivos solo pueden modificar los compromisos si el proyecto es un `Borrador`.
- **Patrón a aplicar**: **State**.
- **Justificación**: En lugar de ensuciar los métodos `anotarseEn` y `modificarCompromiso` con múltiples bifurcaciones `if/else` basados en un atributo string o enum simple, delegar el comportamiento dinámico a una clase polimórfica de estados (`EstadoProyecto`), evitando la pérdida de cohesión y el acoplamiento rígido de comportamientos que cambian con el ciclo de vida.

#### 2. Planes de Colectivos y Límite de Proyectos Activos (Facturación)

- **El requerimiento**: Para evitar abusos y sustentar la plataforma, los colectivos deben adherirse a un plan de uso (`Plata`, `Bronce` u `Oro`). Cada plan establece un límite estricto de proyectos activos mensuales que el colectivo puede tener creados de forma simultánea. Si se alcanza el límite de su plan, el sistema debe impedir la creación o publicación de nuevos proyectos.
- **Patrón a aplicar**: **Strategy** (Stateful/Stateless).
- **Justificación**: Las reglas de validación de límites de proyectos pueden variar de acuerdo con el nivel del plan (por ejemplo, el plan `Bronce` restringe a 5, `Oro` es ilimitado o posee validaciones cruzadas). Al encapsular estas validaciones dentro de una jerarquía de estrategias de planes de facturación, el `Colectivo` se mantiene limpio de lógica comercial cambiante.

#### 3. Tareas Secundarias Configurables Dinámicamente ante Nuevas Colaboraciones

- **El requerimiento**: Al establecerse una colaboración (cuando una colaboradora se "anota"), se deben ejecutar de manera inmediata tareas secundarias que varían. Por ejemplo: notificar por correo electrónico al colectivo, enviar un mensaje al canal de Discord del equipo, o automatizar la creación de accesos. Estas tareas deben ser configurables globalmente o por proyecto en tiempo de ejecución.
- **Patrón a aplicar**: **Observer** (Modelado dirigido por eventos).
- **Justificación**: Evita que la clase `Proyecto` conozca los componentes de correo, integraciones de chat o infraestructura. Al notificar un evento polimórfico `ColaboracionIniciada`, un conjunto de interesados (`InteresadoNuevaColaboracion`) reaccionará ejecutando su acción correspondiente de manera totalmente desacoplada.

#### 4. Asignación de Permisos y Repositorio de Código en Sistemas Externos

- **El requerimiento**: Cuando un proyecto en desarrollo requiere de software profesional, se debe aprovisionar de manera automática un repositorio en GitHub para el equipo de desarrollo, asignando permisos de escritura a la colaboradora que se anota. El sistema debe comunicarse con las APIs del SDK de GitHub (u otros proveedores como GitLab) para cumplir con esto.
- **Patrón a aplicar**: **Adapter**.
- **Justificación**: Las firmas de las bibliotecas de terceros (`GitHubSDK`) no coinciden directamente con el modelo de dominio y son inestables. Al interponer una interfaz propia (`ProveedorControlVersiones`) y crear clases adaptadoras concretas (`GitHubAdapter`, `GitLabAdapter`), blindamos al dominio contra cambios de infraestructura y permitimos una testeabilidad unitaria impecable mediante la inyección de mocks.

#### 5. Historial de Modificaciones del Proyecto con Capacidad de Deshacer ("Undo")

- **El requerimiento**: Un colectivo puede realizar cambios importantes sobre las habilidades requeridas o el compromiso de un proyecto. Debido a posibles errores de carga, es sumamente prioritario poder navegar cronológicamente por las modificaciones pasadas de un proyecto, examinar su estado en un momento exacto del pasado, y poder deshacer o revertir los últimos \(N\) cambios aplicados.
- **Patrón a aplicar**: **Command**.
- **Justificación**: En lugar de almacenar snapshots pesados en memoria (clonar el grafo de objetos del proyecto recursivamente, lo cual es altamente ineficiente), se cosifican los cambios en objetos polimórficos (`CambioHabilidadRequerida`, `CambioCompromiso`) que implementan los métodos `aplicar(Proyecto)` y `deshacer(Proyecto)`.

#### 6. Equipos de Colaboración Compuestos (Células de Trabajo Colectivas)

- **El requerimiento**: En lugar de que una sola persona colaboradora se anote a un proyecto de forma individual, múltiples personas colaboradoras pueden organizarse previamente, crear un "Equipo" (célula de trabajo autogestionada) y anotarse juntas en un proyecto complejo. El proyecto debe poder evaluar si el equipo cuenta con las habilidades necesarias sumando las capacidades de todos sus miembros de forma transparente.
- **Patrón a aplicar**: **Composite**.
- **Justificación**: Al hacer que tanto `PersonaColaboradora` (elemento simple / hoja) como `EquipoColaboradores` (contenedor compuesto / rama) implementen una misma interfaz polimórfica (`Colaborador`), el `Proyecto` interactúa con ambos indistintamente para evaluar la disponibilidad de habilidades o para registrar la colaboración grupal.

#### 7. Algoritmo de Compatibilidad o Matching de Proyectos con Coeficientes Dinámicos

- **El requerimiento**: Para facilitar la búsqueda, el sistema debe calcular un puntaje o ranking de compatibilidad para sugerir proyectos ideales a las colaboradoras en base a: porcentaje de coincidencia de habilidades, cercanía geográfica, urgencia del proyecto y coincidencia en la modalidad de tiempo. Cada empresa o el propio sistema debe configurar la importancia de cada índice mediante coeficientes numéricos variables.
- **Patrón a aplicar**: Combinación de **Strategy** y **Composite** (con clases de parámetros dinámicos).
- **Justificación**: Se define un `CalculadorCompatibilidad` que mantiene una colección de `ParametrosDeCompatibilidad`. Cada parámetro asocia un `Coeficiente` (ponderación numérica) con una instancia polimórfica de un `IndiceDeCalculo` (`IndiceGeografico`, `IndiceHabilidades`, etc.). Esto permite armar fórmulas matemáticas complejas y personalizables en tiempo de ejecución de manera limpia y cohesiva.

#### 8. Restricciones de Disponibilidad Horaria No Solapada (Value Objects)

- **El requerimiento**: Las colaboradoras deben registrar su agenda o disponibilidad en intervalos de tiempo determinados (por ejemplo, Martes de 18:00 a 20:00). Al momento de anotarse en un proyecto que requiere una modalidad horaria sincrónica fija, el sistema debe validar dinámicamente que la colaboradora posea disponibilidad suficiente y que la misma no se solape con otras colaboraciones activas que ya haya agendado.
- **Patrón a aplicar**: **Value Object** (Modelado semántico de intervalos de tiempo sin identidad propia).
- **Justificación**: En lugar de trabajar con primitivos (como strings o fechas sueltas en las clases principales de dominio), se introduce un objeto inmutable de valor (`IntervaloDisponibilidad` o `Horario`) que encapsula la lógica matemática compleja para evaluar solapamientos, intersecciones y adiciones de tiempo de manera robusta y aislada de efectos secundarios.

#### 9. Procesos por Lotes Automáticos (Cierre por Inactividad de Convocatorias)

- **El requerimiento**: Los proyectos en estado de convocatoria que no hayan recibido nuevas colaboradoras o interacciones por parte del colectivo durante un plazo máximo de 3 meses, deben ser automáticamente pausados y archivados para no saturar la grilla pública. Este proceso se debe ejecutar de forma desatendida a la madrugada sin bloquear la navegación de los usuarios web durante el día.
- **Patrón a aplicar**: **Planificación Asincrónica** (a través de un planificador operativo/cron que invoca un ejecutable ligero).
- **Justificación**: No se debe sobrecargar al servidor web con lógica persistente de monitoreo en tiempo real. Un demonio del sistema operativo (`crontab`) ejecuta de forma planificada un proceso (`JAR` dedicado) que consume un método de consulta asíncrono en un repositorio (`proyectosInactivos()`) y les envía polimórficamente el mensaje `pausarPorInactividad()`.

#### 10. Control de Flujos de Aprobación de Candidaturas en Múltiples Pasos

- **El requerimiento**: Para proyectos sensibles, la incorporación definitiva de una colaboradora anotada requiere pasar por aprobaciones previas de la mesa directiva del colectivo. Según el proyecto, estas aprobaciones pueden realizarse de manera libre (orden indistinto de moderadores de la ONG) o de forma estrictamente secuencial y ordenada (primero el evaluador técnico, luego el director social).
- **Patrón a aplicar**: **Strategy** (o métodos de validación de secuencia polimórficos).
- **Justificación**: Se encapsula el flujo del proceso en clases polimórficas de orden (`OrdenIndistinto` u `OrdenPrefijado`). La colaboración delega la validación de turnos y las notificaciones a estas estrategias, evitando acoplar las clases de negocio a reglas jerárquicas y validaciones manuales de índices o booleanos que suelen cambiar frecuentemente en los requerimientos.

---

💬 ¿Qué te parece si tomamos uno de estos escenarios avanzados —por ejemplo, el de **Tareas Secundarias Configurables Dinámicamente (Observer)** o el de **Fórmulas de Compatibilidad con Coeficientes (Strategy/Composite)**— y elaboramos su diagrama de clases con pseudocódigo detallado para practicar el modelado?