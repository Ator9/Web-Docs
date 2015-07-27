## New Project
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
@extends('layouts.master')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
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
