Comprendo perfectamente lo que te ocurre. Al aumentar el porcentaje de velocidad, el emulador simplemente fuerza el motor gráfico del juego a correr en "cámara rápida", lo cual hace que sea imposible jugar.

Para aumentar los FPS de forma real manteniendo la velocidad del juego normal (sin cámara rápida), no se puede hacer modificando los menús generales de PCSX2. Tienes que parchear el motor interno del juego.

Aquí tienes el método correcto paso a paso que utiliza la comunidad:

## Paso 1: Habilitar los Parches de Códigos (Cheats/Patches) en PCSX2

Primero debemos indicarle al emulador que acepte códigos externos que alteran el comportamiento del juego.

1. Abre PCSX2.
2. Ve a Ajustes (Settings) > Emulación (Emulation).
3. Asegúrate de marcar la casilla que dice Habilitar Códigos / Parches (Enable Cheats / Patches).

## Paso 2: Conseguir el Parche de 60 FPS o FPS Desbloqueados

Cada juego requiere un código único porque sus programadores originales diseñaron el motor de forma distinta.

1. Abre tu juego en PCSX2.
2. Mira el título de la ventana del emulador o ve a las propiedades del juego para ver su CRC (un código de letras y números de 8 caracteres, por ejemplo: `A1B2C3D4`) y su Código de Región (ejemplo: `SLUS-21601`).
3. Ve a Google y busca: _"Nombre de tu juego" 60fps patch PCSX2 CRC [coloca tu código]_ o busca en los foros oficiales de PCSX2 en la sección "60fps codes".

## Paso 3: Instalar el código en el emulador (Versión PCSX2 2.0+)

Si usas la versión moderna del emulador, es extremadamente sencillo:

1. Haz clic derecho sobre el juego en tu lista de la biblioteca de PCSX2 y selecciona Propiedades (Properties).
2. En la barra lateral izquierda, dirígete a la pestaña Códigos / Parches (Cheats o Patches).
3. Haz clic en el botón Añadir / Crear o pega el código de texto directamente en la caja de trucos del emulador.
4. Guarda los cambios y ejecuta el juego.

Si el código es el correcto para tu versión exacta del juego, el contador de FPS subirá (o se estabilizará si el juego original iba a 30 FPS) pero los personajes y el tiempo del juego se moverán a la velocidad de la vida real.

¿Cuál es el nombre exacto del juego que quieres jugar? Si me lo dices, puedo buscarte directamente el código o parche de FPS correcto para que no tengas que buscarlo tú.