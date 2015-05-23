###View Composers

Sirven para contener código que se puede asociar a varias plantillas. Podríamos decir que son como los `Traits` pero para las plantillas.

Son callbacks o métodos que se ejecutan al renderizar una vista.

Permiten abstraer código repetido en varias plantillas a un fichero común. Especialmente, código que proporciona los mismos datos a varias vistas.

Asociar un view composer a la vista `profile`:
```
View::composer('profile', function($view)
{
  $view->with('count', User::count());
});
```

Asociarlo a las vistas `profile` y `dashboard`:
```
View::composer([ 'profile','dashboard' ], function($view)
{
  $view->with('count', User::count());
});
```

Usando una clase:
```
View::composer('user.index', 'UserComposer');

// UserComposer.php
class UserComposer {

    public function compose($view)
    {
        $view->with('count', User::count());
    }

}
```

###Más info:

[View Composers en la documentación oficial de Laravel](http://laravel.com/docs/master/views#view-composers)

http://culttt.com/2014/02/10/using-view-composers-laravel-4/
http://laraveles.com/foro/viewtopic.php?id=432
http://www.codeheaps.com/php-programming/creating-blog-using-laravel-4-part-4-layout-views/
http://laravelsnippets.com/snippets/view-composer-extends-paginator-with-query-string
http://nicolaswidart.com/blog/view-composers-and-view-creators-in-laravel
http://forumsarchive.laravel.io/viewtopic.php?id=12144
http://laravel.io/forum/12-17-2014-use-of-viewcomposer
http://blog.chrislahaye.com/view-composers-to-share-data-with-views-in-laravel/
https://laracasts.com/discuss/channels/general-discussion/laravel-5-view-composers
http://heera.it/laravel-4-view-composer-master-layout#.VV0NPvn0jDc
http://culttt.com/2014/02/10/using-view-composers-laravel-4/