####Obtener todos los registros:
```
	$boletines = Boletin::all();
```

####Mostrar los últimos o los primeros registros.

Para mostrar los últimos en orden descendente:

```
	$boletin = Boletin::latest();
```

> Por defecto es equivalente a `Boletin::orderBy('created_at', 'desc')`, pero puedes especificar cualquier otra columna.


Para mostrar los primers en orden ascendente:

```
	$boletin = Boletin::oldest();
```

