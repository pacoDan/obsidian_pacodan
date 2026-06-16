A)
manejo de cookies

diagrama de Arq.:
```plantuml
node Cliente

node Servidor{
	component Frontend
}
Cliente -- Frontend : HTTPS

Node ServidorAPIGateway{
	component "API Gateway"
}
Node ServerBack1{
	component "Servicio 1"
}
Node ServerBack2{
	component "Servicio 2"
}




 
```