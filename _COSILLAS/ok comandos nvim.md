En LazyVim, dentro de la lista de archivos recientes (que se abre con **dos veces espacio**), puedes eliminar archivos rápidamente usando la tecla **`d`** para eliminarlos de la lista de recientes.
## Comandos clave en LazyVim para gestionar archivos:

|Acción|Comando|
|---|---|
|Eliminar de recientes|**`d`** (en la lista de archivos recientes) [[gist.github](https://gist.github.com/rwietter/7fbf550dda73b209d659a7cd380d35f5)]|
|Eliminar archivo del sistema|**`d`** en el file explorer (`<leader>` + `e`) [[dev](https://dev.to/memlenz/lazyvim-adventure-part-2-managing-projects-like-a-boss-3125)]|
|Renombrar archivo|**`r`** en el file explorer [[dev](https://dev.to/memlenz/lazyvim-adventure-part-2-managing-projects-like-a-boss-3125)]|
|Crear archivo/carpeta|**`a`** en el file explorer [[dev](https://dev.to/memlenz/lazyvim-adventure-part-2-managing-projects-like-a-boss-3125)]|
|Ver buffers abiertos|`<leader>` + `b` [[dev](https://dev.to/memlenz/lazyvim-adventure-part-2-managing-projects-like-a-boss-3125)]|
|Close buffer|`<leader>` + `c` [[dev](https://dev.to/memlenz/lazyvim-adventure-part-2-managing-projects-like-a-boss-3125)]|


==para copiar es precionar "c" una vez seleccionado pero aun asi no pega bien== 

**Nota importante**: La tecla `d` en la lista de archivos recientes solo **elimina el archivo de la lista de historial**, no borra el archivo del sistema. Para eliminar físicamente un archivo del sistema, usa el file explorer con `<space>` + `e` y luego `d`.

Si necesitas una función que borre físicamente el archivo desde la lista de recientes, podrías configurar un mapping personalizado en tu `lua/config/lazyvim.lua`.