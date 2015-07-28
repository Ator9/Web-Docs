## Laravel - <a href="http://www.php-fig.org/psr/psr-2/" target="_blank">Coding Style Guide</a>
```sh
composer create-project laravel/laravel foldername
```

## Artisan Console
```sh
php artisan help COMMAND
```

Make Controller
```sh
php artisan make:controller PagesController --plain
```

Database Migrations
```sh
php artisan migrate
```

## Blade Templates - <a href="http://laravel.com/docs/blade" target="_blank">Docs</a>
```html
{{ $varname }} // echo with htmlentities
{!! $varname !!} // echo without htmlentities
```
Master Layout
```html
<html>
<head>
<title>App Name - @yield('title')</title>
</head>
<body>
@section('sidebar')
    This is the master sidebar.
@show

<div class="container">
    @yield('content')
</div>
</body>
</html>
```
Extended
```html
@extends('master')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@stop

@section('content')
    <p>This is my body content.</p>
@stop
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
