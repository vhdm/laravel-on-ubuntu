
# How to install Laravel on Ubuntu


## Install Nginx

```
sudo apt update
sudo apt install nginx
```

```
systemctl enable nginx
```

```
systemctl status nginx
```

```
curl -4 icanhazip.com
```

```
vi /etc/nginx/nginx.conf
```

`http`
```
server_tokens off;
limit_req_zone  $binary_remote_addr  zone=req_limit:10m  rate=10r/s;
limit_conn_zone $binary_remote_addr  zone=conn_limit:10m;
proxy_intercept_errors on;

```

```
vi /etc/nginx/sites-available/laravel.conf
```

```
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;

    root   /var/www/example.com/public;
    index  index.php;
    charset utf-8;

    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;

    client_max_body_size 20m;    
    client_body_timeout 30s;
    client_header_timeout 15s;
    keepalive_timeout 65s;

    limit_req  zone=req_limit  burst=20  nodelay;
    limit_conn conn_limit 200;

    access_log /var/log/nginx/example.access.log;
    error_log  /var/log/nginx/example.error.log warn;

    location ~ ^/flux/flux(\.min)?\.(js|css)$ {
        expires off;
        try_files $uri $uri/ /index.php?$query_string;
    }




    location ~ ^/xvpma/(.+\.php)$ {
        root /var/www/example.com/public;

        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;

        fastcgi_read_timeout 60s;
        fastcgi_send_timeout 60s;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;

        fastcgi_hide_header X-Powered-By;
    }

    location ~ ^/xvpma/(doc|sql|setup|test|config|examples|libraries) {
        deny all;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ ^/index\.php(/|$) {
        include fastcgi.conf; 
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;

        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;

        fastcgi_read_timeout 60s;
        fastcgi_send_timeout 60s;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_busy_buffers_size 64k;

        fastcgi_hide_header X-Powered-By;

        try_files $fastcgi_script_name =404;
    }

    location ~ \.php$ {
        return 403;
    }

    location ~* ^/(?:\.env|\.git|\.hg|\.svn|\.DS_Store) { deny all; }
    location ~* ^/(?:storage|vendor|node_modules|routes|database|resources|tests|config)/ { deny all; }
    location ~* /(composer\.(json|lock)|package(-lock)?\.json|yarn\.lock|artisan|README|LICENSE|Makefile)$ { deny all; }
    location ~ /\.(?!well-known).* { deny all; }  
}
```

```
ln -s /etc/nginx/sites-available/laravel.conf /etc/nginx/sites-enabled/laravel.conf
```

## Install PHP8.3

```
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```

```
sudo apt install php8.3-{cli,fpm,curl,mysqlnd,gd,opcache,zip,intl,common,bcmath,imagick,xmlrpc,readline,memcached,redis,mbstring,apcu,xml,dom,memcache} -y
```

```
sudo systemctl enable php8.3-fpm
```


## Install Composer

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

```
mv composer.phar /usr/bin/composer
chmod +x /usr/bin/composer
```


## Install MariaDB

```
sudo apt install mariadb-server
```

```
sudo systemctl enable mariadb
```

## Install Supervisor

```
sudo apt update
sudo apt install supervisor
```

```
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /path-to-your-laravel-app/artisan queue:work
autostart=true
autorestart=true
user=your-user
numprocs=1
redirect_stderr=true
stdout_logfile=/var/log/laravel-worker.log
```

## Nginx with Let's Encrypt

```
sudo apt install certbot python3-certbot-nginx
```

```
sudo certbot --nginx -d example.com -d www.example.com
```


## Install NodeJS v22

```
cd ~
curl -sL https://deb.nodesource.com/setup_22.x -o nodesource_setup.sh
```

```
sudo bash nodesource_setup.sh
```

```
sudo apt install nodejs
```
