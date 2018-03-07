# WordPress - <a href="https://codex.wordpress.org/Updating_WordPress#Manual_Update" target="_blank">Manual Update</a>

### Multisite
<a href="https://codex.wordpress.org/Create_A_Network" target="_blank">Create a Network</a>
```php
/* Multisite */
define( 'WP_ALLOW_MULTISITE', true );
```

### Custom Vars
```php
if($_SERVER['HTTP_HOST'] != DOMAIN_CURRENT_SITE) define('COOKIE_DOMAIN', false);
```

### Home Redirect with htaccess
```sh
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^currentsite.com$ [NC]
RewriteRule ^$ http://newsite.com/ [R=301,L]
```

### Admin IP Restriction with htaccess
```sh
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{REQUEST_URI} ^(.*)?wp-login\.php(.*)$
RewriteCond %{REMOTE_ADDR} !^IP_ONE$
RewriteCond %{REMOTE_ADDR} !^IP_TWO$
RewriteRule ^(.*)$ - [R=403,L]
</IfModule>
```
Restrict specific IP
```sh
Order Deny,Allow
Deny from xxx.xxx.xx.xx
```

### Cache htaccess
```sh
# Cache 1 week (year = 31536000)
<FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|swf|mp3|mp4|js|css)$">
Header set Cache-Control "max-age=604800, public, must-revalidate"
</FilesMatch>
```

# Avada
### /etc/httpd/conf/httpd.conf
```php
FcgidMaxRequestLen 8000000
```
### php.ini
```php
max_execution_time = 300
max_input_vars = 1500
```
### wp-config.php
```sh
define('WP_MEMORY_LIMIT', '256M'); // Avada
```
