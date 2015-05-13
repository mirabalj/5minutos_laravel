###Buenos principios y prácticas de programación.

Habitualmente, la vida de una aplicación es mucho mayor que el tiempo que lleva desarrollarla. Durante la fase de desarrollo pasaremos una gran parte de nuestro tiempo probando la aplicación y solucionando los posibles errores que surjan. Una vez desarrollada, el tiempo que le dediquemos será principalmente para implementar cambios y nuevas funcionalidades o solucionar errores.

Todos los principios que te presento en este post están pensados para minimizar los posibles errores de tu aplicación y el tiempo dedicado a corregirlos y al mantenimiento de esta.

Aunque en ocasiones pueda parecer que implementar estos principios retrasarán los tiempos de la fase de desarrollo, a la larga, el ratio coste/beneficio es muy favorable si tenemos en cuenta en cuanto reducen el tiempo dedicado a corregir errores y al mantenimiento de la aplicación. Incluso, durante la fase de desarrollo ese ratio es positivo, dado que también durante esa fase pasamos una gran parte del tiempo corrigiendo errores.

Por supuesto, son recomendaciones y principios. Por tanto, no son obligatorios y está en tu mano utilizarlos o no, y elegir aquellos que creas conveniente en función de las necesidades de tu aplicación, de tu estilo de programación, etc...

Afortunadamente la Programación Orientada a Objetos (OOP) y el Framework Laravel implementan la mayoría de estos principios. Por lo que tienes una gran parte del trabajo hecho.

###La caja negra: La importancia de la 'discreción'.

En OOP, un principio muy importante es el que se suele llamar como 'La Caja Negra' o encapsulamiento. Ese principio es un metáfora de las cajas negras de los aviones. Y recomienda que por defecto las clases mantengan todo el código posible de forma privada. Exponiendo públicamente únicamente aquellas propiedades y métodos que sean imprescindibles para usar la clase.

De este modo, haremos nuestra clase más mantenible, flexible y tolerante a modificaciones.

Yo lo suelo llamar también el principio de la 'discreción': Imagina que tus clases van a una fiesta o a una entrevista de trabajo, deberían ser lo más discretas posibles sobre sí mismas y contar sólo aquello que consideren imprescindible.

####Utiliza __getters y setters__ para acceder a las propiedades o campos de las clases.

En lugar de exponer tus campos como públicos, utilizar métodos getters y setters para acceder a ellos. De ese modo, podrás validarlos y procesarlos antes de devolverlos o modificar su valor.

###Código abierto para extensión y cerrado para modificación.

En inglés: __Open/Closed Principle (OCP)__

Es decir, crea clases que otros no puedan modificar pero sí puedan extender. Es otra forma de expresar el concepto de caja negra o encapsulamiento.

Por defecto, declara todos los campos o miembros de la clase como privados. Escribe métodos `get()` y `set()` únicamente cuando los necesites.

###Mantén el código simple, estúpido

 - Mantén tu código simple.

 Esta es la primera clave y la más importante. Una frase ideal para recordarla es: 'Divide y vencerás': Divide tu código complejo en varios métodos más simples. De ese modo será menos propenso a errores y cuando estos aparezcan será mucho más fácil encontrarlos y solucionarlos.
 
> En inglés se suele llamar: __KISS (Keep it simple, stupid)__ que traducido sería algo como: Mantenlo simple, estúpido.
 
 - Mantén simples tus métodos. 
 
  Un método debería tener las mínimas líneas posibles. Incluso sólo una si es posible.
  
  Un método con más de 15 o 20 líneas puede ser una señal de que es demasiado complejo.
  
 
###El Principio de Responsabilidad Simple. (SRP o Single Responsibility Principle).

- Aplica SRP a tus clases.
  
  - Cada clase debería tener sólo una responsabilidad y esa responsabilidad debería ser encapsulada totalmente por la clase.
  
  - Nunca debería de existir más de una razón para modificar una clase.

>  Si cuando describes lo que hace tu clase, usas la palabra 'y' más de tres veces, probablemente es una perfecta candidata a una refactorización.
>  
>  Otra señal de que una clase tiene demasiadas responsabilidades es que te sea difícil poner un nombre a tu clase.

  
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

 Otra forma de aplicar este principio es crear un servicio. Por ejemplo, en una aplicación es posible que validemos los datos en varios puntos del código. Dado que en ocasiones esos puntos no tendrán relación entre sí, podemos crear un servicio que valide los datos y, de ese modo estamos separando esa responsabilidad en un lugar común sin mezclar clases con responsabilidades distintas.

