editar y agregar al archivo las lineas:
```sh
sudo nano /etc/lightdm/lightdm.conf
```


estas lineas:
```conf
[Seat:*]
autologin-user=daniel
autologin-user-timeout=0
greeter-session=lightdm-gtk-greeter
user-session=cinnamon
session-wrapper=/etc/lightdm/Xsession
```

después ultimo esto:
```sh
sudo groupadd -r autologin
sudo gpasswd -a daniel autologin





sudo reboot
```