# WordPress

### Multisite
https://codex.wordpress.org/Create_A_Network
```php
/* Multisite */
define( 'WP_ALLOW_MULTISITE', true );
```

### Custom Vars
```php
if($_SERVER['HTTP_HOST'] != DOMAIN_CURRENT_SITE) define('COOKIE_DOMAIN', false);
```
