###Vistas 

Crea una vista para usarla como plantilla:

`layout.blade.php`




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