diagrama de clases:
```plantuml
class Curso{
crearGrupo():Grupo

} 
class Grupo{
}
Curso -> Grupo

class Entrega{
	Asignacion asignacion
}
Asignacion <|--- TrabajoPractico
Asignacion <|--- ActividadDeClase
```


pseudocódigo:
```cpp
#Curso>>Grupo crearGrupo(){
	return this.gruposDeTrabajo.crear(this.cantidadDeGrupos(),this.tamanioIdeal())
}
#Curso>>List<Grupo> crearGruposDeCurso(){ // M, N podrian ser recibidos como parametro
	this.grupos= new List<>();
	for(i=0;1<this.cantidadDeGrupos();i++){
		grupos.add(new Grupo(this.tamañoIdeal()))
	}
}

#Curso>>inscribir(Estudiante estudiante){ // recibe grupo , el alumno elije?
	this.grupo=this.buscarGrupoAbierto(estudiante);
	this.grupo.inscribir(estudiante)
}
#Curso>>darDeBaja(Estudiante estudiante){
	this.grupo=this.buscarGrupo(estudiante);
	this.grupo.inscribir(estudiante)
	
req 3

#Grupo>>aniadirSuscriptorBajas(Estudiante alumno){
	this.bajas.aniadirAlumno(estudiante)
}


REQ 4

#Grupo>>cerrar(){
	this.repo= new Repositorio();
	this.estudiantes.recibirAcceso(repo);
}
REQ 6
#Curso>>darDeBaja(Estudiante estudiante){
	this.grupo=this.buscarGrupo(estudiante);
	this.grupo.inscribir(estudiante)
	
	
	
REQ 11
#Asignacion>>crear(Grupo grupo, int cantidadDeEntregas){
	this.grupoCorrespondiente=grupo;
	this.entregas=new List<Entrega>();// siete new Entrega
	grupo.notificarAlumnosAsignacion();
}
#Grupo>>notificarAlumnosAsignacion(){
	this.medioDeNotificacion().notificar(this.obtenerMails(), "Se ha asignado" );
}
#MedioDeNotificacion -¬°°!!>
#MedioMail>>notificar(mails, String s){
	this.mailSender.notificarEntrega(mails,s)
}


```

