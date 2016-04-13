 #### Buffer
```php
ob_start(); // Start output buffering

// INFO !!;

$data = ob_get_contents(); // Store buffer in variable
ob_end_clean(); // End buffering and clean up
echo $data; // will contain the contents
```
