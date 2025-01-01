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
- MediaWiki:Aboutsite
```
Unset PoweredBy
```php;
unset( $wgFooterIcons['poweredby'] );
```
Add custom external link
```php;
$wgHooks['SkinAddFooterLinks'][] = function ( Skin $skin, string $key, array &$footerlinks ) {
    if ( $key === 'places' ) {
        $footerlinks['test'] = Html::rawElement( 'a',
            [
                'href' => 'https://www.example.org/wiki/Project:Imprint',
                'rel' => 'noreferrer noopener' // not required, but recommended for security reasons
            ],
        $skin->msg( 'test-desc' )->escaped() // test-desc is an i18n message of the text
        );
    };
};
```
