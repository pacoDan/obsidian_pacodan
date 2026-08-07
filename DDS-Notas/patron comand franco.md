El patrón **Command** es una solución de diseño fundamental dentro del paradigma de objetos que se enfoca en la **cosificación (o reificación) del comportamiento**.

A continuación, se explica en detalle su propósito, qué problemas resuelve, sus ventajas y desventajas, y un ejemplo práctico basado en las resoluciones de diseño de los casos de estudio de la materia.

---

### ¿Qué es y a qué se debe el patrón Command?

El patrón **Command** surge ante la necesidad de **ejecutar un "cachito" de código más adelante, pero decidir en el momento actual qué es lo que se va a ejecutar después**. En lugar de ejecutar un método de manera destructiva o lineal e inmediata sobre las estructuras de datos, el comportamiento o la operación en sí misma se convierte en un **objeto con entidad propia (un objeto polimórfico)**.

Este patrón se debe principalmente a escenarios donde requerimos:

1. **Diferir operaciones en el tiempo**: Decidir ahora la acción y delegar su ejecución a un momento posterior, como en tareas programadas, operaciones asincrónicas o colas de procesamiento.
2. **Soportar reversibilidad (Hacer y Deshacer / Undo-Redo)**: Registrar de manera exacta qué cambios se hicieron para poder revertirlos de forma consistente sin alterar la identidad del modelo.

---

### ¿Qué mejora en el diseño?

- **Evita la pérdida de abstracción de la operación**: En muchos diseños deficientes, la acción en sí no existe como concepto; solo existen los datos modificados. Al cosificar la operación, esta pasa a ser un ciudadano de primer orden en el modelo.
- **Previene la ineficiencia de memoria por Snapshots masivos**: Para poder deshacer cambios, una alternativa rústica es clonar o sacar una copia entera del árbol de objetos antes de cada modificación. Esto es costoso e ineficiente en memoria. **Command** mejora esto guardando únicamente la información mínima y necesaria dentro del comando para revertir su propia acción sobre el receptor.
- **Desacoplamiento semántico**: El objeto que invoca la acción (cliente o emisor) no necesita conocer los detalles algorítmicos ni las clases concretas que realizan la modificación, interactuando únicamente con una interfaz polimórfica.

---

### Ventajas y Desventajas

#### **Ventajas:**

- **Historial de operaciones limpio**: Permite almacenar una colección de comandos ejecutados dentro del historial del objeto receptor para auditoría o para desapilar modificaciones en orden inverso.
- **Extensibilidad polimórfica (Open/Closed Principle)**: Agregar una nueva modificación o acción al sistema es tan simple como crear una nueva clase que implemente la interfaz del comando, sin necesidad de tocar o alterar las clases que coordinan o ejecutan el flujo.
- **Composición de comandos**: Permite agrupar múltiples comandos simples bajo un comando compuesto (lote o _Hit_) para que se ejecuten o deshagan como una única transacción atómica.

#### **Desventajas:**

- **Conocimiento sumamente limitado (Falta de visión global)**: Por diseño, el comando cosificado **no debe saber para qué existe en el plano general, quién controla el flujo, qué otras opciones hay en el sistema, ni qué comandos se ejecutan antes o después**. Su única responsabilidad es saber cómo aplicarse o deshacerse sobre el receptor cuando se lo indiquen. No debe utilizarse para coordinar lógicas de negocio complejas.
- **Complejidad del diagrama de clases**: Introducir una jerarquía de comandos hace que la estructura estática del modelo sea más compleja al agregar múltiples abstracciones y relaciones, lo cual no siempre se justifica si las operaciones del dominio son simples o estáticas.

---

### Ejemplo de Pseudocódigo en Funcionamiento

Tomando como base el caso de estudio **Hitbug**, modelaremos un sistema donde un contenedor de archivos multimedia (`Bag`) recibe modificaciones agrupadas (como cambiar nombre o agregar contenido) utilizando el patrón **Command**.

