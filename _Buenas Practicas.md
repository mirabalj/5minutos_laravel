http://williamdurand.fr/2013/07/30/from-stupid-to-solid-code/
###La caja negra: La importancia de la 'discreción'

En OOP, un principio muy importante es el que se suele llamar como 'La Caja Negra' o encapsulamiento. Ese principio es un metáfora de las cajas negras de los aviones. Y recomienda que por defecto las clases mantengan todo el código posible de forma privada. Exponiendo públicamente únicamente aquellas propiedades y métodos que sean imprescindibles para usar la clase.

De este modo, haremos nuestra clase más mantenible, flexible y tolerante a modificaciones.

Yo lo suelo llamar también el principio de la 'discreción': Imagina que tus clases van a una fiesta o a una entrevista de trabajo, deberían ser lo más discretas posibles sobre sí mismas y contar sólo aquello que consideren imprescindible.

####Utiliza __getters y setters__ para acceder a las propiedades o campos de las clases.

En lugar de exponer tus campos como públicos, utilizar métodos getters y setters para acceder a ellos. De ese modo, podrás validarlos y procesarlos antes de devolverlos o modificar su valor.




###Divide y vencerás

- Mantén tu código simple.

 Esta es la primera clave y la más importante. Una frase ideal para recordarla es: 'Divide y vencerás': Divide tu código complejo en varios métodos más simples. De ese modo será menos propenso a errores y cuando estos aparezcan será mucho más fácil encontrarlos y solucionarlos.
 
 - Mantén simples tus métodos. 
 
  Un método debería tener las mínimas líneas posibles. Incluso sólo una si es posible.
  
  Un método con más de 15 o 20 líneas puede ser una señal de que es demasiado complejo.
 

###El Principio de Responsabilidad Simple

- Aplica SRP a tus clases.
  
  Si cuando describes lo que hace tu clase, usas la palabra 'y' más de tres veces, probablemente es una perfecta candidata a una refactorización.
  
  Otra señal de que una clase tiene demasiadas responsabilidades es que te sea difícil poner un nombre a tu clase.
  
http://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html
###Polimorfismo

- Utiliza el polimorfismo.
 
  El polimorfismo en OOP es la posibilidad de llamar al mismo método en distintas clases. Para ello, hay dos métodos que se usan habitualmente:
  
  - Utilizar una `Interface` que deben implementar esas clases y, de ese modo, 'cumplen el contrato' propuesto en la interface. En resumen: Si tengo varias clases que tienen el método `clearData()`, defino una interface que declare ese método e implemento esa interface en esas clases. De ese modo, tengo la certeza de que en mi código puedo llamar al método  `clearData()` de cualquiera de esas clases.
  
  - Utilizar una clase abstracta de la que heredarán esas clases y que define los métodos comunes que tendrán esas clases.
 
  Un ejemplo claro de posibilidad de refactorización utilizando polimorfismo es en una estructura `switch`. Imagina que tienes el siguiente código:
  
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

En el primer paso aplicamos SRP y separamos cada caso del `switch` en un método:

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

Y, en el segundo paso, utilizamos el polimorfismo para crear una clase para cada Role y una clase abstracta común de la que heredarán ambas:

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

Imagina ahora otro escenario, en el que tenemos una clase `MailSystem` y una clase `DatabaseSystem` que definen permisos en función del rol. En este caso, no sería lógico que heredaran de una clase común. Y utilizaríamos una interface para implementar el método `getPermissions()` en ambas clases.

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
 
###Acoplamientos Fuertes o Débiles.

En OOP el acoplamiento o dependencia de una clase con respecto a otra se suele definir como débil (__loosely coupled__) o fuerte (__tightly coupled__). En función del tipo de relación que mantienen ambas clases entre sí.

En la vida real, un ejemplo de acoplamiento fuerte, es el de un ordenador con el procesador. Es muy complejo construir un ordenador sin procesador o procesadores de naturaleza muy distinta a los actuales. Sin embargo, el sistema de almacenamiento en los ordenadores está débilmente acoplado. De ahí que se puedan usar varios sistemas distintos como: Discos duros, Memorias USB, Cds, Tarjetas de Memoria, Internet, etc...

