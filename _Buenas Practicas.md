http://williamdurand.fr/2013/07/30/from-stupid-to-solid-code/
###La caja negra: La importancia de la 'discreci�n'

En OOP, un principio muy importante es el que se suele llamar como 'La Caja Negra' o encapsulamiento. Ese principio es un met�fora de las cajas negras de los aviones. Y recomienda que por defecto las clases mantengan todo el c�digo posible de forma privada. Exponiendo p�blicamente �nicamente aquellas propiedades y m�todos que sean imprescindibles para usar la clase.

De este modo, haremos nuestra clase m�s mantenible, flexible y tolerante a modificaciones.

Yo lo suelo llamar tambi�n el principio de la 'discreci�n': Imagina que tus clases van a una fiesta o a una entrevista de trabajo, deber�an ser lo m�s discretas posibles sobre s� mismas y contar s�lo aquello que consideren imprescindible.

####Utiliza __getters y setters__ para acceder a las propiedades o campos de las clases.

En lugar de exponer tus campos como p�blicos, utilizar m�todos getters y setters para acceder a ellos. De ese modo, podr�s validarlos y procesarlos antes de devolverlos o modificar su valor.




###Divide y vencer�s

- Mant�n tu c�digo simple.

 Esta es la primera clave y la m�s importante. Una frase ideal para recordarla es: 'Divide y vencer�s': Divide tu c�digo complejo en varios m�todos m�s simples. De ese modo ser� menos propenso a errores y cuando estos aparezcan ser� mucho m�s f�cil encontrarlos y solucionarlos.
 
 - Mant�n simples tus m�todos. 
 
  Un m�todo deber�a tener las m�nimas l�neas posibles. Incluso s�lo una si es posible.
  
  Un m�todo con m�s de 15 o 20 l�neas puede ser una se�al de que es demasiado complejo.
 

###El Principio de Responsabilidad Simple

- Aplica SRP a tus clases.
  
  Si cuando describes lo que hace tu clase, usas la palabra 'y' m�s de tres veces, probablemente es una perfecta candidata a una refactorizaci�n.
  
  Otra se�al de que una clase tiene demasiadas responsabilidades es que te sea dif�cil poner un nombre a tu clase.
  
http://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html
###Polimorfismo

- Utiliza el polimorfismo.
 
  El polimorfismo en OOP es la posibilidad de llamar al mismo m�todo en distintas clases. Para ello, hay dos m�todos que se usan habitualmente:
  
  - Utilizar una `Interface` que deben implementar esas clases y, de ese modo, 'cumplen el contrato' propuesto en la interface. En resumen: Si tengo varias clases que tienen el m�todo `clearData()`, defino una interface que declare ese m�todo e implemento esa interface en esas clases. De ese modo, tengo la certeza de que en mi c�digo puedo llamar al m�todo  `clearData()` de cualquiera de esas clases.
  
  - Utilizar una clase abstracta de la que heredar�n esas clases y que define los m�todos comunes que tendr�n esas clases.
 
  Un ejemplo claro de posibilidad de refactorizaci�n utilizando polimorfismo es en una estructura `switch`. Imagina que tienes el siguiente c�digo:
  
```

function getPermissions()
{
	$roles = new Roles();

	$adminPermissions = $roles->getPermissions('admin');
	$userPermissions  = $roles->getPermissions('user');
}

class Roles {

	public function getPermissions($role)
	{
		switch ($role)
		{
			case 'admin':
				return [ 'create', 'update', 'delete' ];
			case 'users':
				return [ 'create', 'update' ];
		}
	}
}
```

Vamos a refactorizar ese `switch` en dos pasos:

En el primer paso aplicamos SRP y separamos cada caso del `switch` en un m�todo:

```

function getPermissions()
{
	$roles = new Roles();

	$adminPermissions = $roles->getAdminPermissions();
	$userPermissions  = $roles->getUserPermissions();
}

class Roles {

	public function getAdminPermissions()
	{
		return [ 'create', 'update', 'delete' ];
	}

	public function getUserPermissions()
	{
		return [ 'create', 'update' ];
	}
}
```

Y, en el segundo paso, utilizamos el polimorfismo para crear una clase para cada Role y una clase abstracta com�n de la que heredar�n ambas:

