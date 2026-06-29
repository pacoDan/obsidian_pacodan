diagrama de Arq.:
```plantuml
@startuml
node "Frontend Cliente Web / Mobile" as Frontend
cloud "Internet" as Medio
node "Microservicio Donaciones" as Donaciones {
    component "Donaciones" as DonacionesAPI
    component "Notificaciones" as Notificador
    database "DB Donaciones" as DBDonaciones
}

node "Microservicio Logística" as Logistica {
    component "Logística" as LogisticaAPI
    component "Planificador\nde Entregas" as Planificador
    component "Tracking\nTiempo Real" as Tracking
    database "DB Logística" as DBLogistica
    'component "AsignadorDeDonacionesAlgoritmos" as Asignador
}
component "Proveedor Externo\nPlanificación de Rutas" as Externo


DonacionesAPI --> DBDonaciones
LogisticaAPI --> DBLogistica
DonacionesAPI --> LogisticaAPI : API REST
'DonacionesAPI --> Asignador
LogisticaAPI --> Planificador
Planificador --> Externo : API REST, Solicitud lote <=100
Externo --> LogisticaAPI : API REST
Tracking --> Frontend : GraphQL

Frontend --> DonacionesAPI : API REST
Frontend --> LogisticaAPI : API REST

Notificador --> Frontend : Email/SMS/App

actor "Donador/Beneficiario/Administradores/Repartidor" as usuario
usuario --> Medio
Medio --> Frontend
@enduml
```


los algoritmos van donde **Logística**, de alguna manera se hara con un crontask/ de manera periódica ese proceso, pero a la vez hay que recibir las donaciones pero partiendo de las que estan en deposito, no desde cuando el usuario las elige donar.. solo desde cuando estan en reposo en deposito