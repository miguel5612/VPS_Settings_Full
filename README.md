Configuraciones VPS Linux (Ubuntu)
==========

cada vez que nos enfrentamos con la publicacion de una plataforma existe la duda sobre como sera la mejor manera de publicarlo. Este repositorio contentra todas y cada una de las instrucciones para configurar y poner en linea tu aplicacion en el puerto 80.

requisitos:
--------------------

1. editor nano
2. curl
3. nginx
4. mysql-server
5. php
6. phpmyadmin
7. wordpress

Como proceder:
--------------------

__Primer paso__

Compra tu dominio en namecheap.com y crea los subdominios. Para crearlos entra a la configuracion avanzada del dns y colocas un registro tipo A, en el segundo campo el nombre del subdominio y luego la ip.

Ejemplo:

(1) onhub.onmotica.com

(2) pma.onmotica.com

__Segundo paso__

Ejecuta los siguientes comandos:

sudo apt-get update

sudo apt-get install nano

sudo apt-get install software-properties-common

sudo apt-get install curl

sudo apt-get install nginx

sudo apt-get install mysql-server

sudo mysql_secure_installation

sudo apt-get install php-fpm php-mysql

sudo apt update

sudo ufw allow 'Nginx HTTP'

systemctl status nginx

__Para mi subdominio(1) onhub.onmotica.com__

sudo nano /etc/nginx/sites-available/onhub.onmotica.com

__Colocar esto dentro de el archivo onhub.onmotica.com en el editor nano__

```
server
{
        server_name onhub.onmotica.com;
        location / {
                proxy_pass http://127.0.0.1:8000;
        }


    listen 80;
}
```

__Activar el sitio web__

sudo ln -s /etc/nginx/sites-available/onhub.onmotica.com /etc/nginx/sites-enabled

__Elimino el sitio por defecto de nginx__

sudo unlink etc/nginx/sites-enabled/default

sudo nano /var/www/html/info.php
```
<?php
phpinfo()
```
sudo apt-get install phpmyadmin

sudo ln -s /usr/share/phpmyadmin /var/www/html

sudo phpenmod mcrypt

sudo systemctl restart php7.0-fpm

__wordpress - apunta a wp.onmotica.com__
cd /tmp
curl -O https://wordpress.org/latest.tar.gz

tar xzvf latest.tar.gz

sudo cp -a /tmp/wordpress/. /var/www/html

sudo chown -R www-data:www-data /var/www/wp.onmotica.com

__Para mi subdominio(2) admindb.onmotica.com__

sudo nano /etc/nginx/sites-available/admindb.onmotica.com

__Php my admin(2) con nginx (admindb.onmotica.com)__

```
server
{
        root /var/www/html/phpmyadmin/;
        index index.php;
        server_name admindb.onmotica.com;

        location / {
                try_files $uri $uri/ =404;
        }
        location ~\.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }
        location ~/\.ht {
                deny all;
        }


    listen 80;

}

```

__Activar el sitio web__


sudo ln -s /etc/nginx/sites-available/admindb.onmotica.com /etc/nginx/sites-enabled

__Verifico lo que he hecho__

sudo nginx -t

__reinicio nginx__

sudo systemctl reload nginx

__para comprobar__

sudo nginx

__En caso que algo ocupe el puerto 80__

sdo lsof -i -P-n

__Para desinstalar apache si viene por defecto__

sudo  apt-get purge apache2 apache2-bin apache2-data apache2-doc apache2-mpm-prefork apache2-utils

__Certificado SSL__

sudo apt-get update

sudo apt-get install software-properties-common

sudo add-apt-repository universe

sudo add-apt-repository ppa:certbot/certbot

sudo apt-get update

sudo apt-get install certbot python-certbot-nginx 

sudo certbot --nginx

__NVM (Node version manager)__

wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"

[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

command -v nvm

export NVM_DIR="$HOME/.nvm"

[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

nvm ls-remote

Contacto:
--------------------
+ miguelangelcu@ufps.edu.co - 3192597748

