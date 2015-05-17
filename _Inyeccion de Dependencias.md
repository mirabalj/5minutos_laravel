###Inyecci�n de Dependencias

La inyecci�n de dependencia es un patr�n que se utiliza para crear acoplamientos d�biles (__loosely coupled__) entre clases.

Cuando usamos inyecci�n de dependencias, sustituimos una instancia directa de una clase (habitualmente en forma de llamada al constructor de esa clase) por un nuevo par�metro en el m�todo para recibir una interface.

Vamos a ver un ejemplo. Imagina que tenemos el siguiente c�digo:

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

La clase `DatabaseSystem` est� __fuertemente acoplada__ a la clase `DropBox` con los siguientes inconvenientes:

- Si quisi�ramos guardar la configuraci�n en otro sistema de almacenamiento, tendr�amos que modificar el c�digo de la clase `DatabaseSystem`.
- No es posible cambiar el sistema de almacenamiento de forma din�mica durante la ejecuci�n de la aplicaci�n. Por ejemplo, mediante un fichero de configuraci�n.
- Para probar la clase `DatabaseSystem`, tenemos que probar tambi�n la clase `DropBox`.
- Si cambiasen los par�metros del constructor de la clase `DropBox`, tendr�amos que modificar el c�digo de la clase `DatabaseSystem` para a�adir esos par�metros.
- Dado que la dependencia se realiza en el c�digo interno de un m�todo, no es f�cil recordar que existe esa dependencia o incluso es posible que otro programador no sea consciente de ella.

Vamos a ver paso a paso, distintas formas de implementar este patr�n:

####Inyecci�n de dependencias en el m�todo

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

- Ahora la dependencia de la clase `DatabaseSystem` respecto de `DropBox` es p�blica.
- Podemos configurar la instancia de `DropBox` antes de pas�rsela a la clase `DatabaseSystem`.

Pero la clase `DatabaseSystem` sigue esperando un objeto de tipo `DropBox`. Por lo que a�n no podemos utilizar otro m�todo de almacenamiento alternativo.

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

En este c�digo hemos declarado una interface con los m�todos que deben implementar todas las clases de tipo `CloudStorage`. Hemos implementado esa interface en nuestra clase `DropBox` y hemos creado una nueva clase que nos permita utilizar el sistema de almacenamiento de GoogleDrive.

Ahora, en el m�todo `openConnection` de la clase `DatabaseSystem`, recibimos un objeto de tipo `CloudStorage`. Lo que nos permite recibir cualquier objeto que implemente esa interface. Hemos cambiado una __dependencia fuerte__ con la clase `DropBox` a una __dependencia d�bil__ con el interface `CloudStorage`.

Y como puedes ver en la �ltima l�nea, podemos utilizar el sistema de almacenamiento que queramos, como por ejemplo, `GoogleDrive`.

Incluso, podr�amos crear una clase que nos devuelva una instancia de tipo `CloudStorage` en funci�n de un par�metro en el fichero de configuraci�n de la aplicaci�n. De modo que podr�amos escribir una l�nea como esta:

```
$database = new DatabaseSystem();
$database->openConnection(getStorageFromConfigFile());
```

Qu� hemos conseguido:

- Ahora la dependencia de la clase `DatabaseSystem` respecto de `CloudStorage` es p�blica y d�bil.
- Podemos modificar el sistema de almacenamiento din�micamente durante la ejecuci�n de la aplicaci�n.
- Podr�amos declarar un Mock (clase de pruebas) que implemente `CloudStorage` y utilizarlo en nuestras pruebas cuando no tengamos conexi�n a internet o para tener un entorno de pruebas controlado.


####Inyecci�n de dependencias en el constructor.

Otra posibilidad, es inyectar la dependencia en el constructor en lugar de en el m�todo. 

Las ventajas de utilizar este m�todo son:

- El c�digo que declara la dependencia est� m�s focalizado. Lo que facilita su modificaci�n y pasarle los tests. 

 Habitualmente se suele instanciar el objeto s�lo en algunos lugares muy concretos de la aplicaci�n. Mientras que a un m�todo es posible que lo llamemos desde muchos m�s puntos. Por tanto, a la hora de modificar ese c�digo, es m�s f�cil y hay que buscar en menos puntos de la aplicaci�n si hemos utilizado la inyecci�n en el constructor.
 
- Si utilizamos el objeto en varios m�todos de nuestra clase, no estar�amos repitiendo c�digo al declarar el mismo par�metro en cada uno de esos m�todos.


Imaginemos que en el ejemplo anterior, tenemos tambi�n un m�todo que para actualizar la configuraci�n de la conexi�n. En ese caso, ser�a m�s eficiente implementar la inyecci�n en el constructor: (Para que el ejemplo no sea muy largo, no voy a incluir el c�digo del interface `CloudStorage` y las clases `DropBox` y `GoogleDrive` dado que se mantendr�a igual).


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

Como ves, ahora recibimos la instancia en el constructor, la guardamos en un campo protected y, de ese modo, podemos usarla en cualquier m�todo que la necesite.

####Inyecci�n de dependencias con `getters y setters`.

Si una clase tiene varias dependencias y no queremos que el constructor sea muy complejo, y, a la vez, esas dependencias son usadas en varios m�todos, podemos utilizar una variante del m�todo anterior que consiste en implementar m�todos `get() y set()` para establecer y acceder a la instancia de la que dependemos.

Si modificamos el ejemplo anterior, quedar�a as�:

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

Este m�todo requiere algo m�s de c�digo que el anterior. Pues antes de acceder a la instancia de `$cloudStorage`, deber�amos comprobar siempre que es una instancia v�lida. Por ejemplo, que no se ha llamado a `openConnection()` sin haber usado antes el m�todo `setCloudStorage()` para establecer una instancia v�lida a un objeto del tipo `CloudStorage`.


