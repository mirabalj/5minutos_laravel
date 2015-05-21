###Plantillas 

Crea una vista para usarla como plantilla:

Utiliza la extensión `.blade.php` en lugar de `.php`:

```
layout.blade.php
```


```
<!-- Stored in resources/views/layouts/master.blade.php -->

<html>
    <head>
        <title>Blog con mucha salsa - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            Esta es la barra lateral de la página principal
        @show

        <div class="container">
            @yield('content')
        </div>

        @section('foot')
            Este es el pie de la página principal
        @show

	</body>
</html>
```

- Definir un campo a reemplazar en la plantilla hija:

Usa `@yield`.

```
@yield('title')
```

> Le puedes pasar un valor por defecto como segundo parámetro. Se usará si la plantilla no redefine ese campo.
> Por ejemplo:  
> ```
>	<head>
>		<title>Blog con mucha salsa - @yield('title',' Y mucho sabor')</title>
>	</head>

- Definir una sección que puede ser extendida o reemplazada en la plantilla hija:

Usa `@section` y `@show`.

```
@section('sidebar')
	(Contenido)
@show
```

Si la plantilla hija define esa sección, el contenido del padre será sobreescrito con el contenido de la plantilla hija. Para añadir también el contenido de la plantilla padre, puedes usar `@parent`.

>**Nota:** La diferencia entre `@section` y `@yield` es que `@yield` está pensado para secciones que siempre van a ser sobreescritas por las plantillas hijas. Por eso, la forma de indicar un valor por defecto está pensada para valores simples y o para bloques de código. Mientras que `@section` está pensada para definir contenido en la plantilla padre y dar la oportunidad a que una plantilla hija lo complemente o lo sobreescriba. `@yield` se suele usar para los contenidos principales de las páginas que tenemos la certeza de que serán sobreescritos y `@section` para condenidos complementarios como barras laterales, widgets, cabeceras, pies, etc...


Ahora, en las 'plantillas hijas':

```
@extends('layouts.master')

@section('title', 'Salsa cubana')

@section('sidebar')
    @parent

    <p>Este contenido es añadido a la barra lateral de la plantilla padre .</p>
@stop

@section('content')
    <p>Este es el contenido de la página.</p>
@stop

@section('foot')
	Este pie sobreescribe el pie de la página principal
@stop
```

- Extender de una plantilla.

Usa `@extends`.

```
@extends('layouts.master')
```

- Establecer el valor de un campo definido en la plantilla padre:

Usa `@yield`.

```
@section('title', 'Salsa cubana')
```

- Sobreescribir una sección definida en la plantilla padre:

Usa `@section` y `@stop`.

```
@section('foot')
	Este pie sobreescribe el pie de la página principal
@stop
```

> Usa `@parent` para añadir el contenido de la plantilla padre.

- Incluir una plantilla hija.

También puedes hacer el proceso contrario, en lugar de crear una plantilla que extienda de la plantilla padre, puedes incluir esa plantilla desde la plantilla padre:

```
@include('view.name')
```

Y puedes pasarle datos igual que haces desde el controlador:

```
@include('view.name', ['var1' => 'data', 'var2' => 'another data'])
```


###Mostrar datos:

Usa `{{ código php }}`. Que sería el equivalente a `<?php código php ?>`.

```
Encantado de saludarte, {{ $name }}.

Eres el visitante número {{ getGuestsCounter() }}.
```

###Comprobar si una variable está vacía antes de mostrarla:

```
Encantado de saludarte, {{ $name or 'Desconocid@' }}.

Eres el visitante número {{ getGuestsCounter() }}.
```

> Si quieres mostrar textos que contengan los símbolos `{{ o }}', usa `@` delante del texto.
>
> ```
> En las plantillas de Laravel, usa @{{ código php }} para ejecutar y mostrar código php.
> ```

###Utilizar variables que contengan código HTML.

Por defecto, antes de mostrar el contenido, se pasa a través de la función `htmlentities()` para evitar ataques XSS. Si tus variables contienen código HTML. Por ejemplo, `$mail = <strong>jatubio@gmail.com</strong>`, utiliza `{!! !!}`:

