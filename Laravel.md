## New Project
```sh
composer create-project laravel/laravel foldername
```

## Artisan
Make Controller
```sh
php artisan make:controller PagesController
```

#### Commands
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
