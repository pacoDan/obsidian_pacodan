# Parcial 2 \- LivreStream \- Segunda parte

## Contexto

Tras concretar la primera iteración de LivreStream, contamos con los nuevos requerimientos de la segunda, que se describen a continuación. 

### Moderaciones

Con el objetivo de reforzar las normas de la comunidad, deseamos poder moderar las interacciones dentro de una transmisión. Para eso, se permitirá a quien administre al canal que en cualquier momento realice cualquiera de las siguientes acciones: 

* Eliminación de mensajes: se elige un mensaje del chat y se elimina  
* *Muteo* temporal: se elige a una persona participante del chat e impide que mande mensajes por los próximos N minutos.   
* *Muteo* permanente: acción similar a la anterior, pero no expira.  
* *Baneo* temporal: se elige a una persona participante del chat y se impide que mande mensajes en ese chat o cualquier otro de las transmisiones del canal por N minutos.  
* *Baneo* permanente: acción similar a la anterior, pero no expira. 

Siempre que se realice alguna de estas acciones, se deberá indicar cuál norma de la comunidad se está incumpliendo (Inicitación a la violencia, Discriminación, Contenido ilegal, etc). Además, en cualquier momento se podrán listar y deshacer estas acciones (ya sea porque se realizó por error o se arrepintió)

### Preferencias de chat

Las *personas administradoras de cada canal* pueden aplicar preferencias sobre los mensajes que pueden enviarse en una transmisión. Algunos **ejemplos** pueden ser:

* Modo *emote*, que solo permite el envío de emoticones.  
* Modo familiar, que sólo permite enviar mensajes aptos para todo público.  
* Modo idioma, que sólo permite enviar mensajes escritos en el idioma de la transmisión. 

Los mensajes que no cumplan las preferencias configuradas no deben ser publicados en el chat. 

Para resolver este requerimiento, tené en cuenta que ya contamos con un componente AnalizadorDeTexto desarrollado por Dany, colega del equipo de desarrollo.

### Componente de envío de chat

Cada vez que una persona envía un mensaje, éste debe aparecer en los *dispositivos* de todas las demás personas conectadas. Este es un requerimiento que no resolvimos en la primera iteración, pero que encararemos ahora utilizando distintas plataformas externas de chat. Si bien todas ellas exponen métodos y formatos diferentes, permiten realizar, de mínima, las siguientes operaciones: 

* publicar un mensaje en el chat  
* actualizar un mensaje ya publicado en el chat  
* eliminar un mensaje ya publicado en el chat

   
Nos interesa diseñar una solución a éste problema que sea fácil de testear y extender. 

### Terminación automática de transmisiones

Cada vez que una transmisión finaliza, se debe publicar en el chat un mensaje “*Transmisión finalizada*”. Además, para evitar que las personas usuarias permanezcan indefinidamente conectadas a la plataforma, las mismas deben durar como máximo 2 horas. Si pasado ese tiempo una transmisión aún sigue en curso, se debe terminar abruptamente.

## Nuevos requerimientos

A continuación, se detallan los nuevos requerimientos que debemos soportar

1. Como *persona* *administradora de un canal*, deseo ejecutar y deshacer acciones de moderación sobre un participante de un stream.   
2. Como *persona* *administradora de un canal,* deseo cargar mis preferencias en la transmisión en curso y que tengan efecto sobre todos los mensajes que se envían al chat.   
3. Como *persona participante de un chat*, deseo que cada vez que envíe un mensaje, todas las demás personas participantes lo reciban en sus computadoras.   
4. Como parte *del comité de cuidados de* *LivreStream* deseo que las transmisiones terminen siempre a las 2 horas. 

Comunicá una solución usando prosa, código o pseudocódigo y diagramas UML (como mínimo un diagrama de clases y uno de despliegue) que resuelva cada uno de los nuevos requerimientos y los requerimientos originales.

