Para levantar un proyecto **WordPress** desde el código fuente en PHP (similar a `npm install && npm start`), sigue estos pasos:

## 🛠️ **Requisitos previos**
```bash
# Instala PHP (8.0+ recomendado)
# Instala MySQL/MariaDB
# Instala un web server (Apache/Nginx)
# O usa Docker/XAMPP/Laragon/MAMP
```

## 🚀 **Método 1: Servidor local con PHP integrado (Más simple)**

```bash
# 1. Navega a la carpeta del proyecto WordPress
cd mi-proyecto-wordpress/

# 2. Levanta el servidor PHP integrado
php -S localhost:8000

# 3. Abre http://localhost:8000 en tu navegador
```

## 🛠️ **Método 2: Con Composer (Recomendado para desarrollo)**

```bash
# 1. Instala dependencias
composer install

# 2. Copia config
cp wp-config-sample.php wp-config.php

# 3. Configura wp-config.php
# DB_NAME, DB_USER, DB_PASSWORD, DB_HOST

# 4. Instala WP-CLI (opcional pero súper útil)
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp

# 5. Crea base de datos y configura
wp core config --dbname=wordpress --dbuser=root --dbpass='' --dbhost=localhost
wp core install --url="http://localhost:8000" --title="Mi Sitio" --admin_user=admin --admin_password=123456 --admin_email=tu@email.com

# 6. Levanta servidor
php -S localhost:8000
```

## 🐳 **Método 3: Docker (Más profesional)**

Crea `docker-compose.yml`:
```yaml
version: '3.8'
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    ports:
      - "3306:3306"

  wordpress:
    image: wordpress:latest
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - ./:/var/www/html
    depends_on:
      - db
```

```bash
docker-compose up -d
```

## ⚙️ **Script automatizado (package.json style)**

Crea `start.sh`:
```bash
#!/bin/bash
echo "🚀 Iniciando WordPress..."

# Instalar dependencias
composer install --no-dev --optimize-autoloader

# Configurar wp-config si no existe
if [ ! -f "wp-config.php" ]; then
    cp wp-config-sample.php wp-config.php
    echo "✅ wp-config.php creado. Edita las credenciales de DB"
    exit 1
fi

# Instalar WordPress CLI
if [ ! -f "wp" ]; then
    curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
    chmod +x wp-cli.phar
    mv wp-cli.phar wp
fi

# Levantar servidor
php -S 0.0.0.0:8000 -t . &
echo "🌐 WordPress corriendo en http://localhost:8000"
```

```bash
chmod +x start.sh
./start.sh
```

## 📦 **package.json para Node devs** (Bonus)

```json
{
  "scripts": {
    "dev": "php -S localhost:8000",
    "install": "composer install && cp wp-config-sample.php wp-config.php",
    "db:setup": "wp core install --url=http://localhost:8000 --title=MiSitio --admin_user=admin --admin_password=123456 --admin_email=test@test.com",
    "docker": "docker-compose up -d"
  }
}
```

## 🔍 **Comandos WP-CLI útiles**
```bash
wp plugin install akismet --activate
wp theme activate twentytwentythree
wp db export backup.sql
wp cache flush
wp search-replace 'old-domain.com' 'localhost:8000'
```

**¡El equivalente más cercano a Node es el Método 1 (`php -S`) + WP-CLI!** 🎉