###Buenos principios y pr�cticas de programaci�n.

Habitualmente, la vida de una aplicaci�n es mucho mayor que el tiempo que lleva desarrollarla. Durante la fase de desarrollo pasaremos una gran parte de nuestro tiempo probando la aplicaci�n y solucionando los posibles errores que surjan. Una vez desarrollada, el tiempo que le dediquemos ser� principalmente para implementar cambios y nuevas funcionalidades o solucionar errores.

Todos los principios que te presento en este post est�n pensados para minimizar los posibles errores de tu aplicaci�n y el tiempo dedicado a corregirlos y al mantenimiento de esta.

Aunque en ocasiones pueda parecer que implementar estos principios retrasar�n los tiempos de la fase de desarrollo, a la larga, el ratio coste/beneficio es muy favorable si tenemos en cuenta en cuanto reducen el tiempo dedicado a corregir errores y al mantenimiento de la aplicaci�n. Incluso, durante la fase de desarrollo ese ratio es positivo, dado que tambi�n durante esa fase pasamos una gran parte del tiempo corrigiendo errores.

Por supuesto, son recomendaciones y principios. Por tanto, no son obligatorios y est� en tu mano utilizarlos o no, y elegir aquellos que creas conveniente en funci�n de las necesidades de tu aplicaci�n, de tu estilo de programaci�n, etc...

Afortunadamente la Programaci�n Orientada a Objetos (OOP) y el Framework Laravel implementan la mayor�a de estos principios. Por lo que tienes una gran parte del trabajo hecho.

###La caja negra: La importancia de la 'discreci�n'.

En OOP, un principio muy importante es el que se suele llamar como 'La Caja Negra' o encapsulamiento. Ese principio es un met�fora de las cajas negras de los aviones. Y recomienda que por defecto las clases mantengan todo el c�digo posible de forma privada. Exponiendo p�blicamente �nicamente aquellas propiedades y m�todos que sean imprescindibles para usar la clase.

De este modo, haremos nuestra clase m�s mantenible, flexible y tolerante a modificaciones.

Yo lo suelo llamar tambi�n el principio de la 'discreci�n': Imagina que tus clases van a una fiesta o a una entrevista de trabajo, deber�an ser lo m�s discretas posibles sobre s� mismas y contar s�lo aquello que consideren imprescindible.

####Utiliza __getters y setters__ para acceder a las propiedades o campos de las clases.

En lugar de exponer tus campos como p�blicos, utilizar m�todos getters y setters para acceder a ellos. De ese modo, podr�s validarlos y procesarlos antes de devolverlos o modificar su valor.

###C�digo abierto para extensi�n y cerrado para modificaci�n.

En ingl�s: __Open/Closed Principle (OCP)__

Es decir, crea clases que otros no puedan modificar pero s� puedan extender. Es otra forma de expresar el concepto de caja negra o encapsulamiento.

Por defecto, declara todos los campos o miembros de la clase como privados. Escribe m�todos `get()` y `set()` �nicamente cuando los necesites.

###Mant�n el c�digo simple, est�pido

 - Mant�n tu c�digo simple.

 Esta es la primera clave y la m�s importante. Una frase ideal para recordarla es: 'Divide y vencer�s': Divide tu c�digo complejo en varios m�todos m�s simples. De ese modo ser� menos propenso a errores y cuando estos aparezcan ser� mucho m�s f�cil encontrarlos y solucionarlos.
 
> En ingl�s se suele llamar: __KISS (Keep it simple, stupid)__ que traducido ser�a algo como: Mantenlo simple, est�pido.
 
 - Mant�n simples tus m�todos. 
 
  Un m�todo deber�a tener las m�nimas l�neas posibles. Incluso s�lo una si es posible.
  
  Un m�todo con m�s de 15 o 20 l�neas puede ser una se�al de que es demasiado complejo.
  
 
###El Principio de Responsabilidad Simple. (SRP o Single Responsibility Principle).

- Aplica SRP a tus clases.
  
  - Cada clase deber�a tener s�lo una responsabilidad y esa responsabilidad deber�a ser encapsulada totalmente por la clase.
  
  - Nunca deber�a de existir m�s de una raz�n para modificar una clase.

>  Si cuando describes lo que hace tu clase, usas la palabra 'y' m�s de tres veces, probablemente es una perfecta candidata a una refactorizaci�n.
>  
>  Otra se�al de que una clase tiene demasiadas responsabilidades es que te sea dif�cil poner un nombre a tu clase.

  
- Aplica SRP a los constructores.

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

- Agrupa tu c�digo por responsabilidades o incumbencias.

 Es decir, organiza tus clases y m�todos en funci�n de las responsabilidades que tengan en com�n. En ingl�s se le llama __Separation of Concerns (SoC)__ o __Advanced Separation of Concerns (ASoC)__.

 Ser�a algo as� como 'que cada clase se ocupe de sus propios asuntos'. Habitualmente las responsabilidades o asuntos suelen ser sin�nimos de comportamientos y caracter�sticas.
 
 Por ejemplo, el patr�n de dise�o MVC (Modelo - Vista - Controlador), es un ejemplo pr�ctico de implementaci�n de la separaci�n de responsabilidades.
 
