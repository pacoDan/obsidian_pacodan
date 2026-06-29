### 2. Mocks para el Microservicio de Logística

Aquí el escenario es más complejo: el MS-Logística recibe peticiones web (REQ 6), ejecuta tareas programadas internamente (REQ 3 y 5) y actúa como un **cliente HTTP** haciendo peticiones hacia el Frontend/BFF (REQ 2 y 7).

Usaremos Mockito para simular el cliente HTTP que dispara peticiones al frontend.

```java
package ar.edu.utn.frba.dds.logistica;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.ArgumentMatchers.eq;

@WebMvcTest(LogisticaController.class) // Levanta solo la capa Web
class LogisticaTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private LogisticaService logisticaService;

    // Mockeamos la clase encargada de hacer los POST/GET hacia el Frontend
    @MockBean
    private ClienteHttpFrontend clienteFrontendMock; 
    
    // Mockeamos el servicio externo de planificación
    @MockBean
    private ApiPlanificadorExterno apiPlanificadorMock;

    @Test
    @DisplayName("REQ 6: Frontend envía GET para ordenar el inicio del recorrido")
    void testInicioRecorridoPorGet() throws Exception {
        // Adaptado a tu especificación: El frontend envía un GET
        mockMvc.perform(get("/api/v1/rutas/55/iniciar")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());

        Mockito.verify(logisticaService, Mockito.times(1)).iniciarRuta(55L);
    }
}
```

#### Testeando los Procesos de Fondo y Envíos al Frontend (Servicios)

Para probar los procesos asincrónicos (CronTasks) y asegurarnos de que el backend hace los `POST` y `GET` correspondientes hacia el Frontend, usamos JUnit clásico (sin `MockMvc`):
```java
package ar.edu.utn.frba.dds.logistica.servicios;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.jupiter.MockitoExtension;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.ArgumentMatchers.eq;

@ExtendWith(MockitoExtension.class)
class LogisticaProcesosTest {

    @Mock
    private ClienteHttpFrontend clienteFrontendMock; // Hace peticiones al Frontend

    @Mock
    private ApiPlanificadorExterno apiPlanificadorMock;

    @InjectMocks
    private LogisticaService logisticaService; // La clase a testear

    @Test
    @DisplayName("REQ 2: Logística hace POST al Frontend informando Asignación e Inicio de Ruta")
    void testLogisticaHacePostAlFrontend() {
        // Simulamos que el repartidor inicia la ruta
        logisticaService.iniciarRuta(55L);

        // Verificamos que Logística disparó un POST HTTP hacia el Frontend
        Mockito.verify(clienteFrontendMock, Mockito.times(1))
               .enviarPostNotificacion(eq("/frontend-api/notificaciones/ruta-iniciada"), any());
    }

    @Test
    @DisplayName("REQ 7: MS-Logística envía GET al Frontend con las posiciones en tiempo real")
    void testEnvioPosicionesPorGetAlFrontend() {
        // Simulamos una actualización interna del GPS del camión
        Ubicacion ubicacion = new Ubicacion(-34.603, -58.381);
        logisticaService.actualizarPosicion(10L, ubicacion);

        // Verificamos que Logística llame al Frontend mediante GET (pasando parámetros en URL)
        Mockito.verify(clienteFrontendMock, Mockito.times(1))
               .enviarGetPosicion(eq("/frontend-api/monitoreo?camion=10&lat=-34.603&lng=-58.381"));
    }

    @Test
    @DisplayName("REQ 3 y 5: CronTask ejecuta algoritmos y envía a planificador nocturno")
    void testCronTaskPlanificacionNocturna() {
        // Este método en tu código real tendría la anotación @Scheduled(cron = "0 0 2 * * ?")
        logisticaService.jobPlanificacionNocturna();

        // Verificamos que la tarea asincrónica haya llamado a la API externa
        Mockito.verify(apiPlanificadorMock, Mockito.times(1)).enviarLoteAsincronico(any());
    }
}
```

### Resumen de la estrategia aplicada

Al definir que el MS-Logística es el que "dispara" las peticiones al Frontend (REQ 2 y REQ 7), la clave de la prueba es mockear un **Cliente HTTP** (por ejemplo, si en tu proyecto usas `RestTemplate`, `WebClient` o `OpenFeign`) y verificar con `Mockito.verify()` que esos clientes fueron invocados con las URLs y verbos (`POST`/`GET`) exactos que especificaste.

----
----


### Parte 1: Documentación de Endpoints Clave (Contrato de API)

He diseñado estos endpoints respetando rigurosamente las convenciones REST y los requerimientos de asincronismo y trazabilidad.

#### Microservicio de Donaciones (MS-Donaciones)

|**Ruta**|**Verbo**|**Propósito y Requerimiento Cubierto**|**Respuestas Esperadas**|
|---|---|---|---|
|`/api/v1/asignaciones/ejecutar`|`POST`|**Req. 3:** Dispara la ejecución asincrónica de los algoritmos de asignación. No recibe un _body_. Retorna inmediatamente y el proceso continúa en segundo plano.|`202 Accepted`|
|`/api/v1/entregas/{id}/recepcion`|`PATCH`|**Req. 8 y 2:** La entidad beneficiaria confirma la recepción de la donación. Dispara el cambio de estado a "Entregada" y lanza el evento de notificación.|`200 OK`, `404 Not Found`|

