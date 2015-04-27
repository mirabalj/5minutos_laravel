##Estándares de programación - Resumen.

###Edición

 - Elimina los espacios en blanco al final de las líneas y en las líneas en blanco.

 - Intenta mantener el límite de tus líneas en 80 caracteres.

 - Usa siempre el tag de apertura largo de PHP `<?php`. Sin espacios delante.

 - No uses el tag de cierre de PHP `?>` al final de los ficheros.

 - Indenta con tabuladores y alinea con espacios.

 - Usa sólo `UTF-8 without BOM` para código PHP.  

###Código

 - La declaración del `namespace` **DEBE** estar en la misma línea que `<?php`

 - Deja una línea en blanco después de `namespace` y después de `use`. Escribe sólo un `use` por declaración.

 - La llave de apertura de las clases va en la misma línea que el nombre de la clase. `extends` e `implements` van también en esa línea.

 - Siempre que sean aplicables, usa el prefijo `Abstract` y los sufijos `Interface`, `Trait` y `Exception`.

 - Declara siempre la visibilidad en todas las propiedades y métodos. (No uses un `_` en los nombres para eso).

 - Declara las propiedades de las clases antes de los métodos.

 - No declares más de una propiedad por sentencia. No uses `var` en las propiedades.
 
 - Sigue el orden: `abstract|final <visibilidad> static function()`

 - No uses espacios antes ni después de los paréntesis de los métodos y funciones.
 
 `public function convertException($message, DriverException $exception, force = false) {...}`.

 - Declara los métodos en este orden: `public`, `protected` y `private`. (Excepto los constructores y `setUp` y `tearDown` de PHPUnit)

 - En la lista de argumentos pon espacios sólo después de cada coma. Coloca los argumentos con valores por defecto al final.

 - Usa una línea en blanco antes de los `returns` a no ser que sea la única línea dentro de una estructura. (Como un `if`)

 - Usa el estilo `Allman` en los métodos, funciones y estructuras de control.

 - Usa un espacio después de Las palabras clave de las estructuras de control. Y un espacio entre el paréntesis de cierre y la llave de apertura.
  
 `if ($this->driver->getVisibility($path) == AdapterInterface::VISIBILITY_PUBLIC)`
 
 - Usa `elseif` en lugar de `else if`. 

 - Indenta `case` dentro de `switch` y `break` dentro de `case`. Escribe `// no break` cuando hay `case` en cascada no vacío.

 - En las closures, rodea los paréntesis de `function` y la palabra `use` con espacios. Pon `{` en la misma línea.

 `$listener = function () use (&$invoked) {`

 - Las palabras clave de PHP y las constantes `true`, `false` y `null` van en minúsculas.

 - Usa sólo una 'instrucción' por línea. 

 - Los comentarios de una sola línea colócalos al mismo nivel que la línea a la que se refieren. (Como en el ejemplo anterior del 'if')

 - Utiliza espacios alrededor de los operadores matemáticos y lógicos.

 - Usa los operadores lógicos `&&`y `||` en lugar de `AND` y `OR`.

 - Usa comillas sencillas habitualmente y comillas dobles cuando quieras expandir una variable.

###Procesos

 - En cada fichero declara o bien estructuras y símbolos o bien partes de la lógica secundaria, pero no ambas.

 - Coloca los `returns` al comienzo de las funciones. (Return early)

 - No uses `else` para salir al final de las funciones.

###Nombres

 - Las rutas completas de los Namespaces tienen la estructura `Proveedor\Namespace\NombreDeLaClase`. `(VendorName\Namespace\ClassName)`
 
 `use Illuminate\Database\Schema\Blueprint;`

 
 - Usa `StudlyCaps` para los nombres de las clases. (`MySqlBuilder`, `CreateUsersTable`)
 
 - Usa `camelCase` para los nombres de los métodos. (`getColumnListing()`, `compileTableExists()`)
  
 - Las constantes de las clases van en MAYÚSCULAS usando guiones bajos como separadores. (`DATE_APPROVED`)

 - Usa los siguientes nombres para métodos de objetos con múltiples relaciones y una relación 'principal'.
 
 `set()`, `has()`, `all()`, `get()`, `replace()`, `remove()`, `clear()`, `isEmpty()`, `add()`, `register()`, `count()`, `keys()`

 - Usa los siguientes nombres para métodos de objetos con múltiples relaciones y varias relaciones.
 
 `getXXX()`, `setXXX()`, `replaceXXX()`, `hasXXX()`, `getXXXs()`, `setXXXs()`, `removeXXX()`, `clearXXX()`, `isEmptyXXX()`, `addXXX()`, `registerXXX()`, `countXXX()`

 - Para las rutas, usa nombres afines a las convenciones internas de Laravel.

  `users.index`, `users.create`, `users.store`, `users.show`, `users.edit`, `users.update`, `users.destroy` 

 - Para los modelos Eloquent, usa el nombre de tu tabla en `StudlyCaps` y en singular. Llama `id` a tu clave principal. 
  
  Usa `snake_case` en las demás columnas y `getNombreCompletoAttribute($name)` en los métodos.

 
<br>  
###Más información:
[Guía de Contribución de Laravel - Estilo del Código](http://laravel.com/docs/5.0/contributions#coding-style)   

[Discusión acerca del Estilo de Código del Framework Laravel abierta por GrahamCampbell en GitHub](https://github.com/laravel/framework/issues/6836)   

[Estándar de código de Symfony](http://symfony.com/doc/current/contributing/code/standards.html)

[Convenciones para los nombres de Symfony](http://symfony.com/doc/current/contributing/code/conventions.html)

[Estándar PSR-2](http://www.php-fig.org/psr/psr-2/)  

[Estándar PSR-4](http://www.php-fig.org/psr/psr-4/)  