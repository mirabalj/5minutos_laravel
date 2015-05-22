####Recorrer registros

```
File: index.blade.php

    @foreach($boletinEMails as $boletin)
        <boletinmail>
            <h2>
                <a href="/boletin/{{ $boletin->id }}">{{ $boletin->EMail }}</a>
            </h2>
        </boletinmail>
    @endforeach
```

####Generar una url que muestre el detalle de un registro:

Podemos usar la opción más sencilla, llamada también 'hardcoded':

```
    <a href="/boletin/{{ $boletin->id }}">{{ $boletin->EMail }}</a>
```

Genera un link de tipo `/boletin/<id>`.
	
En este caso, si modificamos la ruta, tendremos que modificar la vista a mano.

###Generar una url que ejecute una acción en un controlador:

Con la ventaja de que llamamos directamente al controlador sin necesidad de 'tener conocimiento' sobre cual es el link o ruta asociados.

Utilizamos la función `action('Controlador@acción', [parámetros]);`

```
File: index.blade.php

    @foreach($boletinEMails as $boletin)
        <boletinmail>
            <h2>
                <a href="{{ action('BoletinController@show', [$boletin->id]) }}">{{ $boletin->EMail }}</a>
            </h2>
        </boletinmail>
    @endforeach

```

Genera los mismos links que la opción anterior: `/boletin/<id>`

####Generar una url utilizando la función `url()`:

Otra posibilidad, es utilizar la función `url($path, array $extra, $secure);`

Esta función construye una url **absoluta** con los parámetros indicados. Además de comprobar si la url es válida.

```
File: index.blade.php

    @foreach($boletinEMails as $boletin)
        <boletinmail>
            <h2>
                <a href="{{ url('/boletin',[$boletin->id]) }}">{{ $boletin->EMail }}</a>
            </h2>
        </boletinmail>
    @endforeach

```


####Utilizar el nombre de una ruta:

Por último, podemos crear una url a la ruta en lugar de al controlador. Con la función `route('nombreDeLaRuta');`.

Este método lo pueder usar únicamente con aquellas rutas a las que les hayas asignado un nombre.

```
File: index.blade.php

    @foreach($boletinEMails as $boletin)
        <boletinmail>
            <h2>
                <a href="{{ route('boletin_path'),[$boletin->id] }}">{{ $boletin->EMail }}</a>
            </h2>
        </boletinmail>
    @endforeach

```

---------

You may also share a piece of data across all views:
View::share('name', 'Steve');