###Migraciones en Laravel - Ejecutar Migraciones.

Este post es la continuación del post [Migraciones en Laravel - Introducción](Migraciones-en-Laravel---Introducci%C3%B3n) y en él vamos a ver con más detalle cómo escribir una migración y cómo ejecutarla.

###Métodos disponibles en las migraciones.

Estos son los métodos que usaremos dentro de los ficheros de migraciones en las funciones `up()` y `down()`.

Habitualmente para las migraciones llamaremos a algunos de estos métodos de la clase Schema:

```
// Crear una nueva tabla.
public function create($table, Closure $callback)		

// Borrar una tabla.
public function drop($table)

// Borrar una tabla si existe.
public function dropIfExists($table)

// Renombrar una tabla
public function rename($from, $to)

// Modifica la estructura de una tabla.
public function table($table, Closure $callback)
```

El primer parámetro de estos métodos suele ser el nombre de la tabla. 

El segundo parámetro cuando tenemos que pasarle la estructura de la tabla es una **Closure** (Función anónima)  que recibe un objeto de tipo `Illuminate\Database\Schema\Blueprint` con el que definiremos la estructura.

###Métodos para definir la estructura.

Para definir la estructura de la tabla, lo haremos concatenando métodos que nos permitirán definir el tipo de la columna y asignarle las propiedades necesarias.

Algunos ejemplos serían:

```
// Definir una columna autoincremental llamado id.
$table->increments('id');

// Definir una columna de tipo boolean llamada confirmed.
$table->boolean('confirmed');

// Definir una columna de tipo cadena llamado email que no debe tener duplicados en la base de datos.
$table->string('email')->unique()->index();

// Definir una columna de tipo fecha llamado created_ad.
$table->timestamp('created_at');

// Definir una columna de tipo cadena llamado country que puede contener nulos
// y tendrá por defecto el valor 'Spain'.
$table->string('country')->nullable()->default('Spain');
```			

Si volvemos a revisar el contenido de la migración para crear la tabla de usuarios `create_users_table.php`.

```
	<?php

	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;

	class CreateUsersTable extends Migration {

		/**
		 * Run the migrations.
		 *
		 * @return void
		 */
		public function up()
		{
			Schema::create('users', function(Blueprint $table)
			{
				$table->increments('id');
				$table->string('name');
				$table->string('email')->unique();
				$table->string('password', 60);
				$table->rememberToken();
				$table->timestamps();
			});
		}

		/**
		 * Reverse the migrations.
		 *
		 * @return void
		 */
		public function down()
		{
			Schema::drop('users');
		}

	}
```

Ves que en la función `up()` crea la tabla llamando a `Schema::create()` y en la _closure_ añade un campo incremental llamado 'id', un campo de tipo cadena llamado 'name', otro campo de tipo cadena llamado 'email' que queremos que no se pueda repetir, y otro para el 'password' de 60 caracteres de largo.

Laravel incluye también algunos métodos especiales como:

```
// Crear dos columnas de tipo fecha llamadas created_at y updated_at que permiten registrar cambios en los datos.
$table->timestamps();

// Crear la columna deleted_at para borrados lógicos.
$table->softDeletes();

// Igual que timestamps(), excepto que permite valores NULL.
$table->nullableTimestamps();

// Crear un campo auxiliar para la funcionalidad de 'recuérdame' en el login.
$table->rememberToken();
```

También podemos añadir, modificar y borrar índices:

```
// Añadir una columna de tipo cadena llamada key como clave primaria.
$table->primary('key');

// Añadir una columna de tipo cadena llamada email creando un índice sin duplicados.
$table->string('email')->unique();

// Añadir una columna de tipo cadena llamada name con un índice básico. 
$table->string('name')->index();
```

En el ejemplo anterior, Laravel creará dos índices para la tabla `Users`, uno sin duplicados en el campo `email` y otro de clave primaria en el campo `id`. (Este último es porque Laravel como convención gestiona automáticamente el campo `id` como clave primaria).

La ventaja de usar estos métodos, es que el código es totalmente independiente del gestor de base de datos que estemos utilizando. Por ejemplo, una columna de tipo `boolean` será declarada internamente como 'BOOLEAN' si usamos un gestor que soporte ese tipo de campos, y como 'integer', o 'bit' en gestores que no lo soporten.

###Métodos para modificar la estructura.

Puedes modificar la estructura de una tabla llamando al método `Schema::table($table, Closure $callback)` y utilizando los siguiente métodos en la closure:

```
Schema::create('users', function(Blueprint $table)
{
    // Modificar el tamaño de la columna name a 50 caracteres.
    $table->string('name', 50)->change();

	// Renombrar la columna fullname a name
    $table->renameColumn('fullname', 'name');

	// Borrar las columnas weight y height
    $table->dropColumn(['weight', 'height']);
}
```