- Usa la pregunta: ¿Cual es el código más simple que puedo escribir para que esto funcione?

El flujo de trabajo propuesto por la metodología de pruebas TDD está basado en este principio.

- No me hagas pensar.

 Cualquier programador debería entender lo que hace tu código con echar un simple vistazo.
 
 Un movimiento que promueve este principio es el de utilizar el 'Lenguaje Ubicuo' (__Ubiquitous Language__) para los nombres de las variables, métodos y clases en el código. 
 
 El lenguaje ubicuo es un término que introdujo Eric Evans en su libro sobre DDD (__Domain Driven Design__) como propuesta para crear un lenguaje común entre los programadores y los usuarios.
 
 Intenta adivinar qué hace este código:
 
 ```
	protected function getRandom($name)
	{
		if ( ! $this->isSet($name) )
		{
			throw new Exception ("The $name does not exist");
		}

		return static::$pool[$name]->random();
	}

	(...)
	
	protected function createMany($k)
	{
		for ($i = 1; $i <= $i; $i++)
		{
			$this->create($i);
		}
	}	
 ```
 
 Imagina ahora el mismo código escrito utilizando lenguaje ubicuo:
 
 ```
	protected function getRandomEntityFrom($modelName)
	{
		if ( ! $this->collectionExists($modelName) )
		{
			throw new OutOfRangeException ("The $modelName does not exist");
		}

		return static::$modelsCollection[$modelName]->random();
	}

	(...)
	
	protected function createMultipleEntities($numberOfEntities)
	{
		for ($entitiesCounter = 1; $entitiesCounter <= $numberOfEntities; $entitiesCounter++)
		{
			$this->createEntity($entitiesCounter);
		}
	}	
 ```

 Con este tipo de nombres, el código se documenta por sí mismo:
 - `getRandomEntityFrom($modelName)` devuelve una entidad aleatoria a partir del nombre de un modelo.
 - Comprueba si el modelo existe en la colección antes de acceder a él.
 - `createMultipleEntities($numberOfEntities)` una cantidad de entidades determinada por el parámetro que recibe.
 - Para ello usa un bucle en el que se crea cada vez una nueva entidad a partir del contador actual.
 
###No te repitas (Don't Repeat Yourself o DRY)

Este principio es una filosofía de desarrollo que dice que ningún algoritmo debería aparecer más de una vez.

De ese modo, si hay que hacer cambios, podremos hacerlos sólo en un punto de nuestro código y evitaremos las inconsistencias que pueden aparecer cuando tenemos algoritmos duplicados y no hacemos los cambios en todos los lugares en los que aparece ese algoritmo.

La aplicación práctica es: Tan pronto como veas que escribes el mismo algoritmo varias veces, conviértelo en una función, método, clase, clase abstracta o servicio.

He evitado usar la palabra '__código__', y he utilizado '__algoritmo__' en su lugar porque este principio no se aplica al código, sino al significado de ese código.

Es decir, habrá ocasiones en las que dos fragmentos de código que no tienen relación alguna sean muy parecidos, o incluso idénticos. Pero que en realidad, incluso aunque estén realizando funciones similares, dependen de reglas muy distintas. Y, por tanto, no pueden considerarse duplicados ni sustituires por una única implementación.

Piensa en ello como si estuvieras escribiendo un libro. En un idioma hay palabras que significan los mismo y en otros casos, la misma palabra o expresión significa cosas distintas según el contexto. Habrá ocasiones en las que no podrás sustituir una palabra por un sinónimo y habrá ocasiones en las que podrías decir que una frase está repetida varias veces en el libro, pero si lees atentamente las páginas en las que aparecen esas frases descubrirás que sólo tienen en común que se han utilizado las mismas palabras al escribirlas.

> **Nota:** La clave para saber si un código o algoritmo está duplicado es buscar: ¿Qué reglas determinan ese código? ¿Qué cambio en esas reglas obligarían a modificar el código? Si el cambio en esas reglas afecta por igual a varios trozos de código o algoritmos, puede considerarse que están duplicados y que pueden abstraerse a un código o algoritmo común.

###No cruces el puente antes de llegar a él.

En inglés este principio se llama __You aren’t going to need it (YAGNI)__ que se traduciría por 'No lo vas a necesitar'.

