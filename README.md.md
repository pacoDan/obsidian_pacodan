Habilitar nombres largos en Windows
```sh
git config core.longpaths true
```
para ver mas detalles de cualquier comando git:
```sh
# Modo verbose
git add -v .

# Ver el estado detallado
git status --porcelain

# Ver configuración actual
git config --list
```



`Android_Bluetooth5`   es jhonpaco@frbalutn.edu.ar
`Android_Bluetooth6`   es rextrem.transfers@gmail.com
`Android_Bluetooth7`   es crosariopaco@gmail.com
`Android_Bluetooth8`   es cinthiaolmedo24@gmail.com cintiab256

correr:
```sh
emulator -avd Android_Bluetooth5 -packet-streamer-endpoint default
```
```sh
adb shell svc bluetooth enable 
```