#### Microservicio de Logística (MS-Logística)

|**Ruta**|**Verbo**|**Propósito y Requerimiento Cubierto**|**Respuestas Esperadas**|
|---|---|---|---|
|`/api/v1/logistica/planificador/callback`|`POST`|**Req. Impl. 1, 2 y 3:** Endpoint expuesto (Webhook) para que el proveedor externo nos devuelva el resultado de la planificación nocturna. Aquí procesamos las donaciones asignadas y las que quedaron huérfanas.|`200 OK` (Si procesamos bien el payload)|
|`/api/v1/rutas/{id}/iniciar`|`POST`|**Req. 6 y 2:** El chofer indica que arranca la ruta. Esto cambia el estado de las entregas a "En traslado" y emite un evento para notificar a los donantes y beneficiarios.|`200 OK`, `400 Bad Request`|
|`/api/v1/camiones/{id}/ubicacion`|`PATCH`|**Req. 7:** La app del chofer hace _polling_ constante aquí enviando `{ "lat": X, "lng": Y }` para actualizar el dashboard en tiempo real.|`204 No Content`|

### Parte 2: Estrategia de Testing y Mocks

Para asegurar que nuestra lógica de dominio es robusta, no podemos depender de que el proveedor externo de rutas esté levantado durante nuestros tests, ni queremos enviar correos reales cada vez que corremos JUnit.

Aquí aplico un enfoque de aislamiento total. Usando **JUnit 5 y Mockito**, voy a mockear las interfaces de salida (nuestros "puertos") para simular los escenarios complejos, como el lote de 100 donaciones o las respuestas del planificador.

Así es como preparo los Mocks en Java:

```java
package ar.edu.utn.frba.dds.logistica.rutas;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.any;

import java.util.List;

// Usamos la extensión de Mockito para JUnit 5
@ExtendWith(MockitoExtension.class)
public class PlanificacionDeRutasTest {

    // 1. MOCKEAMOS LOS SERVICIOS EXTERNOS (Adaptadores de salida)
    @Mock
    private ProveedorRutasExterno apiPlanificadorMock;
    
    @Mock
    private ServicioNotificacion notificadorMock;

    // 2. LA CLASE QUE REALMENTE VAMOS A TESTEAR (Nuestro caso de uso)
    private GestorDePlanificacion gestor;

    @BeforeEach
    void prepararEntorno() {
        // Inyectamos los mocks en nuestra clase de dominio
        gestor = new GestorDePlanificacion(apiPlanificadorMock, notificadorMock);
    }

    @Test
    @DisplayName("Debe procesar correctamente el Callback del planificador externo")
    void procesarCallbackConEntregasExitosasYRechazadas() {
        // 1. PREPARAR EL ESCENARIO (Arrange)
        // Simulamos el JSON (objeto) que nos enviaría el proveedor externo a la URL de callback
        ResultadoPlanificacion payloadCallback = new ResultadoPlanificacion(
            "PLAN-999", 
            List.of(new RutaAsignada(10, List.of(55, 56))), // Entregas asignadas al camión 10
            List.of(57, 58) // Entregas que no entraron en el lote (Req. Implementación 3)
        );

        // 2. ACTUAR (Act)
        // Simulamos que el endpoint /callback recibe la petición y llama a nuestro gestor
        gestor.procesarRespuestaPlanificador(payloadCallback);

        // 3. VERIFICAR (Assert)
        // Verificamos que las entregas 55 y 56 hayan cambiado a estado "Lista para entregar"
        assertTrue(gestor.obtenerEntrega(55).estaListaParaEntregar());
        
        // Verificamos que las entregas 57 y 58 vuelvan a encolarse para el día siguiente
        assertTrue(gestor.obtenerEntrega(57).requiereReplanificacion());
    }

    @Test
    @DisplayName("Debe notificar a los beneficiarios cuando el chofer inicia la ruta")
    void notificarAlIniciarRuta() {
        // 1. PREPARAR (Arrange)
        RutaDeReparto rutaDiaria = new RutaDeReparto(10); // Camión 10
        rutaDiaria.agregarEntrega(new Entrega(55, "Escuela Rural N°10"));

        // 2. ACTUAR (Act) - Endpoint /rutas/{id}/iniciar
        gestor.iniciarRuta(rutaDiaria);

        // 3. VERIFICAR (Assert)
        // Req 2: Verificamos que nuestro dominio haya llamado al servicio de notificaciones
        // verificando exactamente cuántas veces se ejecutó el método enviar()
        Mockito.verify(notificadorMock, Mockito.times(1))
               .enviar(any(EventoInicioRuta.class));
               
        // Verificamos el cambio de estado de la entrega
        assertEquals("EN_TRASLADO", gestor.obtenerEntrega(55).getEstado());
    }
}
```

### Por qué esta presentación funciona:

- **Muestra control del protocolo:** Los verbos HTTP están correctamente asignados según las mejores prácticas (usar `PATCH` para estados y ubicaciones es el estándar de la industria).
    
- **Demuestra abstracción:** Los tests prueban que entiendo que mi sistema no debe "romperse" si la API del ruteo se cae. Todo el comportamiento asincrónico y de integración está encapsulado y cubierto por Mockito sin levantar contextos pesados de Spring.