```
function getPermissions()
{
	$adminRol         = new AdminRol();
	$adminPermissions = $adminRol->getPermissions();

	$userRol         = new UserRol();
	$userPermissions = $userRol->getPermissions();
}

abstract class Roles {

	abstract public function getPermissions();
}

class AdminRol extends Roles {

	public function getPermissions()
	{
		return [ 'create', 'update', 'delete' ];
	}
}

class UserRol extends Roles {

	public function getPermissions()
	{
		return [ 'create', 'update' ];
	}
}
```

Imagina ahora otro escenario, en el que tenemos una clase `MailSystem` y una clase `DatabaseSystem` que definen permisos en funci�n del rol. En este caso, no ser�a l�gico que heredaran de una clase com�n. Y utilizar�amos una interface para implementar el m�todo `getPermissions()` en ambas clases.

```
function getPermissions()
{
	$mailSystem           = new MailSystem();
	$mailAdminPermissions = $mailSystem->getAdminPermissions();
	$mailUserPermissions  = $mailSystem->getUserPermissions();

	$databaseSystem           = new DatabaseSystem();
	$databaseAdminPermissions = $databaseSystem->getAdminPermissions();
	$databaseUserPermissions  = $databaseSystem->getUserPermissions();
}

interface RolePermissions {

	public function getAdminPermissions();

	public function getUserPermissions();
}

class MailSystem implements RolePermissions {

	public function getAdminPermissions()
	{
		return [ 'send', 'draft' ];
	}

	public function getUserPermissions()
	{
		return [ 'draft' ];
	}
}

class DatabaseSystem implements RolePermissions {

	public function getAdminPermissions()
	{
		return [ 'create', 'update', 'delete' ];
	}

	public function getUserPermissions()
	{
		return [ 'create', 'update' ];
	}
}
```
 
###Acoplamientos Fuertes o D�biles.

En OOP el acoplamiento o dependencia de una clase con respecto a otra se suele definir como d�bil (__loosely coupled__) o fuerte (__tightly coupled__). En funci�n del tipo de relaci�n que mantienen ambas clases entre s�.

En la vida real, un ejemplo de acoplamiento fuerte, es el de un ordenador con el procesador. Es muy complejo construir un ordenador sin procesador o procesadores de naturaleza muy distinta a los actuales. Sin embargo, el sistema de almacenamiento en los ordenadores est� d�bilmente acoplado. De ah� que se puedan usar varios sistemas distintos como: Discos duros, Memorias USB, Cds, Tarjetas de Memoria, Internet, etc...

Cuando una clase est� fuertemente acoplada a otra, si modificamos esa clase, tendremos que modificar tambi�n su clase acoplada. Por eso, unas clases fuertemente acopladas entre s� son menos flexibles, m�s propensas a errores y m�s dif�ciles de mantener y de testar.

Una frase muy com�n en OOP es: 'Busca acoplamientos d�biles y alta cohesi�n'.

> Hay alta cohesi�n en el c�digo cuando c�digos que est�n agrupados cumplen tareas similares y baja cohesi�n cuando c�digos agrupados cumplen tareas muy distintas. Un modo de conseguir alta cohesi�n es implementar el principio SRP en nuestras clases y m�todos.

En PHP, se suele decir que dos clases est�n __fuertemente acopladas__ cuando una clase contiene una referencia directa a otra clase que le proporciona el funcionamiento que necesita. De modo que es dependiente de la clase contenida. Y si en su lugar, una clase contiene una referencia a un interface (que puede ser implementado por una o varias clases), se dice que esa clase est� __d�bilmente acoplada__.

Por tanto, un modo de conseguir acomplamientos d�biles es utilizando interface e inyecci�n de dependencias. Cuando usamos inyecci�n de dependecias, sustitu�mos una instancia directa de una clase (habitualmente en forma de llamada al constructor de esa clase) por un nuevo par�metro en el m�todo para recibir una interface.

Vamos a ver un ejemplo. Imagina que tenemos el siguiente c�digo:

```
<?php

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
```

La clase `DatabaseSystem` est� __fuertemente acoplada__ a la clase `DropBox` con los siguientes inconvenientes:

