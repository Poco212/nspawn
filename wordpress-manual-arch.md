#### webserver

```
sudo pacman -S apache php php-gd php-cgi php-apache
```

```
sudo systemctl start httpd
sudo systemctl enable httpd
```

```
nvim /etc/php/php.ini
```

```
uncommenting this line:
;extension=pdo_mysql
;extension=mysqli
```

#### database

```
sudo pacman -S mariadb
```

```
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

```
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

```
mysql
```

```
create database wordpress;
```

```
create user wp_user;
```

```
grant all on wordpress.* to wp_user@localhost identified by 'password'; 
```

```
flush privileges;
```

```
exit;
```

#### instalation wordpress

```
sudo pacman -S wget
```

```
cd /srv/http
```

```
wget https://wordpress.org/latest.tar.gz
```

```
tar xvf latest.tar.gz
```

```
mv /srv/http/wordpress/wp-config-sample.php  /srv/http/wordpress/wp-config.php
```

```
nvim /srv/http/wordpress/wp-config.php
```

change the following lines:
```
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'wp_user' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password' );
```

```
chown -R root:http /srv/http/wordpress
```

```
nvim /etc/httpd/conf/extra/httpd-vhosts.conf
```

add the following code: 
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot "/srv/http/wordpress"
    ServerName localhost
    ErrorLog "/var/log/httpd/wordpress-error_log"
    CustomLog "/var/log/httpd/wordpress-access_log" common
</VirtualHost>
```

```
nvim /etc/httpd/conf/httpd.conf
```

Uncomment the following line:
```
Include conf/extra/httpd-vhosts.conf
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
```

Comment out the following line:
```
#LoadModule mpm_event_module modules/mod_mpm_event.so
```

Add the following lines:
```
LoadModule php_module modules/libphp.so
AddHandler php-script .php
Include conf/extra/php_module.conf
```

```
sudo systemctl restart httpd
```
note: browse http://ipaddress or http://localhost