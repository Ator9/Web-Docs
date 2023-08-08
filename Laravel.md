# Install Laravel
New Project
```sh
composer create-project laravel/laravel .
composer create-project laravel/laravel foldername
```
Existing Project
- clone it from github
- install dependencies
- copy .env.example to .env
- create database

```sh
sudo -u web57 git clone git@bitbucket.org:Ator9/server.git .
sudo chmod -R 777 storage
sudo -u web57 ln -s /var/www/domain/private/public/ /var/www/domain/web/
sudo -u web57 php artisan storage:link
```

```sh
composer install
php artisan key:generate
php artisan migrate
php artisan db:seed
```

## Artisan Console
```sh
php artisan route:list

php artisan make:controller AdminController
php artisan make:controller Admin/AdminController --resource

php artisan make:migration create_xxx_table
php artisan make:migration add_column_to_xxx_table --table="xxx"
```

Database Migrations - <a href="http://laravel.com/docs/migrations">Documentation</a>
```sh
php artisan migrate
php artisan migrate:rollback
php artisan migrate:fresh
```

Production Optimizations
```sh
php artisan config:cache
```

## .git/info/exclude
```sh
*

!app/*
app/*
!app/Adminux
!app/Adminux/*

!database/*
database/*
!database/migrations
database/migrations/*
!database/migrations/2019_01_01_000000_create_adminux.php
!database/seeds
database/seeds/*
!database/seeds/AdminuxSeeder.php

!public/*
public/*
!public/adminux/*

!resources/*
resources/*
!resources/views/*
resources/views/*
!resources/views/adminux/*

!resources/lang/*
resources/lang/*
!resources/lang/en/*
resources/lang/en/*
!resources/lang/es/*
resources/lang/es/*
!resources/lang/en/adminux.php
!resources/lang/es/adminux.php
```
