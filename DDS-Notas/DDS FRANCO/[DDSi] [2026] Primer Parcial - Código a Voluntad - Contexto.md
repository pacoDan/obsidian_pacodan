# Código a Voluntad \- Contexto

***Código a Voluntad***, será una plataforma Web, de código libre, que tiene como finalidad ser un punto de encuentro entre, por un lado, **colectivos** (fundaciones, ONGs, asambleas, comunidades, organizaciones territoriales, etc.) que necesitan software de clase profesional pero no cuentan con los recursos económicos para costearlo, y personas ***colaboradoras,*** que buscan realizar contribuciones de diseño y desarrollo de software a estas organizaciones, en forma de mano de obra y asesoramiento.

## Dominio

***Código a Voluntad*** gira en torno a 5 conceptos fundamentales: ***colectivos***, ***proyectos***, ***habilidades***, personas ***colaboradoras*** y ***colaboraciones.*** 

***Colectivos*****:** Son organizaciones que llevan adelante una determinada causa. Al inscribirse, los colectivos deben indicar: nombre, descripción, ubicación (por ahora en texto libre) y tipo de colectivo (por ahora *fundaciones*, *asociaciones barriales*, *organizaciones sociales*, *ONGs* o *asambleas*).

***Proyectos*****:** Los ***colectivos*** pueden cargar uno o más proyectos que describen acciones particulares que realizan y con las que necesitan contribuciones. Cada proyecto cuenta con: título, descripción, una lista de ***habilidades*** necesarias, el tipo de compromiso esperado (opcional; se expresa como X horas totales, X horas semanales o X horas mensuales) y la modalidad de colaboración (completamente gratuita o con un incentivo económico).

***Habilidades***: se las define a partir de un título, un código único (por ahora, el título normalizado[^1]) y una descripción. Las habilidades serán precargadas por el equipo administrativo de Código a Voluntad. 

*Personas **colaboradoras*****:** Son personas del ámbito del software (diseñadoras, desarrolladoras, arquitectas, testers, etc) que ofrecen sus conocimientos. Pueden mantenerse anónimas o registrar nombre y apellido. Las colaboradoras listan sus ***habilidades*** y registran un correo de contacto. En esta etapa, toda esta información será siempre privada. 

**Colaboraciones:** Las personas ***colaboradoras*** que participan o hayan participado de los ***proyectos*** quedarán asociadas a los mismos, construyendo así su historial en la plataforma. Cuando una ***colaboradora*** ve un proyecto de su interés, se “anota” en el mismo, estableciendo una colaboración. Al hacerlo, sus datos de contacto quedan visibles al equipo del colectivo. 

## Requerimientos iniciales

1. Poder crear ***colectivos***, ***colaboradoras, habilidades*** y ***proyectos***.  
2. Saber si una persona ***colaboradora*** puede anotarse en un proyecto, esto es, que cuente con al menos una de las habilidades requeridas.   
3. Como persona ***colaboradora,*** poder anotarse en un proyecto, estableciendo así una colaboración.  
4. Saber si un ***colectivo*** puede acceder a los datos de contacto de una ***colaboradora***. 

## 

## Ejemplos

El *Fondo de Preservación de Libros Antiguos* es una fundación dedicada a la compra y preservación de libros antes de que Antrophic los destruya[^2]. La fundación crea un colectivo homónimo, ubicado en Argentina. Luego, crea dos proyectos: “Plataforma de seguimiento de volúmenes” y “Biblioteca digital”. El primero necesita las habilidades “Desarrollo Móvil Android” y “Desarrollo Móvil iOS”; el segundo necesita la habilidad “Desarrollo Web Java” y cuenta con un incentivo económico. 

Por lado, el *Observatorio De Desalojos CABA* es una organización social que busca llevar estadísticas sobre los desalojos en la ciudad[^3], su ubicación es CABA y en su colectivo se lista el proyecto “Mapa de los desalojos”, que necesita de 2 horas semanales de “Análisis de Datos en Python”. 

Por su parte, Dani cuenta con las habilidades “Desarrollo Web Java” y “Análisis de Datos en Python”, por lo que puede anotarse a “Plataforma de seguimiento de volúmenes”. Cuando lo hace, su nombre y correo electrónico queda visible para el *Fondo de Preservación.* 

Al día siguiente Dani vuelve a entrar al sitio y también nota que puede anotarse al “Mapa de los desalojos”. Si lo hiciera, sus datos quedarían a disposición también del *Observatorio*. Sin embargo, decide no hacerlo porque no cuenta con suficiente tiempo. 

[^1]:  Por ejemplo, usando [snake case](https://es.wikipedia.org/wiki/Snake_case)

[^2]:  La organización es ficticia, [el problema es real](https://es.wired.com/articulos/como-anthropic-destruyo-millones-de-libros-de-papel-para-que-claude-aprendiera-a-escribir) 

[^3]:  Idem anterior, y es bastante urgente, como se explica [acá,](https://defensoria.org.ar/noticias/desocupaciones-y-desalojos-en-caba/) [acá](https://www.iprofesional.com/impuestos/460047-senado-vota-nuevas-reglas-alquileres-y-desalojo-expres), [acá](https://www.pagina12.com.ar/2026/08/02/la-tierra-no-se-vende-no-se-quema-ni-se-desaloja/), [acá](https://chequeado.com/el-explicador/claves-del-proyecto-de-ley-sobre-inviolabilidad-de-la-propiedad-privada-que-impulsa-el-gobierno-de-milei/), etc. 