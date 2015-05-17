###Empezando con los tests

- Nombres de los tests.
 Es importante que usemos nombres descriptivos para los tests. De ese modo, al mismo tiempo que desarrollamos y creamos nuestros tests, estaremos documentando nuestra aplicación con posibles casos de uso.
 
 Por otro lado, si los nombres de los tests son auto-descriptivos, cuando falle un test, será más fácil entender qué ha fallado sin necesidad de ver el código del test para entenderlo.
 
- ¿Qué partes hay que probar?

 La teoría dice que hay que probar todo aquello que sea susceptible de fallar, por tanto, habría que probar todas nuestras líneas de código. Sin embargo, aunque los tests son muy útiles y nos ayudan a generar un código más robusto, un exceso de tests no compensa el ratio esfuerzo/beneficio y podemos volvernos locos para conseguir probar cada línea de código.
 
 Por tanto, lo ideal es aplicar el sentido común y encontrar un equilibrio entre el número de tests desarrollados, el porcentaje de código cubierto, la importancia de ese código en la estabilidad de la aplicación y el porcentaje de calidad obtenido.
 
 Mi método es identificar las partes críticas de mi aplicación y asegurarme de que como mínimo esas partes están cubiertas. Y añadir también al menos los tests que me permitan validar las especificaciones funcionales y las especificaciones del cliente.

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
 
###Metodologías de Pruebas:
**TCA - Pruebas después de programar (Test After Coding)** 
En esta metodología, las pruebas se realizan después de que se haya escrito el código.

**TDD - Desarrollo basado en pruebas (Test Driven Development)**
Las pruebas se realizan antes de escribir el código.

Ciclo del TDD:
 1 Escribir un test e intentar compilar. (Aunque aún no sea posible porque no se ha desarrollado la funcionalidad correspondiente)
 2 Escribir la mínima cantidad de código posible para que la unidad compile y podamos ejecutar el test. (Volverá a fallar, ya que aún no tiene tiene la funcionalidad requerida)
 3 Escribir el código para que al ejecutar el test, esta no falle. 
   La idea es que la función no esté necesariamente 'elegantemente' escrita ni optimizada, sino que símplemente permita pasar el test.
 4 Refactorizar y optimizar el código de la función.
 5 Volver a pasar el test y comprobar que no hay errores.
 6 Pasar a desarrollar la siguiente unidad.
 
 En función de la complejidad de la unidad, es posible que el ciclo de los pasos 4 y 5 se repita varias veces.
 Cada vez que se modifique el código de esta unidad, volveremos de nuevo a ejecutar los pasos 3,4 y 5.
 Si cambian las especificaciones de la unidad, empezaremos primero reescribiendo el test.

**BDD - Desarrollo basado en comportamientos (Behavior Driven Development)**
Es un subconjunto de la metodología TDD.

 
###Ventajas del uso de pruebas:
 * Facilitar los cambios en la aplicación
	Nos permiten realizar cambios y estar seguros de que no han introducido errores en la aplicación.
 * Documentar la aplicación.
	Cada tipo de prueba permite conocer los procesos, requerimientos, comportamientos y flujos de la aplicación.
 * Facilitan la independencia entre el código de la aplicación y el interface.
 * Los errores están más acotados y son más fáciles de localizar.
 * Aceleran el desarrollo del software.
 * Ayudan a tener un código desacoplado.
 
Una vez leí que _Programar sin pruebas es cómo cacharrear en un panel eléctrico con un tenedor_ ;)
 
###Afirmaciones (Assert)
Son condiciones que el programador coloca en aquellos lugares en los que deben ser ciertas. De este modo se crean condiciones para la ejecución de ciertos bloques o líneas de código.

Por ejemplo:

	$semanas=5;
	$dias=3;
	$dias_totales=$dias + ($semanas * 7);
	echo("Han pasado $dias_totales días");
	// Assert ($dias_totales == 38)
	if($dias_totales != 38)
	{
		// Error de test
	}
	
	
###Glosario:

**Clase Mock, Mock Class o Mocked Class**

Es una clase creada especialmente para los tests. Simula a la clase real y nos permite pasar tests a nuestro código y a nuestra aplicación en un entorno controlado, e incluso hacer pruebas aunque la clase original aún no esté disponible (no tenemos conexión, no ha sido desarrollada aún, etc..).
	
###Más información:

[Estupendo tutorial paso a paso sobre la refactorización de código en PHP](http://jesuslc.com/2014/10/29/refactorizando-legacy-code-en-php-parte-1/)  
[Patrones para mejorar tests con PHP y PHPUnit – TDD](https://jesuslc.com/2014/07/04/patrones-para-mejorar-tests-con-php-y-phpunit-tdd/=
[Legacy code retreat](http://legacycoderetreat.typepad.com/)  
[Refactoring Legacy Code](http://code.tutsplus.com/series/refactoring-legacy-code--cms-633)	