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

