Parte 1
- Que Clases debe ser entidades?
	- Autor
	- Revista
	- Libro
	- Biblioteca
- Que atributos pueden mapearse a columnas?
	- Todos menos los campus del enum  (y el enum)
	- List<>, almenos que se use de manera adecuada las anotaciones
- Que atributos representan las relaciones entre entidades?
	- `Biblioteca.publicaciones` es una colección de objetos de tipo `Publicacion`
	- El problema que genera `Biblioteca.publicaciones` es que la lista asi como tal no se puede persistir, sino que se se mantiene una FK en las clases hijas de Publicacion haciendo referencia a Biblioteca o por medio de una tabla intermedia, ya que es un impedance mismatch
- cardinalidad: 
- 
	- @OneToOne:  
	- @OneToMany: Entre `Biblioteca` y `Publicacion`
	- @ManyToOne: Publicacion hacia Biblioteca
	- @ManyToMany:

Parte 2


