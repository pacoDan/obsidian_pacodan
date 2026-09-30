Sí. Si quieres eliminar Plank por completo, incluyendo configuración, caché y archivos residuales, como si nunca hubiera estado instalado, puedes hacer:

sudo pacman -Rns plank


Después, elimina los archivos de usuario que pacman no gestiona:

rm -rf ~/.config/plank
rm -rf ~/.cache/plank
rm -rf ~/.local/share/plank


Y para buscar posibles restos:

find ~/.config ~/.cache ~/.local/share -iname '*plank*' 2>/dev/null


Si quieres hacer una limpieza más agresiva de todos los archivos relacionados con Plank en tu usuario, puedes usar:

find ~ -iname '*plank*' -print


Revisa primero los resultados antes de borrarlos.

Importante

pacman -Rns plank elimina el paquete y las dependencias que fueron instaladas como dependencias y que ya no son necesarias, pero no necesariamente borra configuraciones creadas en tu directorio personal. Por eso hacen falta los rm -rf anteriores.

Si además quieres quitar cualquier autoinicio de Plank (por ejemplo, que siga intentando arrancar al iniciar sesión), revisa:

grep -Ril plank ~/.config/autostart ~/.config 2>/dev/null


No recomiendo ejecutar rm -rf sobre resultados de find automáticamente sin revisarlos.