La idea es no crear funcionalidades hasta que no las necesitemos. Ya que cada funcionalidad añade complejidad al código y en muchas ocasiones empezamos a implementar muchas funcionalidades del tipo 'Por si acaso' que no llegamos a usar nunca.

###La ley de Demeter:

Una clase sólo debe comunicarse con aquellas clases con las que esté directamente relacionada: Por herencia, porque las contiene o está contenida en ellas, porque se le ha pasado una instancia como argumento, etc...

Por ejemplo, evitar código como este:

```
	return $this->mailMessage->html->header->status();
```

###El Principio de Substitución de Liskov (__Liskov Substitution Principle o LSP__)

Este principio dice: Las clases derivadas o heredadas deben ser sustituíbles por sus clases bases o padres.

Cuando sale un nuevo sistema operativo, por ejemplo, Windows 8, habitualmente se ha implementado de modo que garantice una compatibilidad 'hacia atrás'. Es decir, cualquier programa que funcionaba en Windows 7, debería funcionar con Windows 8. La implementación de este principio sería garantizar justo lo contrario. Es decir, una 'compatibilidad hacia delante'. De modo que cualquier programa que utilice funciones de Windows 8 que ya existían en Windows 7 (Por ejemplo, el sistema de ficheros), debería funcionar correctamente en Windows 7 y poder utilizar esas funciones sin cambios.

Pasándonos al mundo de la programación y la OOP, este principio habla de un **contrato de comportamientos**. Cuando una clase hereda de otra (o implementa un inteface), no sólo debe cumplir el contrato implementando al menos los mismos métodos y propiedades públicas, sino que además esos métodos deben comportarse internamente del mismo modo en que lo hacen en la clase base. Es decir, no basta con que la nueva clase 'sea de un tipo', debe también 'comportarse de ese modo'.

La idea básica es que al heredar, puedes extender los métodos existentes pero no puedes restringirlos. Por tanto, el principio se aplica únicamente a la funcionalidad existente en la clase base. Ya que un método que utilice la clase base, si le pasamos una instancia de la clase hija, nunca llamará a la nueva funcionalidad porque no sabe que existe.

Las claves de este principio son: (Referidas a la clase hereda o clase 'hija')

- Debe cumplir el contrato público de la clase base.
  Deben existir todos los métodos y propiedades públicas que implementa la clase base.
  Se pueden añadir nuevos métodos y propiedades públicas.
- Los argumentos de los métodos deben ser compatibles.
  Si se añaden argumentos, deben ser opcionales.
  Si se modifica el tipo de dato de los argumentos, el nuevo tipo debe ser compatible con el anterior.
- Los resultados o valores de retorno deben ser compatibles.
  Debe devolverse el mismo tipo de dato o, si se ha modificado, un tipo compatible con el anterior.
- Las excepciones deben coincidir. (En los métodos heredados)
  Si devuelves una excepción, debe ser del mismo tipo que las excepciones devueltas por la clase base.
  Las reglas que lanzan una excepción no pueden ser más restrictivas que las de la clase base.

###El Principio de Segregación de Interfaces (__Interface Segregation Principle o ISP__)

Este principio dice textualmente: Ningún cliente (de una interface) debería estar obligado a implementar métodos que no utiliza.

Dado que todas las clases que implementan una interface tienen que implementar todos los métodos declarados en esta, puede suceder que con el tiempo lo que era una interface sencilla llegue a tener muchos métodos y no todos los métodos estén relacionados entre sí.

Lo que promueve este principio es la separación de interfaces complejas en varias interface cuyos métodos tengan una alta cohesión entre sí. Este principio está relacionado con el __Principio de Responsabilidad Simple__ y promueve también la alta cohesión en las intefaces.

Si es necesario, podrías dividir una interfaces en varias intefaces más pequeñas y las clases implementar sólo aquellas interfaces que necesiten. Es decir, según este principio, una clase puede implementar más de una interface. Pero, ten en cuenta que eso podría estar incumpliendo el __Principio de Simple Responsabilidad__.
  
###SOLID

**SOLID** es un acrónimo inglés para referirse a cinco de los principios que ya hemos visto:

- Single Responsibility Principle. (Principio de Responsabilidad Simple)
- Open/Closed Principle. (Código abierto para extensión y cerrado para modificación)
- Liskov Substitution Principle. (Principio de Substitución de Liskov)
- Interface Segregation Principle. (Principio de Segregación de Interfaces)
- Dependency Inversion Principle. (

Su propuesta es diseñar nuestro código y nuestras clases teniendo en cuenta esos cinco principios.

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

