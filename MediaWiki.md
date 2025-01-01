# OptionsLocalSettings.php
Allow same origin iframe
```php
$wgEditPageFrameOptions = 'SAMEORIGIN';
```
Max 1MB uploads
```php;
$wgMaxUploadSize = 1000000;
```
Disable page creation for logged in users
```php;
$wgGroupPermissions['*']['createpage'] = false;
$wgGroupPermissions['user']['createpage'] = false;
```
To remove the privacy, about and disclaimer links, replace the link text with a single dash ("-").
```php;
- MediaWiki:Privacy
- MediaWiki:Disclaimers
```

Unset PoweredBy
```php;
unset( $wgFooterIcons['poweredby'] );
```
