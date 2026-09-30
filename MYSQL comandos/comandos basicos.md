para establecer contraseñas
se ingresa a la db 
==mysql -u root -p==
```sh
ALTER USER 'root'@'localhost'
IDENTIFIED BY '1234';
FLUSH PRIVILEGES;
```



