Configuring Doctrine ORM
========================

Nice features a simplistic Doctrine ORM bridge that integrates the two, exposing a few useful services. Usage 
is straightforward: install the Doctrine ORM package and then configuration the extension.

First, add the Nice Doctrine ORM extension to your project. You can do this by updating your `composer.json` or
running `composer require` at the command line.

*   Example `composer.json`:

    ```json
    {
        "require": {
            "nice/framework": "~1.0",
            "nice/doctrine-orm": "~1.0"
        }
    }
    ```
    
    Then run `composer update` at the command line.
    

*   Using the `composer` command line tool

    ```
    composer require nice/doctrine-orm:~1.0
    ```

Once Doctrine ORM and the Nice extension are installed, open your front controller (usually `web/index.php`) and 
add the following:

```php
use Nice\Extension\DoctrineOrmExtension;

// ...

$app = new Application();
$app->appendExtension(new DoctrineOrmExtension(array(
    'database' => array(
        'driver' => 'pdo_sqlite',
        'path' => '%app.root_dir%/sqlite.db'
    ),
    'mapping' => array(
        'default' => array(
            'driver' => 'annotation',        // Or xml, yml, or php
            'paths' => '%app.root_dir%/src', // Path to your entities or mapping files
            'namespace' => 'Example'
        ),
    ),
)));
```

> **Note:** You will need to modify the `namespace` option in the above example to match your project.

You can specify multiple mapping drivers. Ensure each has a unique name and namespace prefix.

```php
// ...
    'mapping' => array(
        'default' => array(
            'driver' => 'annotation',
            'paths' => '%app.root_dir%/src',
            'namespace' => 'Example\\Entity'
        ),
        'secondary' => array(
            'driver' => 'yml',
            'paths' => '%app.root_dir%/mapping',
            'namespace' => 'Awesome\\Entity'
        ),
    ),
)));
```

Two services are made available by the extension:

* `doctrine.orm.entity_manager` is the EntityManager. It is an instance of 
[Doctrine\ORM\EntityManager](http://doctrine-orm.readthedocs.org/en/latest/reference/working-with-objects.html).
* `doctrine.orm.configuration` is the Configuration instance for the database.