> La metodolog�a de Programaci�n Orientada al Aspecto (__Aspect Oriented Programming o AOP__) consiste en dise�ar nuestras clases y c�digo de modo que est�n
> agrupados por responsabilidades.
> Tambi�n se le conoce como __Aspect-Oriented Software Development (AOSD)__.

 Otra forma de aplicar este principio es crear un servicio. Por ejemplo, en una aplicaci�n es posible que validemos los datos en varios puntos del c�digo. Dado que en ocasiones esos puntos no tendr�n relaci�n entre s�, podemos crear un servicio que valide los datos y, de ese modo estamos separando esa responsabilidad en un lugar com�n sin mezclar clases con responsabilidades distintas.

- Usa la pregunta: �Cual es el c�digo m�s simple que puedo escribir para que esto funcione?

El flujo de trabajo propuesto por la metodolog�a de pruebas TDD est� basado en este principio.

- No me hagas pensar.

 Cualquier programador deber�a entender lo que hace tu c�digo con echar un simple vistazo.
 
 Un movimiento que promueve este principio es el de utilizar el 'Lenguaje Ubicuo' (__Ubiquitous Language__) para los nombres de las variables, m�todos y clases en el c�digo. 
 
 El lenguaje ubicuo es un t�rmino que introdujo Eric Evans en su libro sobre DDD (__Domain Driven Design__) como propuesta para crear un lenguaje com�n entre los programadores y los usuarios.
 
 Intenta adivinar qu� hace este c�digo:
 
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
 
 Imagina ahora el mismo c�digo escrito utilizando lenguaje ubicuo:
 
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

 Con este tipo de nombres, el c�digo se documenta por s� mismo:
 - `getRandomEntityFrom($modelName)` devuelve una entidad aleatoria a partir del nombre de un modelo.
 - Comprueba si el modelo existe en la colecci�n antes de acceder a �l.
 - `createMultipleEntities($numberOfEntities)` una cantidad de entidades determinada por el par�metro que recibe.
 - Para ello usa un bucle en el que se crea cada vez una nueva entidad a partir del contador actual.
 
###No te repitas (Don't Repeat Yourself o DRY)

Este principio es una filosof�a de desarrollo que dice que ning�n algoritmo deber�a aparecer m�s de una vez.

De ese modo, si hay que hacer cambios, podremos hacerlos s�lo en un punto de nuestro c�digo y evitaremos las inconsistencias que pueden aparecer cuando tenemos algoritmos duplicados y no hacemos los cambios en todos los lugares en los que aparece ese algoritmo.

La aplicaci�n pr�ctica es: Tan pronto como veas que escribes el mismo algoritmo varias veces, convi�rtelo en una funci�n, m�todo, clase, clase abstracta o servicio.

He evitado usar la palabra '__c�digo__', y he utilizado '__algoritmo__' en su lugar porque este principio no se aplica al c�digo, sino al significado de ese c�digo.

Es decir, habr� ocasiones en las que dos fragmentos de c�digo que no tienen relaci�n alguna sean muy parecidos, o incluso id�nticos. Pero que en realidad, incluso aunque est�n realizando funciones similares, dependen de reglas muy distintas. Y, por tanto, no pueden considerarse duplicados ni sustituires por una �nica implementaci�n.

Piensa en ello como si estuvieras escribiendo un libro. En un idioma hay palabras que significan los mismo y en otros casos, la misma palabra o expresi�n significa cosas distintas seg�n el contexto. Habr� ocasiones en las que no podr�s sustituir una palabra por un sin�nimo y habr� ocasiones en las que podr�as decir que una frase est� repetida varias veces en el libro, pero si lees atentamente las p�ginas en las que aparecen esas frases descubrir�s que s�lo tienen en com�n que se han utilizado las mismas palabras al escribirlas.

> **Nota:** La clave para saber si un c�digo o algoritmo est� duplicado es buscar: �Qu� reglas determinan ese c�digo? �Qu� cambio en esas reglas obligar�an a modificar el c�digo? Si el cambio en esas reglas afecta por igual a varios trozos de c�digo o algoritmos, puede considerarse que est�n duplicados y que pueden abstraerse a un c�digo o algoritmo com�n.

###No cruces el puente antes de llegar a �l.

En ingl�s este principio se llama __You aren�t going to need it (YAGNI)__ que se traducir�a por 'No lo vas a necesitar'.

La idea es no crear funcionalidades hasta que no las necesitemos. Ya que cada funcionalidad a�ade complejidad al c�digo y en muchas ocasiones empezamos a implementar muchas funcionalidades del tipo 'Por si acaso' que no llegamos a usar nunca.

###La ley de Demeter:

