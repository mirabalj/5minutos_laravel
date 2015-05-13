###Inyección de Dependencias

La inyección de dependencia es un patrón que se utiliza para crear acoplamientos débiles (__loosely coupled__) entre clases.

Cuando usamos inyección de dependencias, sustituimos una instancia directa de una clase (habitualmente en forma de llamada al constructor de esa clase) por un nuevo parámetro en el método para recibir una interface.

Vamos a ver un ejemplo. Imagina que tenemos el siguiente código:

```
class DropBox {

	public function getConfig()
	{
		(...)
	}

	public function setConfig()
	{
		(...)
	}
}

class DatabaseSystem {

	public function openConnection()
	{
		$dropbox = new DropBox();
		$this->openNewConnection($dropbox->getConfig());
	}
}

$database = new DatabaseSystem();
$database -> openConnection();

```

La clase `DatabaseSystem` está __fuertemente acoplada__ a la clase `DropBox` con los siguientes inconvenientes:

- Si quisiéramos guardar la configuración en otro sistema de almacenamiento, tendríamos que modificar el código de la clase `DatabaseSystem`.
- No es posible cambiar el sistema de almacenamiento de forma dinámica durante la ejecución de la aplicación. Por ejemplo, mediante un fichero de configuración.
- Para probar la clase `DatabaseSystem`, tenemos que probar también la clase `DropBox`.
- Si cambiasen los parámetros del constructor de la clase `DropBox`, tendríamos que modificar el código de la clase `DatabaseSystem` para añadir esos parámetros.
- Dado que la dependencia se realiza en el código interno de un método, no es fácil recordar que existe esa dependencia o incluso es posible que otro programador no sea consciente de ella.

Vamos a ver paso a paso, distintas formas de implementar este patrón:

####Inyección de dependencias en el método

```
class DropBox {

	public function getConfig()
	{
		(...)
	}

	public function setConfig()
	{
		(...)
	}
}

class DatabaseSystem {


	public function openConnection(Dropbox $dropBox)
	{
		$this->openNewConnection($dropbox->getConfig());
	}
}

$database = new DatabaseSystem();
$database->openConnection(new DropBox());
```

En este primer paso, hemos conseguido:

- Ahora la dependencia de la clase `DatabaseSystem` respecto de `DropBox` es pública.
- Podemos configurar la instancia de `DropBox` antes de pasársela a la clase `DatabaseSystem`.

Pero la clase `DatabaseSystem` sigue esperando un objeto de tipo `DropBox`. Por lo que aún no podemos utilizar otro método de almacenamiento alternativo.

Vamos a dar el siguiente paso, usar una interface:

```
interface CloudStorage {

	public function getConfig();

	public function setConfig(array $config=[]);
}

class DropBox implements CloudStorage {

	public function getConfig()
	{
		(...)
	}

	public function setConfig(array $config=[])
	{
		(...)
	}
}

class GoogleDrive implements CloudStorage {

	public function getConfig()
	{
		(...)
	}

	public function setConfig(array $config=[])
	{
		(...)
	}
}

class DatabaseSystem {

	public function openConnection(CloudStorage $cloudStorage)
	{
		$this->openNewConnection($cloudStorage->getConfig());
	}
}

$database = new DatabaseSystem();
$database->openConnection(new GoogleDrive());
```

En este código hemos declarado una interface con los métodos que deben implementar todas las clases de tipo `CloudStorage`. Hemos implementado esa interface en nuestra clase `DropBox` y hemos creado una nueva clase que nos permita utilizar el sistema de almacenamiento de GoogleDrive.

Ahora, en el método `openConnection` de la clase `DatabaseSystem`, recibimos un objeto de tipo `CloudStorage`. Lo que nos permite recibir cualquier objeto que implemente esa interface. Hemos cambiado una __dependencia fuerte__ con la clase `DropBox` a una __dependencia débil__ con el interface `CloudStorage`.

