###Empezando con los tests

- Nombres de los tests.
 Es importante que usemos nombres descriptivos para los tests. De ese modo, al mismo tiempo que desarrollamos y creamos nuestros tests, estaremos documentando nuestra aplicaci�n con posibles casos de uso.
 
 Por otro lado, si los nombres de los tests son auto-descriptivos, cuando falle un test, ser� m�s f�cil entender qu� ha fallado sin necesidad de ver el c�digo del test para entenderlo.
 
- �Qu� partes hay que probar?

 La teor�a dice que hay que probar todo aquello que sea susceptible de fallar, por tanto, habr�a que probar todas nuestras l�neas de c�digo. Sin embargo, aunque los tests son muy �tiles y nos ayudan a generar un c�digo m�s robusto, un exceso de tests no compensa el ratio esfuerzo/beneficio y podemos volvernos locos para conseguir probar cada l�nea de c�digo.
 
 Por tanto, lo ideal es aplicar el sentido com�n y encontrar un equilibrio entre el n�mero de tests desarrollados, el porcentaje de c�digo cubierto, la importancia de ese c�digo en la estabilidad de la aplicaci�n y el porcentaje de calidad obtenido.
 
 Mi m�todo es identificar las partes cr�ticas de mi aplicaci�n y asegurarme de que como m�nimo esas partes est�n cubiertas. Y a�adir tambi�n al menos los tests que me permitan validar las especificaciones funcionales y las especificaciones del cliente.

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
 
###Metodolog�as de Pruebas:
**TCA - Pruebas despu�s de programar (Test After Coding)** 
En esta metodolog�a, las pruebas se realizan despu�s de que se haya escrito el c�digo.

**TDD - Desarrollo basado en pruebas (Test Driven Development)**
Las pruebas se realizan antes de escribir el c�digo.

Ciclo del TDD:
 1 Escribir un test e intentar compilar. (Aunque a�n no sea posible porque no se ha desarrollado la funcionalidad correspondiente)
 2 Escribir la m�nima cantidad de c�digo posible para que la unidad compile y podamos ejecutar el test. (Volver� a fallar, ya que a�n no tiene tiene la funcionalidad requerida)
 3 Escribir el c�digo para que al ejecutar el test, esta no falle. 
   La idea es que la funci�n no est� necesariamente 'elegantemente' escrita ni optimizada, sino que s�mplemente permita pasar el test.
 4 Refactorizar y optimizar el c�digo de la funci�n.
 5 Volver a pasar el test y comprobar que no hay errores.
 6 Pasar a desarrollar la siguiente unidad.
 
 En funci�n de la complejidad de la unidad, es posible que el ciclo de los pasos 4 y 5 se repita varias veces.
 Cada vez que se modifique el c�digo de esta unidad, volveremos de nuevo a ejecutar los pasos 3,4 y 5.
 Si cambian las especificaciones de la unidad, empezaremos primero reescribiendo el test.

**BDD - Desarrollo basado en comportamientos (Behavior Driven Development)**
Es un subconjunto de la metodolog�a TDD.

 
###Ventajas del uso de pruebas:
 * Facilitar los cambios en la aplicaci�n
	Nos permiten realizar cambios y estar seguros de que no han introducido errores en la aplicaci�n.
 * Documentar la aplicaci�n.
	Cada tipo de prueba permite conocer los procesos, requerimientos, comportamientos y flujos de la aplicaci�n.
 * Facilitan la independencia entre el c�digo de la aplicaci�n y el interface.
 * Los errores est�n m�s acotados y son m�s f�ciles de localizar.
 * Aceleran el desarrollo del software.
 * Ayudan a tener un c�digo desacoplado.
 
Una vez le� que _Programar sin pruebas es c�mo cacharrear en un panel el�ctrico con un tenedor_ ;)
 
###Afirmaciones (Assert)
Son condiciones que el programador coloca en aquellos lugares en los que deben ser ciertas. De este modo se crean condiciones para la ejecuci�n de ciertos bloques o l�neas de c�digo.

Por ejemplo:

	$semanas=5;
	$dias=3;
	$dias_totales=$dias + ($semanas * 7);
	echo("Han pasado $dias_totales d�as");
	// Assert ($dias_totales == 38)
	if($dias_totales != 38)
	{
		// Error de test
	}
	
	
###Glosario:

**Clase Mock, Mock Class o Mocked Class**

Es una clase creada especialmente para los tests. Simula a la clase real y nos permite pasar tests a nuestro c�digo y a nuestra aplicaci�n en un entorno controlado, e incluso hacer pruebas aunque la clase original a�n no est� disponible (no tenemos conexi�n, no ha sido desarrollada a�n, etc..).
	
###M�s informaci�n:

[Estupendo tutorial paso a paso sobre la refactorizaci�n de c�digo en PHP](http://jesuslc.com/2014/10/29/refactorizando-legacy-code-en-php-parte-1/)  
[Patrones para mejorar tests con PHP y PHPUnit � TDD](https://jesuslc.com/2014/07/04/patrones-para-mejorar-tests-con-php-y-phpunit-tdd/=
[Legacy code retreat](http://legacycoderetreat.typepad.com/)  
[Refactoring Legacy Code](http://code.tutsplus.com/series/refactoring-legacy-code--cms-633)	