Una clase s�lo debe comunicarse con aquellas clases con las que est� directamente relacionada: Por herencia, porque las contiene o est� contenida en ellas, porque se le ha pasado una instancia como argumento, etc...

Por ejemplo, evitar c�digo como este:

```
	return $this->mailMessage->html->header->status();
```

###El Principio de Substituci�n de Liskov (__Liskov Substitution Principle o LSP__)

Este principio dice: Las clases derivadas o heredadas deben ser sustitu�bles por sus clases bases o padres.

Cuando sale un nuevo sistema operativo, por ejemplo, Windows 8, habitualmente se ha implementado de modo que garantice una compatibilidad 'hacia atr�s'. Es decir, cualquier programa que funcionaba en Windows 7, deber�a funcionar con Windows 8. La implementaci�n de este principio ser�a garantizar justo lo contrario. Es decir, una 'compatibilidad hacia delante'. De modo que cualquier programa que utilice funciones de Windows 8 que ya exist�an en Windows 7 (Por ejemplo, el sistema de ficheros), deber�a funcionar correctamente en Windows 7 y poder utilizar esas funciones sin cambios.

Pas�ndonos al mundo de la programaci�n y la OOP, este principio habla de un **contrato de comportamientos**. Cuando una clase hereda de otra (o implementa un inteface), no s�lo debe cumplir el contrato implementando al menos los mismos m�todos y propiedades p�blicas, sino que adem�s esos m�todos deben comportarse internamente del mismo modo en que lo hacen en la clase base. Es decir, no basta con que la nueva clase 'sea de un tipo', debe tambi�n 'comportarse de ese modo'.

La idea b�sica es que al heredar, puedes extender los m�todos existentes pero no puedes restringirlos. Por tanto, el principio se aplica �nicamente a la funcionalidad existente en la clase base. Ya que un m�todo que utilice la clase base, si le pasamos una instancia de la clase hija, nunca llamar� a la nueva funcionalidad porque no sabe que existe.

Las claves de este principio son: (Referidas a la clase hereda o clase 'hija')

- Debe cumplir el contrato p�blico de la clase base.
  Deben existir todos los m�todos y propiedades p�blicas que implementa la clase base.
  Se pueden a�adir nuevos m�todos y propiedades p�blicas.
- Los argumentos de los m�todos deben ser compatibles.
  Si se a�aden argumentos, deben ser opcionales.
  Si se modifica el tipo de dato de los argumentos, el nuevo tipo debe ser compatible con el anterior.
- Los resultados o valores de retorno deben ser compatibles.
  Debe devolverse el mismo tipo de dato o, si se ha modificado, un tipo compatible con el anterior.
- Las excepciones deben coincidir. (En los m�todos heredados)
  Si devuelves una excepci�n, debe ser del mismo tipo que las excepciones devueltas por la clase base.
  Las reglas que lanzan una excepci�n no pueden ser m�s restrictivas que las de la clase base.

###El Principio de Segregaci�n de Interfaces (__Interface Segregation Principle o ISP__)

Este principio dice textualmente: Ning�n cliente (de una interface) deber�a estar obligado a implementar m�todos que no utiliza.

Dado que todas las clases que implementan una interface tienen que implementar todos los m�todos declarados en esta, puede suceder que con el tiempo lo que era una interface sencilla llegue a tener muchos m�todos y no todos los m�todos est�n relacionados entre s�.

Lo que promueve este principio es la separaci�n de interfaces complejas en varias interface cuyos m�todos tengan una alta cohesi�n entre s�. Este principio est� relacionado con el __Principio de Responsabilidad Simple__ y promueve tambi�n la alta cohesi�n en las intefaces.

Si es necesario, podr�as dividir una interfaces en varias intefaces m�s peque�as y las clases implementar s�lo aquellas interfaces que necesiten. Es decir, seg�n este principio, una clase puede implementar m�s de una interface. Pero, ten en cuenta que eso podr�a estar incumpliendo el __Principio de Simple Responsabilidad__.
  
###SOLID

**SOLID** es un acr�nimo ingl�s para referirse a cinco de los principios que ya hemos visto:

- Single Responsibility Principle. (Principio de Responsabilidad Simple)
- Open/Closed Principle. (C�digo abierto para extensi�n y cerrado para modificaci�n)
- Liskov Substitution Principle. (Principio de Substituci�n de Liskov)
- Interface Segregation Principle. (Principio de Segregaci�n de Interfaces)
- Dependency Inversion Principle. (

Su propuesta es dise�ar nuestro c�digo y nuestras clases teniendo en cuenta esos cinco principios.

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

Otro modo de conseguir acomplamientos d�biles es implementar el patr�n de dise�o 'Mediator' o 'Facade'.
 
> **Recomendaci�n:** Busca todas las l�neas de tu c�digo en las que usas la palabra clave `new` y comprueba si hay un modo mejor de escribir ese c�digo utilizando inyecci�n de dependencias.

###Gesti�n de errores en los par�metros.

En cada m�todo comprueba valida los valores recibidos y lanza una excepci�n si no son v�lidos.

