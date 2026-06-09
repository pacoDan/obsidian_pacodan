primero de desinstala el `xfce4-screenshooter`:
```sh
sudo pacman -R xfce4-screenshooter
```
despues de instaala `gnome-screenshot`

```sh
sudo pacman -S gnome-screenshot --noconfirm
```

Para capturar toda la pantalla (Tecla Impr Pant):
```sh
xfconf-query -c xfce4-keyboard-shortcuts -p /commands/custom/Print -n -t string -s "gnome-screenshot"
```
Para seleccionar un área (Mayús + Impr Pant):
```sh
xfconf-query -c xfce4-keyboard-shortcuts -p "/commands/custom/<Shift>Print" -n -t string -s "gnome-screenshot -a"
```
Para capturar la ventana activa (Alt + Impr Pant):
```sh
xfconf-query -c xfce4-keyboard-shortcuts -p "/commands/custom/<Alt>Print" -n -t string -s "gnome-screenshot -w"

```