```
Encantado de saludarte, {!! $name or 'Desconocid@' !!}.

Eres el visitante número {{ getGuestsCounter() }}.
```

> **Nota:** No uses **NUNCA** este modo para mostrar contenido que haya sido introducido por los usuarios.

###Bucle if.

Puedes utilizar `@if` como siempre, o utilizar el nuevo `@unless` que sería equivalente a `if not ()`.

```
@if (($user->rol) === 'Admin')
    Bienvenido, Administrador.
@elseif (($user->rol) === 'Usuario')
    Bienvenido, Usuario.
@else
    Bienvenido, ¿Tienes un rol nuevo?
@endif

@unless (Auth::check())
    No estás autenficiado.
@endunless
```

###Bucles.

Tienes los bucles habituales de `@for`, `@while` y `@foreach` y el nuevo `@forelse` que te permite especificar contenido dentro de `@empty` en el caso de que la colección esté vacía.

```
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

###Determining If A View Exists

If you need to determine if a view exists, you may use the exists method:

if (view()->exists('emails.customer')) {
    //
}

###Returning A View From A File Path

If you wish, you may generate a view from a fully-qualified file path:

return view()->file($pathToFile, $data);

###Funciones útiles:

- Añadir el host a las urls estáticas.

Puedes usar la función `asset()` para que te añada el dominio o host a las urls estáticas. 
Suponiendo que el sitio de tu aplicación es: `http://www.tudominio.com/`, la siguiente línea:

```
<link href="{{ asset('/assets/css/style.css') }}" rel="stylesheet">
```

Escribe en tu html: 

```
<link href="http://www.tudominio.com/assets/css/style.css" rel="stylesheet">
```


####Errores:

- `ErrorException in FileViewFinder.php line 140:`

 ```
ErrorException in FileViewFinder.php line 140:
View [app] not found. (View: D:\Proyectos\Repositorios_Git\Laravel\Salud Digital\sdf\resources\views\home.blade.php)

in FileViewFinder.php line 140
at CompilerEngine->handleViewException(object(InvalidArgumentException), '1') in PhpEngine.php line 43
at PhpEngine->evaluatePath('D:\Proyectos\Repositorios_Git\Laravel\Salud Digital\sdf\storage\framework\views/cbeb3125295553e2c29b382694929447', array('__env' => object(Factory), 'app' => object(Application), 'errors' => object(ViewErrorBag))) in CompilerEngine.php line 57
at CompilerEngine->get('D:\Proyectos\Repositorios_Git\Laravel\Salud Digital\sdf\resources\views/home.blade.php', array('__env' => object(Factory), 'app' => object(Application), 'errors' => object(ViewErrorBag))) in View.php line 136
```

Hay que borrar la cache de las vistas.

----------------
https://laracasts.com/discuss/channels/general-discussion/what-exactly-are-the-differences-between-show-yield-parent

explicacion de @overwrite: http://stackoverflow.com/questions/21169414/laravel-blade-templating-discrepancy

Investigar y explicar graficamente el flujo de las plantillas incluyendo recursividad

Explicación de @stop, @show, @append y @overwrite

En la plantilla padre, si usas `@endsection` y la plantilla hija no declara la sección, aunque tengamos contenidos en la padre, no se mostrarán. Para que se muestren, usar @show


http://laravel-recipes.com/recipes/244/stopping-injecting-content-into-a-section-and-overwriting

Un montón de trucos sobre las plantillas: http://laravel-recipes.com/categories/9

https://laravelartisan.wordpress.com/2015/03/09/blade-templating-in-laravel/

http://www.codeitive.com/0yyjUgXeWV/override-section-in-a-laravel-blade-template-throwing-undefined-variable-errors.html

http://programming.nullanswer.com/forum/6043477

http://gotoanswer.stanford.edu/confusion_between_childparent_relationship-17554627/

http://tiku.io/questions/3915739/override-section-in-a-laravel-blade-template-throwing-undefined-variable-errors

http://www.tagwith.com/question_871800_how-can-i-straighten-out-laravel-blade-extends-order-of-execution

Extending Blade: Custom directives

http://laravel.com/docs/master/views

Buscar también info sobre @endsection

https://scotch.io/tutorials/simple-laravel-layouts-using-blade