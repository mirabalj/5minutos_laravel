###Controladores

Para utilizar un controlador hay que registrarlo en alguna vista. Habitualmente, se hace en el fichero `app/Http/routes.php` con una línea como esta:

```
Route::get('boletin','BoletinController@index');
Route::get('boletin/{id}','BoletinController@show');
```

####Mostrar todos los registros de un modelo:

```
    public function index()
    {
        return Boletin::all();
    }
```

####Pasar esos registros a una vista:

```
    public function index()
    {

		$boletines = Boletin::all();

		return view('boletin.index')->with('boletines', $boletines);
    }
```

> Puedes utilizar también la función `compact()`.
>
> `return view('boletin.index')->with(compact('boletines');`
>
> Esa función convierte las variables cuyos nombres son pasados como cadenas y sus valores en arrays.
>
> Por ejemplo, en lugar de escribir: 
> `return view('home')->with(array('products' => $products, 'title' => $title, 'filter' => $filter'));`
>
> Podrías escribir:
> `return view('home')->with(compact('products', 'title', 'filter'));`

####Comprobar si existe un registro:

Mostrando un error http '404':

```
    public function show($id)
    {
	    $boletinEMail=Boletin::find($id);

	    if(is_null($boletinEMail))
	    {
		    abort(404, 'No existe ese e-mail en el boletín');
	    }

	    return view('boletin.show',compact('boletinEMail'));
    }
```

Lanzando una Excepción: `ModelNotFoundException` 

```
    public function show($id)
    {
	    $boletinEMail=Boletin::findOrFail($id);

	    return view('boletin.show',compact('boletinEMail'));
    }
```

####Recuperar los datos de un formulario (POST)

Podemos usar `Request::all()` para obtener todos los datos de las variables globales de PHP `$_GET` y `$_POST`. 

```
public function store()
{
    $input = Request::all();
	
	$boletin = new Boletin();
	$boletin->email = $input['email'];
	$boletin->save();
}
```

> Puedes usar también `$boletin = new Boletin($input);` o `$boletin = new Boletin('email' => $input['email']);`
> 
> Ten en cuenta que cuando se asignan valores a las propiedades directamente, no se comprueban las propiedades `$fillable` ni `$guarded` del modelo.

o 

```
public function store()
{
    $input = Request::all();
	
	Boletin::Create($input);
}
```

> Usa `$input = Request::get('email')` para obtener sólo un campo.

No debes preocuparte por las posibles inyecciones de SQL porque Eloquent se encargará de filtrar el `input()` antes de ejecutar la consulta SQL.

#### `Non-static method Illuminate\Http\Request::all() should not be called statically`

```
ErrorException in BoletinController.php line 38:
Non-static method Illuminate\Http\Request::all() should not be called statically, assuming $this from incompatible context
```

Se debe a que por defecto, en los controladores se importa el Namespace: `Illuminate\Http\Request`. Pero si hacemos uso del método `Request::all()`, ese método no es de la clase `Illuminate\Http\Request` sino de la Facade `Request`. 

Por tanto, hay que sustituir la línea:

```
use Illuminate\Http\Request;
```

Por

```
use Request;
```

> La Facade `Request` está definida en los `alias` del fichero `config/app.php`: 
> ```
> 'Request'   => 'Illuminate\Support\Facades\Request',
> ```