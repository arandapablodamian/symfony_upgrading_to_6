

# Tutorials, Friendship & Upgrading to Symfony6

Author: Aranda Pablo
Requirements:
- PHP 8
- mysql or MariaDb
- Composer 2.X
- Symfony console

Well hi there! This repository holds the code and script
for the [Symfony6 Upgrade Tutorial](https://symfonycasts.com/screencast/symfony6-upgrade) on SymfonyCasts.

## Setup

If you've just downloaded the code, congratulations!!

To get it working, follow these steps:

**Download Composer dependencies**

Make sure you have [Composer installed](https://getcomposer.org/download/)
and then run:

```
composer install --ignore-platform-reqs
```

The `--ignore-platform-reqs` is added because our "old code" contains old
dependencies that support PHP 8 (which you are probably using). Well, in
reality, the code *does* work in PHP 8. And so by adding this flag, it tells
Composer to download those dependencies anyways. It's definitely time to
upgrade this old code!

You may alternatively need to run `php composer.phar install`, depending
on how you installed Composer.

**Database Setup**

The code comes with a `docker-compose.yaml` file and we recommend using
Docker to boot a database container. You will still have PHP installed
locally, but you'll connect to a database inside Docker. This is optional,
but I think you'll love it!

First, make sure you have [Docker installed](https://docs.docker.com/get-docker/)
and running. To start the container, run:

```
docker-compose up -d
```

Next, build the database and the schema with:

```
# "symfony console" is equivalent to "bin/console"
# but its aware of your database container
symfony console doctrine:database:create
symfony console doctrine:schema:update --force
symfony console doctrine:fixtures:load
```

(If you get an error about "MySQL server has gone away", just wait
a few seconds and try again - the container is probably still booting).

If you do *not* want to use Docker, just make sure to start your own
database server and update the `DATABASE_URL` environment variable in
`.env` or `.env.local` before running the commands above.

**Webpack Encore Assets**

This app uses Webpack Encore for the CSS, JS and image files. We'll
be tweaking the Encore setup while upgrading, so we want to get this
running. Make sure you have [yarn](https://yarnpkg.com/lang/en/)
or `npm` installed (`npm` comes with Node) and then run:

```
yarn install
yarn encore dev --watch

# or
npm install
npm run watch
```

**Start the Symfony web server**

You can use Nginx or Apache, but Symfony's local web server
works even better.

To install the Symfony local web server, follow
"Downloading the Symfony client" instructions found
here: https://symfony.com/download - you only need to do this
once on your system.

Then, to start the web server, open a terminal, move into the
project, and run:

```
symfony serve
```

(If this is your first time using this command, you may see an
error that you need to run `symfony server:ca:install` first).

Now check out the site at `https://localhost:8000`

Have fun!

## Have Ideas, Feedback or an Issue?

If you have suggestions or questions, please feel free to
open an issue on this repository or comment on the course
itself. We're watching both :).

## Magic

Sandra's seen a leprechaun,
Eddie touched a troll,
Laurie danced with witches once,
Charlie found some goblins' gold.
Donald heard a mermaid sing,
Susy spied an elf,
But all the magic I have known
I've had to make myself.

Shel Silverstein

## Thanks!

And as always, thanks so much for your support and letting
us do what we love!

<3 Your friends at SymfonyCasts


## Course annotations

Tu run rector

```
./vendor/bin/rector init
```

It creates de rules files in rector.php

To run rector

```
SymfonyLevelSetList
```

After we can install PHP Fixer

```
mkdir -p tools/php-cs-fixer
```

and after
```
friendsofphp/php-cs-fixer.
```

And finally to run cs-fixer

```
./tools/php-cs-fixer/vendor/bin/php-cs-fixer fix
```

remueve use que no utilizamos
cambia a nombres cortoes los use en los constructores


## Recomendacion con php cs fixer
There is no perfectly right or wrong way to do this... and installing a tool like php-cs-fixer into your main composer.json is fine in most cases.

However, a tool like php-cs-fixer is... exactly that: a standalone, executable tool. If you install it into your main composer.json file, that tool - and its dependencies - need to be compatible with the dependencies of your application... like any other package.

But because a tool like php-cs-fixer isn't meant to be used and referenced from your code (it will just be run from the command line), it doesn't need to live in your main composer.json file. For that reason, as a best practice, it's simpler to create a tools/ directory at the root of your project and install things there


## Updating the All-Important FrameworkBundle Recipe

Run
```
composer recipes
```

```
composer recipes:update
```

After thar correct de conflicts from the recipe files, and after updating

```
composer require symfony/runtime
```

First update
- symfony framework recipe
- twig
- doctrine

y asi sucesivamente con todos resolviendo conflictos


Luego para cambiar de version de php

En rector php reemplazamos para que quede asi

$rectorConfig->sets([
    LevelSetList::UP_TO_PHP_81
]);

Luego ejecutamos
```
vendor/bin/rector process src
```

## Promoted propeties
Esto cambia en los constructores para inyectar directamente las variables con el tipo dentro del ()

Tambien modifica algunas cosas de codigo necesarias como revision de sintaxis de php -cs-fixer

Example:
This

class Pizza
{
    private array $toppings;

    public function __construct(array $toppings)
    {
        $this->toppings = $toppings;
    }
}

To this

class Pizza
{
    public function __construct(private array $toppings)
    {
    }
}


Luego para cambiar las anotaciones de entidades por los atributos de php8

En rector.php reemplazamos a

$rectorConfig->import(DoctrineSetList::ANNOTATIONS_TO_ATTRIBUTES);
$rectorConfig->import(SymfonySetList::ANNOTATIONS_TO_ATTRIBUTES);
$rectorConfig->import(SensiolabsSetList::FRAMEWORK_EXTRA_61);


y corremos


```
vendor/bin/rector process src
```

Y luego corremos para corregir cosas como espacio y demas

```
tools/php-cs-fixer/vendor/bin/php-cs-fixer fix
```


You can run 

```
composer outdated
```

to gell all outupdated packages.


## Some annotations

###Composer recipes
The composer recipes:update command (which is added by Symfony Flex) checks which recipes are installed in your project and looks for new versions. When you choose a recipe to update, it generates a diff between the original version and the new version... then applies those changes.


You should always check the differences closely to avoid losing any custom config... though (other than old files being entirely deleted), the patch system is pretty good at its job.

## Atributes in php 8
Attributes are a new feature of PHP 8, and the goal is to add metadata to classes, methods, variables, etc. in a structured way (and without the need of a custom parser). Before attributes, docblocks (i.e. annotations) were used to simulate their behavior.

## Profiler

in the browser url write /_profiler and there you have the last request to get all the information we need

Separate the deprecations in a different monolog in prod is a good way to kwnow.

## To know why it doesn't install an updated packege with composer up

For example
```
composer why-not doctrine/dbal 3
```

## renderForm (symfony >=6) vs render (symfony <6)
The biggest motivation behind introducing the renderForm() shortcut method is to return a 422 status code when validation fails instead of a 200 status code. For most users and projects, even though 422 is "more correct", you won't notice any difference. However, if you use the Turbo JavaScript library, a 422 response is needed to tell Turbo that validation failed.

And, since we were already adding this shortcut method, as a bonus, we designed it to call createView() automatically for you. Sweet!

## Important for docker dependencies

The Symfony binary is smart enough to detect your Docker services and expose the environment variables with the appropriate port value. By the way, the env vars won't be set up correctly if you run a command in the traditional way with bin/console app:do:something. You need to execute it through the Symfony binary symfony console app:do:something.