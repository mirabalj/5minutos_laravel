###Seeders: Cargar información en la Base de Datos.

Los seeders son especialmente útiles para probar tu aplicación. Te permiten introducir la misma información en tu base de datos de forma automática.

Los seeders son clases que se almacenan en `database/seeds`.

Laravel incluye por defecto una clase llamada `DatabaseSeeder` en el fichero `database/seeds/DatabaseSeeder.php`.

Puedes usar esa clase para llamar al método `run()` del resto de clases de tipo seeder y de ese modo controlar el orden en el que se ejecutan.

###Crear una clase seeder.

- Crear la clase seeder:

 Para crear una clase seeder, símplemente crea una clase que extienda de `Illuminate\Database\Seeder`.

 Puedes usar cualquier nombre para la clase y para el fichero, aunque la convención suele ser un nombre descriptivo con notación `StudlyCaps` del tipo: `UserTableSeeder`.

 Añade un método `run()` que ejecute el código para añadir los nuevos registros.
 
 Por ejemplo:

 ```

 ```

> Como puedes ver en el código anterior, no olvides usar el método `bcrypt()` para encriptar los passwords antes de almacenarlos en la base de datos.

 Si hubieras creado un Modelo `User`, podrías usar este código en lugar del anterior:
 ```
class UserTableSeeder extends Seeder {

	public function run()
	{
		User::create([
			'name' => 'Juan Antonio Tubío',
			'email' => 'jatubio@gmail.com',
			'password' => bcrypt('admin'),
            'type' => 'admin'
		]);
	}
}
```
 


- Llamarla desde la clase `DatabaseSeeder`:
  
  Añade una línea al método `run()` de la clase `DatabaseSeeder` (en el fichero: `database/seeds/DatabaseSeeder.php`) para que ejecute a su vez el método `run()` de tu nueva clase:
  
  ```
$this->call('UserTableSeeder');
```

 > Para la clase `UserTableSeeder`, no es necesario añadir la línea, es suficiente con descomentar la línea que ya viene por defecto en el código de la clase `DatabaseSeeder`.

###Ejecutar la nueva clase seeder.

Para ejecutar los seeders usamos el comando `php artisan db:seed`.

Con ese comando, se ejecuta el método `run()` de la clase `DatabaseSeeder`.

> Puedes ejecutar una clase distinta con el parámetro --class. Por ejemplo: `php artisan db:seed --class=UserTableSeeder`.

Las clases de los directorios `database` y `seeds`, usan `PSR-0` en lugar de `PSR-4` para el sistema de autocarga. Por tanto, en estos momentos tu nueva clase aún 'no existe' en tu aplicación.

Para añadirla al sistema de autocarga, ejecuta:
```
composer dump-autoload
```

Ahora sí puedes ejecutar:
```
php artisan db:seed
```

Y si vas a la base de datos podrás comprobar que se han creado los registros que hayas definido en tu clase seeder.

> También puedes llenar tus datos al mismo tiempo que ejecutas tus migraciones, añadiendo el parámetro `--seed`. Por ejemplo: `php artisan migrate --seed` o `php artisan migrate:refresh --seed`.

###Tablas relacionadas.

Cuando queremos cargar datos en tablas relacionadas, podemos hacerlo en una única clase seeder.

Recibimos en una varible el `id` generado en la tabla padre, y lo usamos al crear los registros relacionados en las tablas hijos.

#ToDo:Ejemplo

###Faker: Generar datos falsos.

Si vas a usar pocos datos de prueba, puedes crearlos manualmente. A partir de cierta cantidad, es preferible usar un sistema automático.

**Faker** es un paquete que te permite generar datos de pruebas de forma automática.

Estos son los pasos para usarlo:

- Para instalar el paquete, ejecuta: 
```
composer require fzaninotto/Faker --dev
```

- Ahora, para usarlo en tus seeders, edita el fichero con la clase seeder y añade al comienzo la siguiente línea: 
```
use Faker\Factory as Faker;
```

Faker genera cada vez que se le llame un registro aleatorio con datos de pruebas.

Por tanto, si lo que queremos es ejecutar varios registros, podemos llamarlo dentro de un bucle `for`:




> Faker tiene proveedores para varios idiomas, entre ellos el castellano en versión Española (es_ES), Argentina (es_AR), Peruana (es_PE) y Venezolana (es_VE).
> Tan sólo tienes que indicarle el idioma cuando llamas al método `create`. Por ejemplo: `$faker = Faker\Factory::create('es_ES');`.
> Como no todos los métodos están traducidos aún, puedes comprobar cuales están disponibles en el [Directorio de proveedores](https://github.com/fzaninotto/Faker/tree/master/src/Faker/Provider).


> También puedes usar [faker-cli](https://github.com/bit3/faker-cli) para generar datos desde la línea de comandos.

###Vaciar los registros antes de ejecutar la migración.

Puedes usar la sentencia `truncate` de SQL para vaciar una tabla antes de ejecutar una migración. De ese modo, te evitas posibles errores por registros duplicados.

Puedes llamarla al comienzo del método `run()` de la migración, como en el siguiente código:

```
	public function run()
	{
		DB::table('users')->truncate();

		$this->createAdmin();
		$this->createUsers(50);
	}
```

###Vaciar los registros en tablas con relaciones.

Si hay alguna tabla que está relacionada con la tabla que vas a 'truncar' y has definido una 'constraint' o clave foránea en esa tabla, si tienes registros relacionados, te aparecerá un error de 'constraint' al intentar 'truncar' la tabla. Como este:

```
[Illuminate\Database\QueryException]
  SQLSTATE[42000]: Syntax error or access violation: 1701 Cannot truncate a table referenced in a foreign key constraint (`...`, CONSTRAINT `...` FOREIGN KEY (`...`) REFERENCES `...` (`...`)) (SQL: truncate `users`)
 ```
 
> **Nota:** Si usas MySQL con el formato MyISAM para crear las tablas, no obtendrás error alguno, ya que ese formato no admite claves foráneas. 
 
Para evitar ese error, hay que desactivar la comprobación de las claves foráneas.

Añade esta línea antes de llamar al método `truncate()`:
```
DB::statement('SET FOREIGN_KEY_CHECKS = 0');
```

Y esta otra después de truncar la tabla. Para dejar activada de nuevo esa comprobación:
```
DB::statement('SET FOREIGN_KEY_CHECKS = 1');
```

El código definitivo sería algo así:
```
DB::statement('SET FOREIGN_KEY_CHECKS = 0;');
DB::table('users')->truncate();
DB::statement('SET FOREIGN_KEY_CHECKS = 1;');
```

###Usando migraciones para llenar tus bases de datos.

Un inconveniente del sistema estándar de Laravel de Seeders, es que no es 'versionable' igual que el sistema de migraciones.

Si tus datos de prueba son pocos y los estás generando manualmente, puedes cargar los datos usando migraciones en lugar de seeders.

Es tan sencillo como crear una nueva migración y añadir los datos en el método `up()` y borrar esos mismos datos en el método `down()`.


###Errores Frecuentes.

- `Call to undefined method <seederClass>::setContainer()`

Si al ejecutar `php artisan db:seed` obtienes un mensaje de error similar a este:

```
PHP Fatal error:  Call to undefined method <seederClass>::setContainer() in <app_path>\vendor\laravel\framework\src\Illuminate\Database\Seeder.php on line 57

  [Symfony\Component\Debug\Exception\FatalErrorException]
  Call to undefined method <seederClass>::setContainer()
```

Comprueba que en la declaración de tu clase `<seederClass>` extiendes de la clase `Seeder`.

- `Class <seederClass> does not exist`

Si acabas de crear tu clase y el error que obtienes al ejecutar el seeder es este:

```
[ReflectionException]
  Class <seederClass> does not exist
```

Ejecuta `composer dump-autoload` para añadir tu nueva clase al sistema de autocarga de Laravel.

- `Column not found: 1054 Unknown column 'updated_at'`

 ```
  [Illuminate\Database\QueryException]
  SQLSTATE[42S22]: Column not found: 1054 Unknown column 'updated_at' in 'field list' (SQL: insert into `<table>` (`email`, `update
  d_at`, `created_at`) values (xxxx, 2015-05-15 17:15:35, 2015-05-15 17:15:35))


  [PDOException]
  SQLSTATE[42S22]: Column not found: 1054 Unknown column 'updated_at' in 'field list'
```

 Un mensaje de error similar a los anteriores se debe habitualmente a que no has creado los campos `created_at` y `updated_at` en la tabla `<table>`.

 Laravel espera que por defecto, existan esos campos.

 Tienes dos opciones para solucionar este error:

 1. Crear los campos:
 
	Añade la siguiente línea a tu migración para esa tabla: `$table->timestamps();` y ejecuta de nuevo la migración. (O crea una nueva migración)

 2. Configurar el modelo de esa tabla para que no use esos campos:
 
	Añade la siguiente línea al fichero de clase de tu Modelo:
	
	`public $timestamps = false;`

-------------

http://laravel.com/docs/5.0/migrations

https://github.com/fzaninotto/Faker

https://styde.net/seeders-y-el-componente-faker-en-laravel-5/

[fzaninotto/Faker](https://github.com/fzaninotto/Faker)

[faker-cli - Una herramienta de la línea de comandos para Faker](https://github.com/bit3/faker-cli)   


###ToDo:
Faker->paragraph(rand(2,5)),1.5.0,15:03