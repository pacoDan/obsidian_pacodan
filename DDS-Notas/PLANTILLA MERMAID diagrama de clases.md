caso de uso plantUML:
```plantuml
left to right direction
skinparam packageStyle rectangle
actor customer
actor clerk
rectangle checkout_ {
  customer -- (checkout)
  (checkout) .> (payment) : include
  (help) .> (checkout) : extends
  (checkout) -- clerk
}
```


diagrama de clases mermaid ejemplo
```mermaid
classDiagram
class Animal {
+String name
+int age
+makeSound()
}
class Dog {
+String breed
+fetch()
+bark()
}
class Cat {
+bool isIndoor
+meow()
+purr()
}
Animal <|-- Dog
Animal <|-- Cat
Dog ..> Toy : plays with
class Toy {
+String type
}
```
casos de uso en plantUML ejemplo
```plantuml
@startuml
left to right direction
actor Guest as g
package Professional {
  actor Chef as c
  actor "Food Critic" as fc
}
package Restaurant {
  usecase "Eat Food" as UC1
  usecase "Pay for Food" as UC2
  usecase "Drink" as UC3
  usecase "Review" as UC4
}
fc --> UC4
g --> UC1
g --> UC2
g --> UC3


User -> (Start)
User --> (Use the application) : A small label

:Main Admin: ---> (Use the application) : This is\nyet another\nlabel


@enduml
```



