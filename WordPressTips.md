### FIX CHMOD
<a href="https://codex.wordpress.org/Create_A_Network" target="_blank">Create a Network</a>
```sh
find . -type d -exec chmod 755 {} \;  # Change directory permissions rwxr-xr-x
find . -type f -exec chmod 644 {} \;
```

Mount uploads in another disk (ispconfig PHP open_basedir :/data1)
```sh
@wp-content#

cp -fr --preserve uploads /data1/uploads
mv uploads uploads.old

ln -sf /data1/uploads uploads  // ln -s [target] [symlink]
chown -h web1:client1 uploads
```

Rsync uploads server to server (r recursive, u sincronize, v verbose, t times)
```sh
rsync -ruvt --delete uploads user@2.2.2.2:/data1
```

```sh
chown -R web1:client1 uploads/
```

## Security
If you are installing WordPress in your web root directory (such as public_html), you can move your wp-config.php file to the parent directory — one that isn’t readable from a browser — without changing any settings. WordPress will automatically recognize the file’s new location.

### FIX WW2 Network Subdomain
```sh
sed -i -e 's/|^www/|^ww2/g' /var/www/ww2.domain.com/web/wp-admin/network/site-new.php
```

# WordPress - <a href="https://codex.wordpress.org/Updating_WordPress#Manual_Update" target="_blank">Manual Update</a>

### Custom Vars wp-config.php
```php
if($_SERVER['HTTP_HOST'] != DOMAIN_CURRENT_SITE) define('COOKIE_DOMAIN', false);

# Disable Theme Editing | trying to avoid hack
define( 'DISALLOW_FILE_EDIT', true );
```

### Block Author with htaccess
```sh
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /

RewriteCond %{QUERY_STRING} ^author= [NC]
RewriteRule .* - [F,L]
RewriteRule ^author/ - [F,L]

RewriteRule ^wp-signup - [F]

# Block/Forbid Requests to: /wp-json
RewriteCond %{REQUEST_METHOD} ^(GET|POST|PUT|PATCH|DELETE) [NC]
RewriteCond %{REQUEST_URI} ^.*wp-json [NC]
RewriteRule ^(.*)$ - [F]

RewriteCond %{REQUEST_URI} ^.*feed/ [NC]
RewriteRule ^(.*)$ - [F]

# block example
RewriteCond %{REQUEST_URI} ^.*hello-world/ [NC]
RewriteRule ^(.*)$ - [F]

# block blog lists 
RewriteCond %{REQUEST_URI} ^.*category/uncategorized/ [NC]
RewriteRule ^(.*)$ - [F]

# block blog lists 
RewriteRule ^\d{4}/(.*)$ - [F]

</IfModule>
```

### Block Files
```sh
<Files xmlrpc.php>
Order Allow,Deny
Deny from all
</Files>

<Files "qnap_firmware.xml">
  Require all denied
</Files>

<Files wp-sitemap.xml>
Order Allow,Deny
Deny from all
</Files>

<Files wp-sitemap-posts-page-1.xml>
Order Allow,Deny
Deny from all
</Files>
```


### Home Redirect with htaccess
```sh
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /

RewriteCond %{HTTP_HOST} ^currentsite.com$ [NC]
RewriteRule ^$ http://newsite.com/ [R=301,L]
</IfModule>
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
#RemoveHandler .php .php3
#RemoveType .php .php3
#php_flag engine off

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

### Divi
Broken content:
```sh
chmod 777 -R /var/www/domain.com/web/wp-content/et-cache/1/

or css dynamic off
```

### Cache htaccess
```sh
# Cache 1 week (year = 31536000)
<FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|swf|mp3|mp4|js|css)$">
Header set Cache-Control "max-age=604800, public, must-revalidate"
</FilesMatch>
```

### Arabic Page / Divi Templates / Text Align Right / RTL
Insert Code in the page - <a href="https://wordpress.org/plugins/page-menu/" target="_blank">Plugin - Select Menu</a>
```javascript
<script>
jQuery('html').attr('dir', 'rtl').attr('lang', 'ar');

jQuery('head').append('<link rel="stylesheet" type="text/css" media="screen" href="'+location.protocol+'//'+location.hostname+'/wp-content/themes/Divi/rtl.css">');

jQuery('head').append('<link rel="stylesheet" type="text/css" media="all" id="contact-form-7-rtl.css" href="'+location.protocol+'//'+location.hostname+'/wp-content/plugins/contact-form-7/includes/css/styles-rtl.css">');

jQuery('body').addClass('rtl');
</script>
<style>
p, h1, h2, h3, h4, h5, h6, h7{text-align:right}
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
