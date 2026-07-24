VARIABLES DE ENTORNO:
```sh
export PATH="$PATH:/home/daniel/Android/Sdk/platform-tools:/home/daniel/Android/Sdk/emulator:/home/daniel/Android/Sdk/tools"
```
```sh
export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools
```

el comando emulator
presente en la carpeta `/c/Users/Daniel/AppData/Local/Android/Sdk/emulator/emulator`
para listar  las maquinas virtuales
```sh
emulator -list-avds
```
para correr una maquina virtual:
```sh
emulator -avd Pixel_2_API_30
```


ADB
Ejecuta los siguientes comandos para reiniciar el servidor ADB:
```sh
adb kill-server
adb start-server
```

para buildear hacia la play store, pero debe estar ene l sitio de expo
https://docs.expo.dev/develop/tools/ 
```sh
eas build -p android
```
 no por npx, por que:
```
╰─❯ npx expo build:android
  $ expo build:android is not supported in the local CLI, please use eas build -p android instead
```
para correrlo en android:
```sh
npx expo run:android
```




---


npx eas credentials --platform android

