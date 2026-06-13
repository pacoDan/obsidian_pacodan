```c
const int LED_BUILTIN = 4;
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  delay(1000);                      // wait for a second
  digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  delay(1000);                      // wait for a second
}
```

https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/windows-setup.html#get-started-windows-first-steps:~:text=for%20more%20information.-,Build%20the%20Project,%EF%83%81,-Build%20the%20project

instalación en arch:
```sh
sudo pacman -S --noconfirm --needed gcc git make flex bison gperf python-pip cmake ninja ccache dfu-util

# 2. Download ESP-IDF
mkdir -p ~/esp 
cd ~/esp 
git clone --recursive https://github.com/espressif/esp-idf.git

# 3. Install Toolchains and Set Environment Variables
cd ~/esp-idf ./install.sh esp32

. ./export.sh

alias get_idf='. $HOME/esp/esp-idf/export.sh'
```
link de espressif:
https://github.com/espressif
bibliotecas en teoria:
https://github.com/espressif/esp32-arduino-lib-builder

sample project: ok
```sh
cd ~/esp
cp -r $IDF_PATH/examples/get-started/blink .
cd blink
```




----


habilitacion del USB :

```sh
# Verificar detección
lsusb
ls -l /dev/ttyUSB*

# Agregar permisos (una sola vez)
sudo usermod -aG uucp $USER

# CERRAR SESIÓN Y VOLVER A ENTRAR (obligatorio)

# Verificar grupos
groups

# Flashear
idf.py -p /dev/ttyUSB0 flash
```

