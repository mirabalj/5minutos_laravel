####Prioridades de las expresiones para definir las rutas.

Hay que tener en cuenta que la prioridad en la que se aplican los filtros a las rutas es por el orden en el que se han declarado.
Por tanto, aquellas expresiones menos genéricas deben declararse antes que las más genéricas. Ya que de lo contrario, nunca llegarán a asociarse.
Por ejemplo, en este orden:

```
Route::get('boletin','BoletinController@index');
Route::get('boletin/{id}','BoletinController@show');
Route::get('boletin/create','BoletinController@create');
```

Cuando en el navegador visitemos `/boletin/create` en realidad se llamará al método `show('create')` del controlador `BoletinController`, como ves, pasándole `create` como si fuera un `id`.

Por tanto, el orden correcto sería:

De ese modo, `/boletin/create` será enviado a `BoletinController@create` y cualquier otra ruta que comience por `/boletin/<expresion>` será enviada a `BoletinController@show`.

```
Route::get('boletin','BoletinController@index');
Route::get('boletin/create','BoletinController@create');
Route::get('boletin/{id}','BoletinController@show');
```

####Rutas con el método `Post`.

Para los formularios, usaremos el método `post`:

```
Route::get('boletin/post','BoletinController@post');
```

####Error si llamas a una ruta no definida

```
Whoops, looks like something went wrong.

1/1
MethodNotAllowedHttpException in RouteCollection.php line 207:
in RouteCollection.php line 207
at RouteCollection->methodNotAllowed(array('GET', 'HEAD')) in RouteCollection.php line 194
at RouteCollection->getRouteForMethods(object(Request), array('GET', 'HEAD')) in RouteCollection.php line 142
at RouteCollection->match(object(Request)) in Router.php line 744
at Router->findRoute(object(Request)) in Router.php line 652
```