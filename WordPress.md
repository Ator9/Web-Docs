# WordPress - <a href="https://codex.wordpress.org/Updating_WordPress#Manual_Update" target="_blank">Manual Update</a>

### Multisite
<a href="https://codex.wordpress.org/Create_A_Network" target="_blank">Create a Network</a>
```php
/* Multisite */
define( 'WP_ALLOW_MULTISITE', true );
```

### Custom Vars wp-config.php
```php
if($_SERVER['HTTP_HOST'] != DOMAIN_CURRENT_SITE) define('COOKIE_DOMAIN', false);

# Disable Theme Editing | trying to avoid hack
define( 'DISALLOW_FILE_EDIT', true );
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

### Block PHP in Uploads Folder
```sh
Options -Indexes

#Apache 2.2
RemoveHandler .php .php3
RemoveType .php .php3
php_flag engine off

#Apache 2.4
<FilesMatch \.php$>
SetHandler None
ForceType text/plain
</FilesMatch>

<FilesMatch ".(php)$">
Order allow,deny
Deny from all
</FilesMatch>
```


### Cache htaccess
```sh
# Cache 1 week (year = 31536000)
<FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|swf|mp3|mp4|js|css)$">
Header set Cache-Control "max-age=604800, public, must-revalidate"
</FilesMatch>
```

### Arabic Page
Insert Code in the page
```javascript
<script>
jQuery('html').attr('dir', 'rtl').attr('lang', 'ar');

jQuery('head').append('<link rel="stylesheet" type="text/css" media="screen" href="'+location.protocol+'//'+location.hostname+'/wp-content/themes/Divi/rtl.css">');

jQuery('head').append('<link rel="stylesheet" type="text/css" media="all" id="contact-form-7-rtl.css" href="'+location.protocol+'//'+location.hostname+'/wp-content/plugins/contact-form-7/includes/css/styles-rtl.css">');

jQuery('body').addClass('rtl');
</script>
<style>
p, h1, h2, h3, h4, h5, h6, h7{text-align:right !important}
input[type="text"], input[type="email"], textarea{text-align:right !important}
</style>
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

define('ALLOW_UNFILTERED_UPLOADS', true); // Allow custom fonts upload
```
