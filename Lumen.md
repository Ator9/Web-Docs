## Lumen - <a href="http://www.php-fig.org/psr/psr-2/" target="_blank">Coding Style Guide</a>
```sh
composer create-project laravel/lumen foldername
```

## Model
```sh
php artisan make:model Admin
```
```php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Admin extends Model
{
    protected $table = 'admins';
    protected $primaryKey = 'adminID';

    const CREATED_AT = 'date_created';
    const UPDATED_AT = 'date_updated';
}
```

## Query Builder - <a href="http://laravel.com/docs/queries" target="_blank">Docs</a>
```php
use DB;
$users = DB::table('users')->get();
```

## Misc Commands
Project test (@foldername)
```sh
php -S localhost:8888 -t public
```

Edit composer.json and add this line.
```json
"config": {
"preferred-install": "dist"
}
```
