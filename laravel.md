Laravel Packages /Apps

Module Manager
https://nicolaswidart.com/blog/writing-modular-applications-with-laravel-modules
https://github.com/nWidart/laravel-modules

composer require nwidart/laravel-modules
php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"

{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/"
    }
  }
}
composer dump-autoload
php artisan module:make Blog

Menu
https://nwidart.com/laravel-menus/v1/introduction
https://github.com/nWidart/laravel-menus

Laravel Debugbar
https://github.com/barryvdh/laravel-debugbar
composer require barryvdh/laravel-debugbar --dev

If you don't use auto-discovery, add the ServiceProvider to the providers array in config/app.php

Barryvdh\Debugbar\ServiceProvider::class,

If you want to use the facade to log messages, add this to your facades in app.php:

'Debugbar' => Barryvdh\Debugbar\Facade::class,

php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"


PhpSpreadsheet
https://github.com/PHPOffice/PhpSpreadsheet
composer require phpoffice/phpspreadsheet

PhpWord
https://github.com/PHPOffice/PHPWord
composer require phpoffice/phpword

Entrust
https://github.com/Zizaco/entrust
"zizaco/entrust": "5.2.x-dev"

provider
Zizaco\Entrust\EntrustServiceProvider::class,

aliases
'Entrust'   => Zizaco\Entrust\EntrustFacade::class,
php artisan vendor:publish

Larvale Socialite
https://github.com/laravel/socialite
composer require laravel/socialite


Laravel User Verification
https://github.com/jrean/laravel-user-verification
composer require jrean/laravel-user-verification

provider
Jrean\UserVerification\UserVerificationServiceProvider::class,

aliases
'UserVerification' => Jrean\UserVerification\Facades\UserVerification::class,

php artisan vendor:publish --provider="Jrean\UserVerification\UserVerificationServiceProvider" --tag="config"

Laravel Breadcrumbs
https://github.com/davejamesmiller/laravel-breadcrumbs
composer require davejamesmiller/laravel-breadcrumbs:5.x
php artisan vendor:publish --provider="DaveJamesMiller\Breadcrumbs\BreadcrumbsServiceProvider"

Eloquent Sluggable
https://github.com/cviebrock/eloquent-sluggable
composer require cviebrock/eloquent-sluggable:^4.5
php artisan vendor:publish --provider="Cviebrock\EloquentSluggable\ServiceProvider"

Laravel Migrations Generator
https://github.com/Xethron/migrations-generator
composer require --dev "xethron/migrations-generator"

provider
Way\Generators\GeneratorsServiceProvider::class,
Xethron\MigrationsGenerator\MigrationsGeneratorServiceProvider::class,

No CAPTCHA reCAPTCHA
https://github.com/anhskohbo/no-captcha
composer require anhskohbo/no-captcha

provider
Anhskohbo\NoCaptcha\NoCaptchaServiceProvider::class,

aliases
'NoCaptcha' => Anhskohbo\NoCaptcha\Facades\NoCaptcha::class,
php artisan vendor:publish --provider="Anhskohbo\NoCaptcha\NoCaptchaServiceProvider"

Artisan View
https://github.com/svenluijten/artisan-view
composer require sven/artisan-view:^3.0 --dev

Laravel Backup
https://github.com/spatie/laravel-backup

php artisan backup:run

Laravel Permissions
https://github.com/spatie/laravel-permission
composer require spatie/laravel-permission
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="migrations"


CORS Middleware
https://github.com/barryvdh/laravel-cors
composer require barryvdh/laravel-cors
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"

Laravel GarphQL
https://github.com/Folkloreatelier/laravel-graphql

php artisan vendor:publish --provider="Folklore\GraphQL\ServiceProvider"

Laravel Mix
https://github.com/JeffreyWay/laravel-mix

Laravel 5 Extended Generators
https://github.com/laracasts/Laravel-5-Generators-Extended
composer require laracasts/generators --dev


composer require doctrine/dbal
	- for deleting column on a table using migrate:rollback

composer require illuminate/html
	- for creating forms and html

composer laracasts/flash
	- used for flash messaging


Laravel DomPDF
Laravel Generators
Intervention
Parsedown
patricktalmadge/bootstrapper


http://www.atlantis-cms.com/#composerInstall
https://quarxcms.com/
https://pyrocms.com/documentation/pyrocms/v3.1
https://github.com/gitscrum-community-edition/laravel-gitscrum
http://builtwithlaravel.com/


[Grafite CMS]

composer require grafite/cms
php artisan vendor:publish --provider="Grafite\Cms\GrafiteCmsProvider"
php artisan grafite:cms
U: admin@example.com
P: admin


[Laravel Voyager]

composer require tcg/voyager
php artisan voyager:install
or
php artisan voyager:install --with-dummy
php artisan voyager:admin your@email.com --create


[Lara Admin Panel]

composer require "dwij/laraadmin:1.0.40"
'providers' => [
	...
	Dwij\Laraadmin\LAProvider::class
],
php artisan la:install