Y como puedes ver en la última línea, podemos utilizar el sistema de almacenamiento que queramos, como por ejemplo, `GoogleDrive`.

Incluso, podríamos crear una clase que nos devuelva una instancia de tipo `CloudStorage` en función de un parámetro en el fichero de configuración de la aplicación. De modo que podríamos escribir una línea como esta:

```
$database = new DatabaseSystem();
$database->openConnection(getStorageFromConfigFile());
```

Qué hemos conseguido:

- Ahora la dependencia de la clase `DatabaseSystem` respecto de `CloudStorage` es pública y débil.
- Podemos modificar el sistema de almacenamiento dinámicamente durante la ejecución de la aplicación.
- Podríamos declarar un Mock (clase de pruebas) que implemente `CloudStorage` y utilizarlo en nuestras pruebas cuando no tengamos conexión a internet o para tener un entorno de pruebas controlado.


####Inyección de dependencias en el constructor.

Otra posibilidad, es inyectar la dependencia en el constructor en lugar de en el método. 

Las ventajas de utilizar este método son:

- El código que declara la dependencia está más focalizado. Lo que facilita su modificación y pasarle los tests. 

 Habitualmente se suele instanciar el objeto sólo en algunos lugares muy concretos de la aplicación. Mientras que a un método es posible que lo llamemos desde muchos más puntos. Por tanto, a la hora de modificar ese código, es más fácil y hay que buscar en menos puntos de la aplicación si hemos utilizado la inyección en el constructor.
 
- Si utilizamos el objeto en varios métodos de nuestra clase, no estaríamos repitiendo código al declarar el mismo parámetro en cada uno de esos métodos.


Imaginemos que en el ejemplo anterior, tenemos también un método que para actualizar la configuración de la conexión. En ese caso, sería más eficiente implementar la inyección en el constructor: (Para que el ejemplo no sea muy largo, no voy a incluir el código del interface `CloudStorage` y las clases `DropBox` y `GoogleDrive` dado que se mantendría igual).


```
class DatabaseSystem {

	protected $cloudStorage;

	public function __construct(CloudStorage $cloudStorage)
	{
		$this->$cloudStorage = $cloudStorage;
	}

	public function openConnection()
	{
		$this->openNewConnection($this->cloudStorage->getConfig());
	}

	public function updateConnection(array $config = [ ])
	{
		$this->cloudStorage->setConfig($config);
	}
}

$database = new DatabaseSystem(new GoogleDrive());
$database->openConnection();
```

Como ves, ahora recibimos la instancia en el constructor, la guardamos en un campo protected y, de ese modo, podemos usarla en cualquier método que la necesite.

####Inyección de dependencias con `getters y setters`.

Si una clase tiene varias dependencias y no queremos que el constructor sea muy complejo, y, a la vez, esas dependencias son usadas en varios métodos, podemos utilizar una variante del método anterior que consiste en implementar métodos `get() y set()` para establecer y acceder a la instancia de la que dependemos.

Si modificamos el ejemplo anterior, quedaría así:

```
class DatabaseSystem {

	protected $cloudStorage;

	public function setCloudStorage(CloudStorage $cloudStorage)
	{
		$this->$cloudStorage = $cloudStorage;
	}

	public function getCloudStorage()
	{
		return $this->cloudStorage;
	}

	public function openConnection()
	{
		$this->openNewConnection($this->getCloudStorage()->getConfig());
	}

	public function updateConnection(array $config = [ ])
	{
		$this->getCloudStorage()->setConfig($config);
	}
}

$database = new DatabaseSystem();
$database->setCloudStorage(new GoogleDrive());
$database->openConnection();
```

Este método requiere algo más de código que el anterior. Pues antes de acceder a la instancia de `$cloudStorage`, deberíamos comprobar siempre que es una instancia válida. Por ejemplo, que no se ha llamado a `openConnection()` sin haber usado antes el método `setCloudStorage()` para establecer una instancia válida a un objeto del tipo `CloudStorage`.