```
import java.util.ArrayList;
import java.util.List;

// 1. Interfaz polimórfica para los comandos (Modificaciones cosificadas)
interface Modificacion {
    void aplicarSobre(Bag bag);
    void deshacerSobre(Bag bag);
}

// 2. Comando Concreto A: Cambiar Nombre del contenedor
class CambiarNombre implements Modificacion {
    private String nombreAnterior;
    private String nuevoNombre;

    public CambiarNombre(String nuevoNombre) {
        this.nuevoNombre = nuevoNombre;
    }

    @Override
    public void aplicarSobre(Bag bag) {
        // Guardamos el estado previo únicamente para poder revertirlo (Undo)
        this.nombreAnterior = bag.getNombre();
        bag.setNombre(nuevoNombre);
    }

    @Override
    public void deshacerSobre(Bag bag) {
        bag.setNombre(nombreAnterior);
    }
}

// 3. Comando Concreto B: Agregar un Contenido multimedia
class AgregarContenido implements Modificacion {
    private String contenido;

    public AgregarContenido(String contenido) {
        this.contenido = contenido;
    }

    @Override
    public void aplicarSobre(Bag bag) {
        bag.getContenidos().add(contenido);
    }

    @Override
    public void deshacerSobre(Bag bag) {
        bag.getContenidos().remove(contenido);
    }
}

// 4. El "Hit": Un comando compuesto que agrupa múltiples modificaciones
class Hit {
    private List<Modificacion> modificaciones = new ArrayList<>();

    public void agregarModificacion(Modificacion mod) {
        this.modificaciones.add(mod);
    }

    public void aplicarSobre(Bag bag) {
        // Ejecuta todas las modificaciones polimórficamente
        this.modificaciones.forEach(mod -> mod.aplicarSobre(bag));
    }

    public void deshacerSobre(Bag bag) {
        // Para deshacer, recorremos en orden inverso para asegurar consistencia
        for (int i = modificaciones.size() - 1; i >= 0; i--) {
            modificaciones.get(i).deshacerSobre(bag);
        }
    }
}

// 5. El Receptor (Receiver): Mantiene el estado y el historial de comandos
class Bag {
    private String nombre;
    private List<String> contenidos = new ArrayList<>();
    private List<Hit> historial = new ArrayList<>(); // Guarda el historial de Hits

    public Bag(String nombre) {
        this.nombre = nombre;
    }

    public void realizarHit(Hit hit) {
        hit.aplicarSobre(this);
        this.historial.add(hit); // Agrega al historial
    }

    public void deshacerUltimoHit() {
        if (!historial.isEmpty()) {
            // Remueve el último comando del historial y lo deshace
            Hit ultimo = historial.remove(historial.size() - 1);
            ultimo.deshacerSobre(this);
        }
    }

    // Getters y Setters
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public List<String> getContenidos() { return contenidos; }
}

// 6. Demostración en un flujo de ejecución (Client / Invocador)
public class Main {
    public static void main(String[] args) {
        // Inicializamos nuestro receptor
        Bag miMusica = new Bag("Hits del Rock");

        // Creamos un lote de cambios (Hit)
        Hit primerHit = new Hit();
        primerHit.agregarModificacion(new CambiarNombre("Rock Nacional"));
        primerHit.agregarModificacion(new AgregarContenido("Ji ji ji"));
        primerHit.agregarModificacion(new AgregarContenido("Seminare"));

        // Aplicamos el comando
        System.out.println("Nombre original: " + miMusica.getNombre());
        miMusica.realizarHit(primerHit);

        System.out.println("Nuevo nombre: " + miMusica.getNombre());
        System.out.println("Canciones añadidas: " + miMusica.getContenidos());

        // Deshacemos el comando de forma transparente usando el historial
        miMusica.deshacerUltimoHit();
        System.out.println("--- Deshaciendo último Hit ---");
        System.out.println("Nombre revertido: " + miMusica.getNombre());
        System.out.println("Canciones en la lista: " + miMusica.getContenidos());
    }
}
```

---

🧩 **Sugerencia de siguiente paso**: ¿Te gustaría que analicemos cómo se mapearía la persistencia relacional en una base de datos (utilizando JPA/Hibernate) para un historial de comandos como el de `Hitbug`, abordando el desafío de guardar una lista de objetos polimórficos?