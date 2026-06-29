uso de tests con junit

Sin embargo, asumiendo que estás trabajando con entornos de desarrollo modernos (como Java 17), el estándar actual es utilizar **JUnit 5 (Jupiter)**. En esta versión, las anotaciones cambiaron de nombre para ser mucho más explícitas sobre _cuándo_ se ejecutan exactamente.

Aquí tienes un resumen rápido del ciclo de vida en JUnit 5:

- **`@BeforeEach`** (El antiguo `@Before`): Se ejecuta **antes de cada test**. Es el lugar ideal para instanciar tus objetos principales y asegurar que cada prueba empiece con un estado limpio y fresco.
    
- **`@BeforeAll`** (El antiguo `@BeforeClass`): Se ejecuta **una sola vez** antes de todos los tests de la clase. El método debe ser estático (`static`). Es ideal para operaciones pesadas o globales, como levantar una conexión a base de datos.
    
- **`@AfterEach`** (El antiguo `@After`): Se ejecuta **después de cada test**. Útil para limpiar archivos temporales, cerrar buffers o resetear mocks.
    
- **`@AfterAll`** (El antiguo `@AfterClass`): Se ejecuta **una sola vez** al final de todo. El método debe ser estático. Ideal para cerrar las conexiones globales que abriste al principio.
    

### Ejemplo Práctico en JUnit 5

Para ilustrarlo, imaginemos que estás testeando un `ClipboardManager` encargado de gestionar un historial de recortes de texto. Así es como estructurarías la preparación del entorno:

Java

```
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class ClipboardManagerTest {

    private ClipboardManager manager;
    private static DatabaseConnection dbConnection;

    // 1. PREPARACIÓN GLOBAL (Se ejecuta UNA vez al inicio)
    @BeforeAll
    static void setUpAll() {
        // Simulamos inicializar un recurso pesado
        dbConnection = new DatabaseConnection();
        dbConnection.connect();
        System.out.println("Conexión global establecida.");
    }

    // 2. PREPARACIÓN LOCAL (Se ejecuta antes de CADA @Test)
    @BeforeEach
    void setUp() {
        // Instanciamos el objeto de cero para que los tests sean 100% independientes
        manager = new ClipboardManager(dbConnection);

        // Podemos setear los primeros objetos o un estado base
        manager.addPaste("Texto inicial por defecto");
    }

    // 3. LOS TESTS
    @Test
    @DisplayName("Debe agregar un nuevo recorte al historial")
    void testAddPaste() {
        // Act (Actuar)
        manager.addPaste("Copia de seguridad");

        // Assert (Comprobar)
        assertEquals(2, manager.getPastesCount(), "Debería haber 2 recortes en total");
        assertTrue(manager.contains("Copia de seguridad"));
    }

    @Test
    @DisplayName("Debe vaciar todo el historial correctamente")
    void testClearPastes() {
        manager.clear();

        assertEquals(0, manager.getPastesCount());
        assertTrue(manager.isEmpty());
    }

    // 4. LIMPIEZA LOCAL (Se ejecuta después de CADA @Test)
    @AfterEach
    void tearDown() {
        // Limpiamos la memoria o el estado local si fuera necesario
        manager.clear();
    }

    // 5. LIMPIEZA GLOBAL (Se ejecuta UNA vez al final)
    @AfterAll
    static void tearDownAll() {
        dbConnection.disconnect();
        System.out.println("Conexión global cerrada.");
    }
}
```

### Consejos clave para una buena arquitectura de tests:

- **Independencia:** La regla de oro es que el orden en el que se ejecutan los `@Test` no debe importar. Usa `@BeforeEach` rigurosamente para garantizar que un test no contamine los datos que necesita el siguiente.
    
- **Nombres descriptivos:** Notas el uso de `@DisplayName("...")`? Es una excelente práctica en JUnit 5 para describir qué hace la prueba en lenguaje natural, lo que facilita muchísimo la lectura de los reportes cuando un test falla.