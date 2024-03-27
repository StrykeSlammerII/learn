---
title: Registering Routes
metadata:
    description: Once your routes definitions are ready, you have to register them inside your Sprinkle Recipe.
taxonomy:
    category: docs
---

So far we've seen how to [create routes definitions](/routes-and-controllers/front-controller) and [controller classes](/routes-and-controllers/controller-classes). However, there one last step required for our routes to be enabled inside our application. That is registering the route class inside the [Sprinkle Recipe](/sprinkles/recipe#routes). 

[notice]Previous version of UserFrosting relied on a naming convention for registering the routes. Routes were expected to be placed in a special directory, and would automatically be registered at runtime. To provide more flexibility, the naming convention has been dropped in UserFrosting 5. You now have to register every class you wish to register, in the order you want them to be registered, inside the Sprinkle Recipe.[/notice]

First step is to create a new class that will returns the Slim route definition. This class **must** implement the `UserFrosting\Routes\RouteDefinitionInterface` interface from the UserFrosting Framework. For example : 

**app/src/MyRoutes.php**
```php
<?php

namespace UserFrosting\App;

use Slim\App;
use UserFrosting\App\Controller\AppController;
use UserFrosting\Routes\RouteDefinitionInterface;

class MyRoutes implements RouteDefinitionInterface
{
    public function register(App $app): void
    {
        $app->get('/owls', [AppController::class, 'pageOwls'])->setName('page.owls');
    }
}
```

Note in the previous example how the class has a [FQN](https://www.php.net/manual/en/language.namespaces.rules.php) of `\UserFrosting\App\MyRoutes`. 

To register this class inside your application, you need to add it to the `getRoutes()` method of the Sprinkle Recipe. Don't forget to add the previous class to the `use` block at the top. For example :

**app/src/MyApp.php**
```php
<?php

namespace UserFrosting\App;

// ...
use UserFrosting\Sprinkle\SprinkleRecipe;
use UserFrosting\App\MyRoutes; // <-- Add here !
// ...

class MyApp implements SprinkleRecipe {
    
    // ... 

    /**
     * Returns a list of routes definition in PHP files.
     *
     * @return string[]
     */
    public function getRoutes(): array
    {
        return [
            MyRoutes::class, // <-- Add here !
        ];
    }
}
```

The routes definitions defined un `MyRoutes` will now be served by UserFrosting !

[notice]Controller classes doesn't need to be registered. As seen in the previous pages, the controller classes will be automatically injected by the Dependency Injection Container when the route is resolved.[/notice]

[notice=tip]Routes classes can be located anywhere. Here they are in `/app/src/` directly. But you can definitively store them in other directory or subdirectories. For example : `app/src/Routes/`, `app/src/Routes/Api/`, `app/src/Owls/Routes/`, etc. Don't forget to adapt the namespace accordingly and import the correct class in your recipe ![/notice]

## Overriding Routes

Routes themselves cannot be extended by other Sprinkles and they cannot be overridden. To modify the behavior of one of the routes that ships with UserFrosting, you may simply redefine it in one of your route classes. However, you cannot register two routes with the same path, otherwise the following error will be thrown:

```txt
Cannot register two routes matching "/" for method "GET"
```

To solve this, it's possible to manually customize a dependency Sprinkle Recipe. Check out the [Advanced Dev Features](/advanced) chapter for more info on this technique.
 <!-- TODO : Update link when page is created -->

[notice=tip]For this reason, if you plan to distribute your Sprinkle as a Community Sprinkle, it can be helpful to split your routes into multiple classes instead of a single big class. It will be easier for dependent sprinkle to cherry pick the routes they need or want to overwrites.[/notice]
