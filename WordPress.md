# WordPress

### Manual Update
<a href="https://codex.wordpress.org/Updating_WordPress#Manual_Update" target="_blank">Manual Update</a>

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

# Avada
### php.ini
```php
max_execution_time = 300
max_input_vars = 1500
```
### wp-config.php
```sh
define('WP_MEMORY_LIMIT', '256M'); // Avada
```