- Si quisi�ramos guardar la configuraci�n en otro sistema de almacenamiento, tendr�amos que modificar el c�digo de la clase `DatabaseSystem`.
- No es posible cambiar el sistema de almacenamiento de forma din�mica durante la ejecuci�n de la aplicaci�n. Por ejemplo, mediante un fichero de configuraci�n.
- Para probar la clase `DatabaseSystem`, tenemos que probar tambi�n la clase `DropBox`.
- Dado que la dependencia se realiza en el c�digo interno de un m�todo, no es f�cil recordar que existe esa dependencia o incluso es posible que otro programador no sea consciente de ella.


> Otro modo de conseguir acomplamientos d�biles es implementar el patr�n de dise�o 'Mediator' o 'Facade'.



 
- Inyecci�n de dependencias.

 El c�digo ha de poder probarse sin necesidad de modificarlo. Cuando llamamos al constructor de una clase directamente desde nuestro c�digo, se dice que est� 'Fuertemente acoplado' a esa clase. Y, por tanto, si queremos sustituirla por otra clase distinta (por ejemplo, para los tests), tendremos que modificar el c�digo.
 
 Es un buen h�bito ese tipo de c�digo y utilizar en su lugar 'Inyecci�n de dependencias'. Al menos el c�digo que vaya a pasar por un test, escribelo utilizando __Inyecci�n de dependencias__.
 
 Para eso, cada vez que en un m�todo est�s creando un objeto directamente desde el constructor de su clase, sustituye ese c�digo por una propiedad protected o un m�todo que te devuelva una instancia de ese objeto y modifica el constructor de la clase o los par�metros de tu m�todo para que reciban esa instancia.
 
 Si por ejemplo, tienes el siguiente c�digo:

 ```
class UserFakerSeeder extends BaseSeeder {

	public function getDummyData(array $customValues = [ ])
	{
		$faker=new Faker\Generator();

		return [
			'name'     => $faker->name,
			'email'    => $faker->email,
			'password' => bcrypt('secret')
		];
	}

}
```
 
 Puedes sustituirlo por este:

 ```
class UserFakerSeeder extends BaseSeeder {

	public function getDummyData(Generator $faker, array $customValues = [ ])
	{
		return [
			'name'     => $faker->name,
			'email'    => $faker->email,
			'password' => bcrypt('secret')
		];
	}

}
```

 O este otro:

 ```
class UserFakerSeeder extends BaseSeeder {

	protected  $faker;

	public function __construct(Generator $faker)
	{
		$this->faker = $faker;
	}

	public function getDummyData(array $customValues = [ ])
	{
		return [
			'name'     => $this->faker->name,
			'email'    => $this->faker->email,
			'password' => bcrypt('secret')
		];
	}

}
``` 

 De ese modo, puedes inyectar una clase de pruebas (`Mock`) como `Faker` en lugar de la clase original `Faker\Generator`.
 
 Por ejemplo, ahora para probar el c�digo anterior, podr�amos usar PHPUnit y escribir algo similar a esto:

 ``` 
 $faker = Mockery::mock('Faker\Generator');
 $userFaker = new UserFakerSeeder($faker);
 (...)
 ```
 
> **Recomendaci�n:** Busca todas las l�neas de tu c�digo en las que usas la palabra clave `new` y comprueba si hay un modo mejor de escribir ese c�digo utilizando inyecci�n de dependencias.

- SRP aplicado a los constructores.

 La �nica responsabilidad del constructor deber�a ser asignar dependencias. Ser�a como un asistente que coloca cada instancia o dependencia en su propiedad correspondiente.
 
 Si tienes un constructor como este:

 ```
	public function __construct(Generator $faker)
	{
		$this->faker = $faker;

		$this->setCheckForeignKeys(false);

		$this->truncateTables();
		
		$this->setCheckForeignKeys(true);
	}
 ```
 
 Deber�as convertirlo en este:
 
 ```
	public function __construct(Generator $faker)
	{
		$this->faker = $faker;
	}
 ```

 Y mover el resto de c�digo a otro m�todo.

 Eso te permitir� mantener m�s simples tus tests. Ya que en el caso anterior, todos los tests que escribas deber�n tener en cuenta cada una de las acciones realizadas en el constructor de la clase.

 http://blog.cleancoder.com/uncle-bob/2014/06/30/ALittleAboutPatterns.html