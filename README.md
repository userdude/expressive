# Pure Expressive

A simple but powerful module loader written for PHP.

---

This is the most basic version of the Pure Expressive project I've been working
on for a while. The objective is to show as minimal as possible example of module
loading using a more modern PHP syntax and controllable environment (container).

Overall, the service implementation with script execution is <200 lines of code.
By itself this isn't too interesting. To a large degree I consider this a replacement
for pretty much any container requirement except where a package expressly needs
a container. Which of course you can add a service to get one for you. Really is
that easy. `:D`

First, let's look at our files.

- `play`: Calls a console service against the `app/console` php script.
- `worker`: This command starts a worker session that `play` uses to run the service 
   request. This provides a consistent PHP version (8.0 as of now).

This basic, compat-level expressive only has one folder, and two more files:

- `app/`: Directory with our service definitions.
- `app/console`: Used to call service commands directly.
- `app/make/service.php`: A simple make script.

> **Important!** A service name is always minus the app/ and .php. For instance, 
> `app/make/service/php` would be become `make/service`.

Now let's create our first service. Painless:

```shell
./play make/service foo/bar
```

Running that in the console, you should receive something like:

```shell
$ ./play make/service foo/bar
Success! /app/foo/bar.php service file generated.
```

You should see that file now in your `app/foo` directory, which was also created.

Opening the file, you should see this:

```php
<?php

declare(strict_types=1);

namespace App\Foo;

interface Bar {
    public function __invoke();
}

return function() use(&$context) {
    // TODO: Make foo/bar happen.
};
```

Let's replace the `return`. Copy and paste this over the file contents:

```php
<?php

declare(strict_types=1);

namespace App\Foo;

interface Bar {
    public function __invoke();
}

return fn() => 'Baz!!!';
```

> **Note!** The `app/console` JSON encodes any service response.

Returning a string from our service allows us to output the service's response
directly to the shell. 

Now in a shell, run the following command:

```shell
./play foo/bar
```

You should receive your function's response in the console output.

```shell
$ ./play foo/bar
Baz!!!
```























