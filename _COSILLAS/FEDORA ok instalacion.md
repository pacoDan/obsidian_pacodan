conexion por rdp (por default) remota PUERTO: 3389 
instalación
```sh
sudo dnf check-update && sudo dnf upgrade --refresh && sudo dnf autoremove
sudo dnf install -y \  
zsh gcc make cmake unzip fastfetch screenfetch gettext gdb tree git curl wget vim openssh maven ulauncher cheese\  
autoconf automake libtool docker-buildx tree-sitter docker-cli docker-compose\  
xsel wl-clipboard ripgrep fd-find  htop plank flameshot gpaste nemo timeshift redshift tilix terminator vlc nvim ufw parole gedit xed

sudo systemctl enable sshd
sudo systemctl restart sshd
```
flatpak
```sh
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo  
  
flatpak install -y flathub \  
md.obsidian.Obsidian \  
com.dropbox.Client \  
com.opera.Opera
com.visualstudio.code
```

```sh
sudo dnf copr enable -y dejan/lazygit
sudo dnf install -y lazygit

wget https://corretto.aws/downloads/latest/amazon-corretto-17-x64-linux-jdk.rpm
chmod +x *.rpm # amazon-corretto-17-x64-linux-jdk.rpm
sudo dnf install -y ./amazon-corretto-17-x64-linux-jdk.rpm
#sudo dnf install -y ./google-chrome-stable_current_x86_64.rpm
mkdir -p PROYECTOS && cd PROYECTOS
git clone https://github.com/vinceliuice/WhiteSur-wallpapers.git
./WhiteSur-wallpapers/install-wallpapers.sh 


git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git
./WhiteSur-icon-theme/install.sh
git clone https://github.com/vinceliuice/WhiteSur-kde.git
./WhiteSur-kde/install.sh
git clone https://github.com/vinceliuice/MacTahoe-kde.git
./MacTahoe-kde/install.sh
git clone https://github.com/vinceliuice/MacSonoma-kde.git
./MacSonoma-kde/install.sh
git clone https://github.com/dracula/terminator.git
./terminator/install.sh

git clone https://github.com/vinceliuice/Mojave-gtk-theme.git
./Mojave-gtk-theme/install.sh
git clone https://github.com/vinceliuice/MacTahoe-icon-theme.git
./MacTahoe-icon-theme/install.sh 
git clone https://github.com/vinceliuice/MacTahoe-gtk-theme.git
./MacTahoe-gtk-theme/install.sh 
git clone https://github.com/pacoDan/install-personalizacion.git
./install-personalizacion/install.sh
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git
./WhiteSur-gtk-theme/install.sh
```

```sh
git clone https://github.com/pacodan/nvim ~/.config/nvim
nvim --headless "+Lazy! sync" +qa # Lazy.nvim 
```
csv a md: 
```sh
pip install csv2md
csv2md "2026.1 - Sysarmy - Encuesta de remuneración salarial Argentina - Sysarmy - sueldos - 2026.01CLEAN.csv" > sueldos.md   
```
https://github.com/lzakharov/csv2md



----


¡Perfecto! Estás usando **plasmalogin** (el nuevo login manager de KDE Plasma 6 en Fedora). Aquí está la solución específica:

## Configurar autologin con **plasmalogin**

### Método 1: Archivo de configuración directo
```bash
sudo nano /etc/plasmalogin/plasmalogin.conf
```

**Agrega o edita esta sección:**
```ini
[Autologin]
User=daniel
Session=plasma.desktop
```

### Método 2: Comando automatizado (copia y pega):
```bash
# Crear el archivo de configuración
sudo mkdir -p /etc/plasmalogin
cat > /tmp/plasmalogin.conf << EOF
[Autologin]
User=daniel
Session=plasma.desktop
EOF

sudo cp /tmp/plasmalogin.conf /etc/plasmalogin/plasmalogin.conf
sudo rm /tmp/plasmalogin.conf
```

### Método 3: Línea única (más rápido):
```bash
sudo mkdir -p /etc/plasmalogin && echo -e "[Autologin]\nUser=daniel\nSession=plasma.desktop" | sudo tee /etc/plasmalogin/plasmalogin.conf
```

## Aplicar los cambios:
```bash
sudo systemctl restart plasmalogin
```

## Verificar la configuración:
```bash
cat /etc/plasmalogin/plasmalogin.conf
```

## Para desactivar (revertir):
```bash
sudo rm /etc/plasmalogin/plasmalogin.conf
sudo systemctl restart plasmalogin
```

## ✅ **Resultado:**
```
[Autologin]
User=daniel
Session=plasma.desktop
```

## **Reinicia tu PC para probar:**
```bash
reboot
```

**¡Ahora sí funcionará perfectamente en tu Fedora 44 con KDE Plasma 6 y plasmalogin!** 🎉

El próximo reinicio iniciará directamente tu sesión de `daniel` sin pedir contraseña.