Cuando una clase está fuertemente acoplada a otra, si modificamos esa clase, tendremos que modificar también su clase acoplada. Por eso, unas clases fuertemente acopladas entre sí son menos flexibles, más propensas a errores y más difíciles de mantener y de testar.

Una frase muy común en OOP es: 'Busca acoplamientos débiles y alta cohesión'.

> Hay alta cohesión en el código cuando códigos que están agrupados cumplen tareas similares y baja cohesión cuando códigos agrupados cumplen tareas muy distintas. Un modo de conseguir alta cohesión es implementar el principio SRP en nuestras clases y métodos.

En PHP, se suele decir que dos clases están __fuertemente acopladas__ cuando una clase contiene una referencia directa a otra clase que le proporciona el funcionamiento que necesita. De modo que es dependiente de la clase contenida. Y si en su lugar, una clase contiene una referencia a un interface (que puede ser implementado por una o varias clases), se dice que esa clase está __débilmente acoplada__.

Por tanto, un modo de conseguir acomplamientos débiles es utilizando interface e inyección de dependencias. Cuando usamos inyección de dependecias, sustituímos una instancia directa de una clase (habitualmente en forma de llamada al constructor de esa clase) por un nuevo parámetro en el método para recibir una interface.

Vamos a ver un ejemplo. Imagina que tenemos el siguiente código:

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

La clase `DatabaseSystem` está __fuertemente acoplada__ a la clase `DropBox` con los siguientes inconvenientes:

- Si quisiéramos guardar la configuración en otro sistema de almacenamiento, tendríamos que modificar el código de la clase `DatabaseSystem`.
- No es posible cambiar el sistema de almacenamiento de forma dinámica durante la ejecución de la aplicación. Por ejemplo, mediante un fichero de configuración.
- Para probar la clase `DatabaseSystem`, tenemos que probar también la clase `DropBox`.
- Dado que la dependencia se realiza en el código interno de un método, no es fácil recordar que existe esa dependencia o incluso es posible que otro programador no sea consciente de ella.


> Otro modo de conseguir acomplamientos débiles es implementar el patrón de diseño 'Mediator' o 'Facade'.



 
- Inyección de dependencias.

 El código ha de poder probarse sin necesidad de modificarlo. Cuando llamamos al constructor de una clase directamente desde nuestro código, se dice que está 'Fuertemente acoplado' a esa clase. Y, por tanto, si queremos sustituirla por otra clase distinta (por ejemplo, para los tests), tendremos que modificar el código.
 
 Es un buen hábito ese tipo de código y utilizar en su lugar 'Inyección de dependencias'. Al menos el código que vaya a pasar por un test, escribelo utilizando __Inyección de dependencias__.
 
 Para eso, cada vez que en un método estés creando un objeto directamente desde el constructor de su clase, sustituye ese código por una propiedad protected o un método que te devuelva una instancia de ese objeto y modifica el constructor de la clase o los parámetros de tu método para que reciban esa instancia.
 
 Si por ejemplo, tienes el siguiente código:

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
 
 Por ejemplo, ahora para probar el código anterior, podríamos usar PHPUnit y escribir algo similar a esto:

 ``` 
 $faker = Mockery::mock('Faker\Generator');
 $userFaker = new UserFakerSeeder($faker);
 (...)
 ```
 
> **Recomendación:** Busca todas las líneas de tu código en las que usas la palabra clave `new` y comprueba si hay un modo mejor de escribir ese código utilizando inyección de dependencias.

- SRP aplicado a los constructores.

 La única responsabilidad del constructor debería ser asignar dependencias. Sería como un asistente que coloca cada instancia o dependencia en su propiedad correspondiente.
 
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
 
 Deberías convertirlo en este:
 
 ```
	public function __construct(Generator $faker)
	{
		$this->faker = $faker;
	}
 ```

 Y mover el resto de código a otro método.

 Eso te permitirá mantener más simples tus tests. Ya que en el caso anterior, todos los tests que escribas deberán tener en cuenta cada una de las acciones realizadas en el constructor de la clase.

 http://blog.cleancoder.com/uncle-bob/2014/06/30/ALittleAboutPatterns.html