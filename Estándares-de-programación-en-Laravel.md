##Estándares de programación en Laravel

Después de ver los estándares PSR-1, PSR-2 y PSR-4 en los posts anteriores, llegamos a los estándares recomendados en Laravel.

###Laravel - Guía de estilo de código.

Laravel sigue los estándares PSR-1 y PSR-4. Y además tiene algunas recomendaciones propias. Lo que en algunos entornos llaman el '_Laravel "flavor" of PSR-2_'. Estos son los estándares extraídos de su guía de colaboración.

He comentado después de cada estándar y resaltado en negrita los cambios con respecto al estándar PSR-2.

> Si usas PhpStorm, al final de este post tienes un link para descargarte un fichero `.xml` para configurar el 'Estilo de código Laravel'.

- La declaración del `namespace` **DEBE** estar en la misma línea que `<?php`
 	```
    <?php namespace Curso\Http\Controllers;
	```

 * No existe en los estándares PSR una regla acerca de esto.

<br>
- La llave de apertura de las clases **DEBEN** ir en la **misma línea** que el nombre de la clase.

	```
	class SQLiteConnection extends Connection {

		protected function getDefaultQueryGrammar()
		{
			return $this->withTablePrefix(new QueryGrammar);
		}

		(...)
	```

 * En PSR-2 recomiendan que la llave vaya en la línea siguiente.

<br>
- Las funciones y estructuras de control deben seguir el estilo de llaves `Allman`.

  El estilo `Allman` define que la llave de apertura de las estructuras de control **DEBE** ir en la **línea siguiente**. La llave de cierre **DEBE** estar al mismo nivel que la de apertura. Y el cuerpo de la estructura **DEBE** estar indentado.

	```
	(...)

	if ($previous instanceof PDOException)
    {
	    $this->errorInfo = $previous->errorInfo;
    }

	(...)
	```

 * En PSR-2 recomiendan que la llave de apertura vaya en la misma línea separada por un espacio del paréntesis de cierre de la declaración.

<br>
- Indentar con **tabuladores** y alinear con espacios.

 * En PSR-2 recomiendan indentar con espacios.

