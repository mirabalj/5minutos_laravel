###La caja negra: La importancia de la 'discreción'

En OOP, un principio muy importante es el que se suele llamar como 'La Caja Negra' o encapsulamiento. Ese principio es un metáfora de las cajas negras de los aviones. Y recomienda que por defecto las clases mantengan todo el código posible de forma privada. Exponiendo públicamente únicamente aquellas propiedades y métodos que sean imprescindibles para usar la clase.

De este modo, haremos nuestra clase más mantenible, flexible y tolerante a modificaciones.

Yo lo suelo llamar también el principio de la 'discreción': Imagina que tus clases van a una fiesta o a una entrevista de trabajo, deberían ser lo más discretas posibles sobre sí mismas y contar sólo aquello que consideren imprescindible.

####Utiliza __getters y setters__ para acceder a las propiedades o campos de las clases.

En lugar de exponer tus campos como públicos, utilizar métodos getters y setters para acceder a ellos. De ese modo, podrás validarlos y procesarlos antes de devolverlos o modificar su valor.

###Manten el código simple, estúpido

 - Mantén tu código simple.

 Esta es la primera clave y la más importante. Una frase ideal para recordarla es: 'Divide y vencerás': Divide tu código complejo en varios métodos más simples. De ese modo será menos propenso a errores y cuando estos aparezcan será mucho más fácil encontrarlos y solucionarlos.
 
> En inglés se suele llamar: __KISS (Keep it simple, stupid)__ que traducido sería algo como: Mantenlo simple, estúpido.
 
 - Mantén simples tus métodos. 
 
  Un método debería tener las mínimas líneas posibles. Incluso sólo una si es posible.
  
  Un método con más de 15 o 20 líneas puede ser una señal de que es demasiado complejo.
  
 
###El Principio de Responsabilidad Simple. (SRP o Simple Responsability Principle).

- Aplica SRP a tus clases.
  
  Si cuando describes lo que hace tu clase, usas la palabra 'y' más de tres veces, probablemente es una perfecta candidata a una refactorización.
  
  Otra señal de que una clase tiene demasiadas responsabilidades es que te sea difícil poner un nombre a tu clase.
  
- Aplica SRP a los constructores.

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

- Agrupa tu código por responsabilidades o incumbencias.

 Es decir, organiza tus clases y métodos en función de las responsabilidades que tengan en común. En inglés se le llama __Separation of Concerns (SoC)__ o __Advanced Separation of Concerns (ASoC)__.

 Sería algo así como 'que cada clase se ocupe de sus propios asuntos'. Habitualmente las responsabilidades o asuntos suelen ser sinónimos de comportamientos y características.
 
 Por ejemplo, el patrón de diseño MVC (Modelo - Vista - Controlador), es un ejemplo práctico de implementación de la separación de responsabilidades.
 
> La metodología de Programación Orientada al Aspecto (__Aspect Oriented Programming o AOP__) consiste en diseñar nuestras clases y código de modo que estén
> agrupados por responsabilidades.
> También se le conoce como __Aspect-Oriented Software Development (AOSD)__.

- Usa la pregunta: ¿Cual es el código más simple que puedo escribir para que esto funcione?

El flujo de trabajo propuesto por la metodología de pruebas TDD está basado en este principio.

- No me hagas pensar.

 Cualquier programador debería entender lo que hace tu código con echar un simple vistazo.
 
 Un movimiento que promueve este principio es el de utilizar el 'Lenguaje Ubicuo' (__Ubiquitous Language__) para los nombres de las variables, métodos y clases en el código. 
 
 El lenguaje ubicuo es un término que introdujo Eric Evans en su libro sobre DDD (__Domain Driven Design__) como propuesta para crear un lenguaje común entre los programadores y los usuarios.
 
 Un ejemplo de código escrito en lenguaje ubicuo sería:
 
 ```
 ```

###No te repitas (Don't Repeat Yourself o DRY)

Este principio es una filosofía de desarrollo que dice que ninguna pieza ni trozo de código debería aparecer más de una vez.

De ese modo, si hay que hacer cambios, podremos hacerlos sólo en un punto de nuestro código y evitaremos las inconsistencias que pueden aparecer cuando tenemos código duplicado y no modificamos todas las apariciones de ese código.

La aplicación práctica es: Tan pronto como veas que escribes el mismo código varias veces, conviértelo en una función, método, clase, clase abstracta o servicio.

###No cruces el puente antes de llegar a él.

En inglés este principio se llama __YAGNI (You aren’t going to need it)__ que se traduciría por 'No lo vas a necesitar'.

La idea es no crear funcionalidades hasta que no las necesitemos. Ya que cada funcionalidad añade complejidad al código y en muchas ocasiones empezamos a implementar muchas funcionalidades del tipo 'Por si acaso' que no llegamos a usar nunca.

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

Otro modo de conseguir acomplamientos débiles es implementar el patrón de diseño 'Mediator' o 'Facade'.
 
> **Recomendación:** Busca todas las líneas de tu código en las que usas la palabra clave `new` y comprueba si hay un modo mejor de escribir ese código utilizando inyección de dependencias.

###Gestión de errores en los parámetros.

En cada método comprueba valida los valores recibidos y lanza una excepción si no son válidos.