> Para poder renombrar columnas en las migraciones, hay que instalar el paquete `Doctrine DBAL`. Ya que como indica la documentación de Laravel, ese paquete ya no está incluido en la instalación por defecto.

 - Puedes hacerlo manualmente:

   Para incluir el paquete, edita el archivo `composer.json` que se encuentra en la raíz de tu proyecto. Y añade la siguiente línea a la opción `require`: `"doctrine/dbal": "~2.3"`

   ```
	“require”: {
	  "laravel/framework": "5.0.*",
	  "doctrine/dbal": "~2.3"
	},
 ```

  **Nota:** Tienes que añadir la coma final en la línea anterior.
 
  Después ejecuta el comando `composer install` para instalar el nuevo paquete.
  
 - O con el comando: `composer require doctrine/dbal:~2.3`.

> **Nota:** Si al ejecutar una migración te aparece el error `[Symfony\Component\Debug\Exception\FatalErrorException]  Class 'Doctrine\DBAL\Driver\PDOMySql\Driver' not found` probablemente es porque no tienes instalado el paquete Doctrine DBAL.

Puedes ver todos los métodos y opciones en la [documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/schema).

###Crear una nueva migración.

Para crear una migración, simplemente tienes que ejecutar el comando: `php artisan make:migration nombre_migracion`.

Se creará un fichero `.php` en el directorio `database\migrations` con el nombre especificado precedido por una marca de tiempo con la fecha y hora actual. 

Ese fichero contendrá una clase con ese nombre y con los métodos `up()` y `down()` vacíos.

Por ejemplo, con el comando `php artisan make:migration change_users_table`, obtendremos un fichero llamado  `AAAA_MM_DD_000000_change_users_table.php` y en él se definirá la clase `class ChangeUsersTable extends Migration { }`

Una migración en Laravel consiste únicamente en ese fichero `.php` creado. Por lo que si en algún momento quieres borrar una migración **que aún no hayas ejecutado**, tan sólo tienes que borrar el fichero correspondiente.

Por tanto, también se pueden crear migraciones manualmente. Sin embargo, se recomienda usar el comando `php artisan make:migration` porque es un método menos propenso a errores.

Cuando quieres hacer cambios en la estructura de la base de datos y **aún no has ejecutado los cambios** de la última migración en el servidor de producción o de desarrollo, puedes ahorrarte crear una nueva migración y, en su lugar, ejecutar un rollback, modificar la última migración con los nuevos cambios y lanzar de nuevo esa migración.

**Parámetros:**

- `--table`

 Si añades el parámetro `--table=nombre_de_la_tabla`, Laravel creará por ti, el código para modificar la estructura de la tabla en ambas funciones.

 Por ejemplo, con el comando `php artisan make:migration create_contacts_table --table=contacts`, Laravel creará la siguiente función `up()` y una función `down()` con el mismo contenido:

 ```
public function up()
{
    Schema::table('contacts', function(Blueprint $table)
    {
        //
    });
}
``` 

> `php artisan make:model <modelo>` te permite crear al mismo tiempo el modelo y la migración. Los modelos se crean por defecto en la carpeta `app`. Aunque una buena práctica tener una carpeta propia para los modelos. Por ejemplo, si vas a usar el patrón 'Repositorio', para crear un modelo llamado cliente podrías ejecutar `php artisan make:model Entities/Cliente` y guardar tus modelos en la carpeta 'Entities'.

###Ejecutar migraciones

El comando `php artisan migrate` te permite ejecutar todas las migraciones pendientes. Es decir, ejecuta todos los scripts que se encuentren en la carpeta `database\migrations` y que aún no se hayan ejecutado.

Al ejecutar ese comando, obtendrás un mensaje que te indicará qué scripts se han ejecutado.

> **Nota:** Si recibes un error `class not found` cuando ejecutas las migraciones, ejecuta el comando `composer dump-autoload` e inténtalo de nuevo. (Tienes más información en [xxxx])

**Parámetros:**

- `--force`

 Cuando estamos en un entorno de producción (`APP_ENV=production` en el fichero `.env`), Laravel nos pedirá confirmación para ejecutar o deshacer migraciones que impliquen una posible pérdida de datos. Si queremos que no pida confirmación y las ejecute directamente, podemos añadir el parámetro `--force` al comando.
Por ejemplo: `php artisan migrate --force`.

- `--pretend`

Si quieres estar seguro de qué cambios va a realizar la migración en tu base de datos antes de ejecutarla, puedes añadir el parámetro `--pretend` y en lugar de ejecutar la migración, Laravel te mostrará las sentencias SQL que ejecutarían esa(s) migración(es).

- `--database`

 Te permite especificar la base de datos en la que se deben ejecutar las migraciones.

- `--path`

 Te permite usar una ruta distinta de `database\migrations` para ejecutar las migraciones.

###Glosario:

- Al código que genera Laravel por defecto cuando ejecutas un comando. Como el que genera genera para las migraciones o modelos. También se le suele llamar '**Boilerplate**' o '**Stub**'.

###Fuentes y más información:

[Migraciones en Laravel - Introducción](Migraciones-en-Laravel---Introducci%C3%B3n)

[Styde.net - Creando Migraciones en Laravel 5](https://styde.net/creando-migraciones-en-laravel-5/)  

[Migraciones - Documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/migrations)

[Constructor de esquemas - Documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/schema)  

[Funciones anónimas o Closures en la documentación oficial de PHP 5](http://php.net/manual/es/functions.anonymous.php)  

[Database Abstraction Layer — Doctrine Project](http://www.doctrine-project.org/projects/dbal.html)  