<br>
> Si usas PHP-Sniffer, este paquete te permite comprobar si tu código sigue los estándares de Laravel y reformatea tu código para que sea compatible con ellos: [Laravel PHP_CodeSniffer](https://github.com/antonioribeiro/laravelcs)

<br>
> Este es otro paquete que te ayuda a formatear tu código para que siga los estándares PSR-1 y PSR-2 y GrahamCampbell está entre uno de sus colaboradores: [FriendsOfPHP/PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)

<br>
###Otras convenciones de código recomendadas.

Estas convenciones no están 'oficialmente' recogidas en estándares. Pero te facilitarán mucho la vida como programador de PHP y te ayudarán a evitar algunos errores. Complementan y amplían las convenciones explicadas hasta ahora.

Están basadas en 'estándares de facto', en las recomendaciones de Symfony y en el código del Framework de Laravel.

 - Usa siempre el tag de apertura largo de PHP `<?php`. No uses el tag corto `<?`.
  
  - No dejes espacios antes del tag de apertura.
  
  Entre otros motivos, el tag corto es incompatible con las especificaciones de XML.
 
 - No uses el tag de cierre de PHP `?>` al final de los ficheros.
 
  Es opcional. Y cuando lo usas, si dejas espacios o líneas en blanco después de él, esos espacios pueden ir a la salida resultante del script y generar errores.
  
  Créeme, a mí me ha pasado alguna vez y es un auténtico incordio tener que revisar decenas de ficheros .php buscando espacios en blanco después del tag de cierre.
  
 - Intenta mantener el límite de tus líneas en 80 caracteres.
 
  Si es necesario, puedes dividir las líneas en varias e indentarlas, ya que PHP lo soporta perfectamente.

  Ganarás en facilidad de lectura y comprensión de tu código.

 - Declara las propiedades de las clases antes de los métodos.
 
 - Declara los métodos en este orden: `public`, `protected` y `private`.
 
   - A excepción de los constructores y los métodos `setUp` y `tearDown` de los tests PHPUnit que deben ir primero para mejorar la lectura.

 - Siempre que sean aplicables, usa el prefijo `Abstract` y los sufijos `Interface`, `Trait` y `Exception`.
   
 - Usa sólo una 'instrucción' por línea. (PSR-2)

 No uses los `;` para unir varias instrucciones.
 
 Incluso los `if()` más sencillos escríbelos en varias líneas.

	```
    // Haz esto:
	if (strlen($path) === 0) {
		return $this->getPathPrefix() ?: '';
	}

	// En lugar de esto:
	if (strlen($path) === 0) { return $this->getPathPrefix() ?: ''; }    
	```

 Una excepción es el 'operador condicional ternario' `a ? b : c` que sí se puede usar en un sola línea o dividir en varias en caso de que las expresiones sean muy complejas, para facilitar su lectura.  
 
	```
	$this->pathPrefix = $is_empty ? null : $prefix;
	```

 - Los comentarios de una sola línea colócalos al mismo nivel que la línea a la que se refieren. (Como en el ejemplo anterior del 'if')
 
 - Elimina los espacios en blanco al final de las líneas y en las líneas en blanco. (PSR-2)
 
  Especialmente si usas algún sistema de versión de código, lo agradecerás.
  
 - Utiliza espacios para mejorar la lectura de los operadores:
 
  - Después del signo de negación `!`.
  
	```
	if (! isset($config[$setting])) {
		continue;
	}
	```
  - Antes y después del signo igual `=`, de los operadores matemáticos y lógicos y de los signos del operador condicional ternario.

	```
	public function setHost($host)
	{
		$this->host = $host;

		return $this;
	}
	```
 
 - Usa comillas sencillas habitualmente y comillas dobles cuando quieras expandir una variable.

 - Usa los operadores lógicos `&&` y `||` en lugar de `AND` y `OR`.
 
 - Usa una línea en blanco antes de los `returns` a no ser que sea la única línea dentro de una estructura. (Como un `if`)
 
 - Coloca los `returns` al comienzo de las funciones. (Return early)
 
   Especialmente en los casos en los que hagas validaciones con un `return`, hazlos al comienzo de la función. De ese modo es más fácil leer lo que hace la función y cuales son sus condiciones de salida.
   
 - No uses `else` para salir al final de las funciones.
 
	```
	// Haz esto
    public function getMimetype($path)
    {
        $path = Util::normalizePath($path);
        $this->assertPresent($path);

        if (! $object = $this->adapter->getMimetype($path)) {
            return false;
        }

        return $object['mimetype'];
    }
	
	// En lugar de esto
    public function getMimetype($path)
    {
        $path = Util::normalizePath($path);
        $this->assertPresent($path);

        if ($object = $this->adapter->getMimetype($path)) {
            return $object['mimetype'];
        } else {
			return false;
		}		
    }
	``` 

 - Para los nombres de los métodos:

  Cuando un objeto tiene una relación principal con varias 'cosas', como objetos, parámetros, etc... se usan estos nombres:

   * ``set()``  

   * ``has()``  

   * ``all()``  

   * ``get()``  

   * ``replace()``  

   * ``remove()``  

   * ``clear()``  

   * ``isEmpty()``  

   * ``add()``  

   * ``register()``  

   * ``count()``  

   * ``keys()``  

  Para relaciones a varios dónde la relación no sea principal, se aplican estos otros nombres:
  
| Relación Principal&nbsp;&nbsp;| Otras Relaciones  |
| ------------------      | ----------------- |
| ``get()``           | ``getXXX()``      |
| ``set()``           | ``setXXX()``      |
| n/a                 | ``replaceXXX()``  |
| ``has()``           | ``hasXXX()``      |
| ``all()``           | ``getXXXs()``     |
| ``replace()``       | ``setXXXs()``     |
| ``remove()``        | ``removeXXX()``   |
| ``clear()``         | ``clearXXX()``    |
| ``isEmpty()``       | ``isEmptyXXX()``  |
| ``add()``           | ``addXXX()``      |
| ``register()``      | ``registerXXX()`` |
| ``count()``         | ``countXXX()``    |
| ``keys()``          | n/a               |
  

  > Aunque `setXXX` y `replaceXXX` se parecen, la diferencia es que mientras `setXXX` puede modificar o añadir elementos a la relación, `replaceXXX` no puede añadir elementos. Y si recibe una clave desconocida, debe lanzar una excepción.

 - Nombres de las rutas.
 
 Usa nombres en línea con las convenciones internas de Laravel:

	* `users.index`

	* `users.create`

	* `users.store`

	* `users.show`

	* `users.edit`

	* `users.update`

	* `users.destroy`  
  
 - Nombres de los modelos Eloquent.
 
 Laravel usa como convención para los modelos la notación `StudlyCaps` y para el nombre de la tabla relacionada con tu modelo, el nombre del modelo en minúsculas y plural.
 
 Por ejemplo, para `class User extends Eloquent {}`, Laravel buscará una tabla llamada `users`.
 
 Y como convención, usará como clave primaria un campo llamado `id`.
 
 Para los nombres de las columnas en la tabla, usa la notación `snake_case`. Y para los métodos de los modelos, usa `camelCase`. Por ejemplo, para una columna llamada `nombre_completo`, el método sería `getNombreCompletoAttribute($name)`.
 
 Para las tablas pivots, el convenio es usar el singular de ambas tablas unidas por un guión bajo. Por ejemplo: `user_product`.
 
<br>
###Fuentes y más información:

[Estándares de programación en PHP (PSR-1 y PSR-4)](/Est%C3%A1ndares-de-programaci%C3%B3n-PSR-1-y-PSR-4)   

[Estándares de programación en PHP (PSR-2)](/Est%C3%A1ndares-de-programaci%C3%B3n-PSR-2)

[Guía de Contribución de Laravel - Estilo del Código](http://laravel.com/docs/5.0/contributions#coding-style)   

[Discusión acerca del Estilo de Código del Framework Laravel abierta por GrahamCampbell en GitHub](https://github.com/laravel/framework/issues/6836)   

[Estándar de código de Symfony](http://symfony.com/doc/current/contributing/code/standards.html)

[Convenciones para los nombres de Symfony](http://symfony.com/doc/current/contributing/code/conventions.html)

[StyleCI - The PHP Coding Style Continuous Integration Service](https://github.com/StyleCI)  

[FriendsOfPHP/PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)  

[Laravel PHP_CodeSniffer](https://github.com/antonioribeiro/laravelcs)  

[PhpStorm Laravel Code Style](https://github.com/deringer/phpstorm-laravel-code-style)