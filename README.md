## A [Composer](http://getcomposer.org/) bundle for [Laravel](http://laravel.com/)

### Quick Start

1. Install the bundle

        php artisan bundle:install composer

1. Add composer to ```bundles.php```

        return array(
            'composer' => array(
            	'auto' => true,
            ),
        );

1. Set up composer's "auto-update" capability ([Set up your database first](http://laravel.com/docs/database/config))

        php artisan composer::setup:auto_update

1. Create ```application/config/composer.php```

        return array(
        	'auto_update' => true, /* <== You'll need that */
        	'require' => array(
        		/* Composer require key styled as an associative array */
        		/* http://getcomposer.org/doc/01-basic-usage.md#the-require-key */
        	),
        );

1. Get out there and use your Composer packages

##### Gotcha: ```auto_update``` requires write permissions in the ```{base}``` and ```{base}/vendor``` directories

##### Note:   The first page load after configuration changes will take a while

### A Partial Example

With this ```application/config/composer.php```

    return array(
        'auto_update' => true,
        'require' => array(
            'monolog/monolog' => '1.0.*',
        ),
    );

Monolog will be available anywhere in your Laravel application, so you could say

    Route::get('/', function()
    {
        $log = new Monolog\Logger('test');
        $log->pushHandler(new Monolog\Handler\StreamHandler(path('storage') . 'logs/mono.log', Monolog\Logger::WARNING));
        $log->addWarning('Hey, look! A visitor!');

        return View::make('home.index');
    });

### Get Started in a *Slightly* More Advanced Way

1. Install the bundle

        php artisan bundle:install composer

1. Add composer to ```bundles.php```

        return array(
            'composer' => array(
            	'auto' => true,
            ),
        );

1. Set up composer's "auto-update" capability ([Set up your database first](http://laravel.com/docs/database/config))

        php artisan composer::setup

1. Create ```application/config/composer.php``` (or don't, see below)

        return array(
        	'auto_update' => false, /* <== This can be omitted */
        	'require' => array(
        		/* Composer require key styled as an associative array */
        		/* http://getcomposer.org/doc/01-basic-usage.md#the-require-key */
        	),
        );

1. Now use the composer bundle's Cli task

        php artisan composer::cli:install

    or

        php artisan composer::cli:update

    etc...

##### Note: With ```auto_update``` set to false, you are responsible for installing and updating your Composer packages.

### More thoughts

It's not necessary to use the composer bundle's Cli task to run Composer. Running ```php artisan composer::setup``` installs composer.phar in the ```{base}``` directory, so from there you can simply run ```php composer.phar [command]```.

If you choose, you need not create ```application/config/composer.php```. As mentioned above, after the composer bundle is setup, you can use it from the ```{base}``` directory, so just create your own ```composer.json``` file there and get going. **Warning:** if you create your own ```composer.json``` and later create ```application/config/composer.php```, your ```composer.json``` might get eaten.

This bundle adds Composer's autoloader to PHP's collection of autoloaders. It's added after Laravel's, so Laravel should have the first shot at resolving classes and namespaces. I suppose this means there is *some* chance of namespaces being resolved by Laravel when you want them to be resolved by Composer, but I guess you can figure that out.