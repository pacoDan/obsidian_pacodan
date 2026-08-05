diagrama:

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