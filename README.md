
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
