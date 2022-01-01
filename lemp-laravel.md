# Laravel LEMP

```sh
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
sudo apt install mysql-server -y
sudo mysql_secure_installation
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.0-fpm php8.0-common php8.0-mysql php8.0-xml php8.0-xmlrpc php8.0-curl php8.0-gd php8.0-imagick php8.0-cli php8.0-dev php8.0-imap php8.0-mbstring php8.0-opcache php8.0-soap php8.0-zip unzip php8.0-intl -y
sudo vi /etc/php/8.0/fpm/php.ini
```
### php ini config
```
upload_max_filesize = 32M 
post_max_size = 48M 
memory_limit = 256M 
```

### restart php fpm
```sh
sudo service php8.0-fpm restart
```
### configure nginx
```sh
sudo vi /etc/nginx/sites-available/YOUR.DOMAIN.COM
```
```
server {
    listen 80;
    listen [::]:80;
    server_name YOUR.DOMAIN.COM;
    root /var/www/html/YOUR.DOMAIN.COM/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
```sh
sudo ln -s /etc/nginx/sites-available/YOUR.DOMAIN.COM /etc/nginx/sites-enabled/
sudo unlink /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```
### laravel
