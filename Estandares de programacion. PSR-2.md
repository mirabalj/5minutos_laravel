##Estándares de programación en PHP (PSR-2)

*TLDR:* Puedes ir directamente a un [Resumen de estos estándares](https://github.com/jatubio/5minutos_laravel/wiki/Estandares-de-programacion.-Laravel).

Esta es la continuación de los estándares de PHP. En el post anterior, hablamos de los estándares [PSR-1 y PSR-4](https://github.com/jatubio/5minutos_laravel/wiki/Estandares-de-programacion.-PSR-1-y-PSR-4).

Hoy vamos a ver el estándar PSR-2 que es el más complejo y más interesante de todos.

El estándar PSR-3 no lo veremos porque se refiere únicamente a sistema de logs. Pero si estás interesado en él, encontrarás un link a la documentación al final del post.

Este estándar también forma parte de los estándares de programación en PHP propuestos por el [php-fig (Grupo de interoperatibilidad para Frameworks PHP)](http://www.php-fig.org).

###PSR-2 - Guía de estilo de código.

Orientado a dar formato al código PHP.
Su objetivo es reducir la dificultad cuando se lee código de diferentes autores. (Y de uno mismo!! jeje)

####Código  

- El código **DEBE** seguir el estándar PSR-1.

- El código **DEBE** usar 4 espacios como indentación, no tabuladores.

 > Nota: Utilizar sólo los espacios, y no mezclar espacios con tabuladores, ayuda a evitar problemas con diffs, parches, historiales y anotaciones. El uso de los espacios también ayuda a ajustar la alineación entre líneas.

- No hay un límite estricto en la longitud de las líneas. El límite **DEBE** estar en 120 caracteres; las líneas deberían tener preferiblemente 80 caracteres o menos.

  - **NO DEBE** haber espacios en blanco al final de las líneas que no estén vacías.

  - **PUEDEN** añadirse líneas en blanco para mejorar la lectura del código y para indicar bloques de código que estén relacionados.

  - **NO DEBE** haber más de una sentencia por línea.

- **DEBE** haber una línea en blanco después de la declaración del `namespace`, y **DEBE** haber una línea en blanco después del bloque de declaraciones `use`.

 **DEBE** haber un `use` por declaración.

	```
    <?php namespace Curso\Http\Controllers;

	use Illuminate\Foundation\Bus\DispatchesCommands;
	use Illuminate\Routing\Controller as BaseController;
	use Illuminate\Foundation\Validation\ValidatesRequests;

	abstract class Controller extends BaseController {...}
```

####Clases  

- Las llaves de apertura de las clases **DEBEN** ir en la línea siguiente, y las llaves de cierre **DEBEN** ir en la línea siguiente al cuerpo de la clase.
```
	class HomeController extends Controller 
	{
		public function __construct()
		{
			$this->middleware('auth');
		}
	}
```

- Las palabras clave `extends` e `implements` **DEBEN** declararse en la misma línea del nombre de la clase.  

- La lista de `implements` **PUEDE** ser dividida en múltiples líneas, indentando las líneas subsiguientes una vez. Al hacerlo, el primer elemento de la lista **DEBE** estar en la línea siguiente, y **DEBE** haber una sola interfaz por línea.

	```
    abstract class AbstractMySQLDriver implements  
		Driver, 
		ExceptionConverterDriver, 
		VersionAwarePlatformDriver
	{
	 (....)
	}
    ```

####Métodos, propiedades y funciones.
	
- Las llaves de apertura de los métodos **DEBEN** ir en la línea siguiente, y las llaves de cierre **DEBEN** ir en la línea siguiente al cuerpo del método.
```
	public function run()
	{
		// Crear los datos
		\DB::table('users')->insert(array(
			'name'	=>	'Juanan',
			'email'=>'jatubio@gmail.com',
			'password'=>\Hash::make('clave')
									 ));
	}
```

- La visibilidad **DEBE** declararse en todas las propiedades y métodos; `abstract` y `final` **DEBEN** declararse antes de la visibilidad. `static` **DEBE** declararse después de la visibilidad.  

- La palabra clave `var` **NO DEBE** ser usada para declarar una propiedad.  

- **NO DEBE** declararse más de una propiedad por sentencia.  

- Los nombres de los métodos y propiedades **NO DEBERÍAN** usar un guión bajo como prefijo para indicar si son privados/as o protegidos/as.  

- Los nombres de métodos **NO DEBEN** estar declarados con un espacio después del nombre del método. La llave de apertura **DEBE** situarse en su propia línea, y la llave de cierre **DEBE** ir en la línea siguiente al cuerpo del método. 

- **NO DEBE** haber ningún espacio después del paréntesis de apertura, y **NO DEBE** haber ningún espacio antes del paréntesis de cierre.  
```
	    abstract protected function doInitialize();
	    public static function fromString($itemValue) {...}
```
  - En la lista de argumentos **NO DEBE** haber un espacio antes de cada coma y **DEBE** haber un espacio después de cada coma.

  - Los argumentos con valores por defecto del método **DEBEN** ir al final de la lista de argumentos.
```
		public function convertException($message, DriverException $exception, force = false) {...}
```
  - La lista de argumentos PUEDE dividirse en múltiples líneas, donde cada línea será indentada una vez. Cuando se dividan de esta forma, el primer argumento **DEBE** estar en la línea siguiente, y **DEBE** haber sólo un argumento por línea.

  - Cuando la lista de argumentos se divide en varias líneas, el paréntesis de cierre y la llave de apertura **DEBEN** estar juntos en su propia línea separados por un espacio.
```  
		public function convertException(
			$message, 
			DriverException $exception, 
			force = false
		) {
		(...)
		}
```  

- Cuando se realize una llamada a un método o a una función, **NO DEBE** haber un espacio entre el nombre del método o la función y el paréntesis de apertura, **NO DEBE** haber un espacio después del paréntesis de apertura, y **NO DEBE** haber un espacio antes del paréntesis de cierre. En la lista de argumentos, **NO DEBE** haber espacio antes de cada coma y **DEBE** haber un espacio después de cada coma.
```
		$this->options = array_merge($this->defaultOptions, $options);
```

 - La lista de argumentos PUEDE dividirse en múltiples líneas, donde cada una se indenta una vez. Cuando esto suceda, el primer argumento **DEBE** estar en la línea siguiente, y **DEBE** haber sólo un argumento por línea.
```	
        if ( ! preg_match(
            '/^(?P<major>\d+)(?:\.(?P<minor>\d+)(?:\.(?P<patch>\d+)(?:\.(?P<build>\d+))?)?)?/',
            $version,
            $versionParts
        )) {
            throw DBALException::invalidPlatformVersionSpecified(
                $version,
                '<major_version>.<minor_version>.<patch_version>.<build_version>'
            );
        }
```		

####Estructuras de control.

- Las palabras clave de las estructuras de control **DEBEN** tener un espacio después de ellas, las llamadas a los métodos y las funciones **NO DEBEN** tenerlo.

- **DEBE** haber un espacio después de una palabra clave de estructura de control. **NO DEBE** haber espacios después del paréntesis de apertura y **NO DEBE** haber espacios antes del paréntesis de cierre.  

- **DEBE** haber un espacio entre el paréntesis de cierre y la llave de apertura. La llave de cierre **DEBE** estar en la línea siguiente al final del cuerpo.
  
- El cuerpo de cada estructura **DEBE** estar encerrado entre llaves. Esto estandariza el aspecto de las estructuras y reduce la probabilidad de añadir errores como nuevas líneas que se añaden al cuerpo de la estructura.

- El cuerpo de la estructura de control **DEBE** estar indentado una vez.

  - `if`, `elseif`, `else`: &nbsp;&nbsp; `else` y `elseif` van en la misma línea que las llaves de cierre del cuerpo anterior. La palabra clave `elseif` **DEBERÍA** ser usada en lugar de `else if` de forma que todas las palabras clave de la estructura estén compuestas por palabras de un solo término.
```
        if ($node instanceof ClassStmt) {
            $this->class = $node;
            $this->abstractMethods = array();
        } elseif ($node instanceof ClassMethod) {
            if ($node->isAbstract()) {
                $name = sprintf('%s::%s', $this->class->name, $node->name);
                $this->abstractMethods[] = $name;

                if ($node->stmts !== null) {
                    throw new FatalErrorException(sprintf('Abstract function %s cannot contain body', $name));
                }
            }
        }
```  
  - `switch`, `case`: La palabra clave `case` **DEBE** estar indentada una vez respecto al `switch` y la palabra clave `break` o cualquier otra palabra clave de finalización **DEBE** estar indentada al mismo nivel que el cuerpo del `case`. **DEBE** haber un comentario como `// no break` cuando hay `case` en cascada no vacío.
  ```
		switch ($expr) {
			case 0:
				echo 'Primer case con break';
				break;
			case 1:
				echo 'Segundo case sin break en cascada';
				// no break
			case 2:
			case 3:
			case 4:
				echo 'Tercer case; con return en vez de break';
				return;
			default:
				echo 'Case por defecto';
				break;
		}
```  
  - `while`, `do while`.
```  
        while ($item = array_shift($listing)) {
            if (preg_match('#^.*:$#', $item)) {
                $base = trim($item, ':');
                continue;
            }
```
```
        do {
            $this->directory = sys_get_temp_dir() . '/doctrine_cache_'. uniqid();
        } while (file_exists($this->directory));
```
  - `for`.
```  
        for ($i = $offset; $i <= $to; $i+= $stepSize) {
            if ($i == $dateValue) {
                return true;
            }
        }
```
  - `foreach`.
```  
        foreach ($constants as $name => $value) {
            if ($value === $token) {
                return $className . '::' . $name;
            }
        }
```  
  - `try`, `catch`.
```
        try {
            do {
                $line = $this->_buffer->readLine($seq);
                $response .= $line;
            } while (null !== $line && false !== $line && ' ' != $line{3});
        } catch (Swift_TransportException $e) {
            $this->_throwException($e);
        } catch (Swift_IoException $e) {
            $this->_throwException(
                new Swift_TransportException(
                    $e->getMessage())
                );
        }
```

####Closures.

- Las closures **DEBEN** declararse con un espacio después de la palabra clave `function`, y un espacio antes y después de la palabra clave `use`.

- La llave de apertura **DEBE** ir en la misma línea, y la llave de cierre **DEBE** ir en la línea siguiente al final del cuerpo.

- **NO DEBE** haber un espacio después del paréntesis de apertura de la lista de argumentos o la lista de variables, y **NO DEBE** haber un espacio antes del paréntesis de cierre de la lista de argumentos o la lista de variables.

- En la lista de argumentos y la lista de variables, **NO DEBE** haber un espacio antes de cada coma, y **DEBE** haber un espacio después de cada coma.

- Los argumentos de las closures con valores por defecto, **DEBEN** ir al final de la lista de argumentos.
```
        $route->setDefault('_controller', $closure = function () { return 'Hello'; });
        
        $this->assertEquals($closure, $route->getDefault('_controller'), '->setDefault() sets a default value');
```
```
        $listener = function () use (&$invoked) {
            $invoked++;
        };		
```

- La lista de argumentos y la lista de variables **PUEDEN** ser divididas en múltiples líneas, donde cada nueva línea se indentará una vez. Cuando esto suceda, el primer elemento de la lista **DEBE** ir en una nueva línea y **DEBE** haber sólo un argumento o variable por línea.

- Cuando la lista de argumentos o variables se divide en varias líneas, el paréntesis de cierre y la llave de apertura **DEBEN** estar juntos en su propia línea separados por un espacio.
```
        $returnFirstCallCallback = function (
			$calls, 
			$object, 
			$method
		) {
            throw new RuntimeException;
        };
```

####Palabras clave.

- Las palabras clave de PHP **DEBEN** estar en minúsculas. Las constantes de PHP `true`, `false` y `null` **DEBEN** estar en minúsculas.
	
###Fuentes y más información:

[Estándares de programación en PHP (PSR-1 y PSR-4)](https://github.com/jatubio/5minutos_laravel/wiki/Estandares-de-programacion.-PSR-1-y-PSR-4)

<a href="https://styde.net/laravel-5/">Curso de Laravel 5 en español desde cero</a>:  <a href="https://styde.net/curso-de-laravel-5-que-es-psr-4-y-uso-de-los-namespaces/">Introducción: PSR-4 y Namespaces</a>.  

[php-fig - Grupo de interoperatibilidad para Frameworks PHP](http://www.php-fig.org)  

[Estándar PSR-1](http://www.php-fig.org/psr/psr-1/)  

[Estándar PSR-2](http://www.php-fig.org/psr/psr-2/)  

[Estándar PSR-3](http://www.php-fig.org/psr/psr-3/)  

[Estándar PSR-4](http://www.php-fig.org/psr/psr-4/)  

[Ejemplos de implementación de PSR-4](http://www.php-fig.org/psr/psr-4/examples/)  