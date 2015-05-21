###Migraciones en Laravel - Rollback.

En el [post anterior](Migraciones-en-Laravel---Ejecutar-Migraciones), vimos cómo ejecutar una migración. En este post vamos a ver cómo podemos deshacer una migración que hemos ejecutado.

En el entorno de bases de datos, se usa el término `Rollback` para referirse al proceso de deshacer una transacción.

###Deshacer una migración (Rollback)

El comando `php artisan migrate:rollback` te permite deshacer la última migración ejecutada. 

Puedes ejecutar `php artisan migrate:reset` para deshacer todas las migraciones.

###La tabla migrations

Ya has visto que es muy sencillo deshacer una migración con Laravel. Ahora bien, ¿Cómo sabe Laravel qué tablas estaban implicadas en esa migración? 

Además de los scripts en el directorio `database\migrations`, al ejecutar la primera migración, Laravel crea la tabla `migrations` en nuestra base de datos para almacenar el **Repositorio** de las migraciones. Esa tabla contiene información sobre qué scripts han sido ejecutados en esa base de datos, y en qué orden.

La tabla `migrations` tiene dos columnas: `migration` y `batch`.

Cada vez que ejecutamos una migración, se crea un registro por cada una de las migraciones ejecutadas. En el campo `migration` se introduce el nombre del script ejecutado. Y el campo `batch` es un contador del número de migraciones ejecutadas hasta ahora. De ese modo, el sistema de migraciones puede conocer el orden en el que se han ejecutado esas migraciones y ejecutar los `rollback` en orden inverso.

> Una vez que has ejecutado una migración, si borras o renombras el fichero de la migración, un `rollback` que afecte a esa migración te dará error. A no ser que modifiques esta tabla manualmente y borres el registro que hace referencia a ese fichero.

En el siguiente apartado veremos un ejemplo del contenido de esa tabla después de ejecutar algunas migraciones.

Puedes especificar otro nombre para la tabla modificando la línea   `'migrations' => 'migrations'`, del fichero `config\database.php` y especificando un nuevo nombre: `'migrations' => 'nuevo_nombre'`.

###Probando las migraciones y los Rollback.

Si quieres probar como funciona el sistema de Rollback en las migraciones, sígueme:

- Antes de empezar, ejecuta `php artisan migrate` para que se ejecuten las migraciones por defecto: `create_users_table` y `create_password_resets_table`.

- Creando una nueva columna `phone` en la tabla `users`.

 1. Ejecuta `php artisan make:migration users_add_phone`.

 2. Edita el nuevo fichero `AAAA_MM_DD_HHMMSS_users_add_phone.php` e introduce estas líneas: 

	```
	public function up()
	{
		// Crear un campo nuevo para el teléfono
		Schema::table('users', function(Blueprint $table)
		{
			$table->string('phone',35);
		});
	}

	public function down()
	{
		// Borrar el nuevo campo creado
		Schema::table('users', function(Blueprint $table)
		{
			$table->dropColumn('phone');
		});
	}
	```

	Como ves, usamos `Schema::table` en lugar de `Schema::create` para modificar la estructura de una tabla.

 3. Ejecuta `php artisan migrate` para añadir la nueva columna.

   Puedes comprobar en la base de datos que la tabla `users` tiene ahora una nueva columna llamada `phone`.

- Instalando el paquete `Doctrine DBAL`.

 Como en el siguiente paso vamos a renombrar una columna, tal y como comentamos en el post anterior, esta funcionalidad la proporciona el paquete `Doctrine DBAL` que no viene instalado por defecto.

 Para instalar el paquete, ejecuta el comando: `composer require doctrine/dbal`.
 
- Renombrando la columna `name` a `first_name`.

 1. Ejecuta `php artisan make:migration users_rename_name`.   

 2. Edita el nuevo fichero `AAAA_MM_DD_HHMMSS_users_rename_name.php` y añade este código:  

	```
	public function up()
	{
		// Renombrar la columna name a first_name
		Schema::table('users', function(Blueprint $table)
		{
			$table->renameColumn('name','first_name');
		});
	}

	public function down()
	{
		// Volver a renombrar la columna con su antiguo nombre
		Schema::table('users', function(Blueprint $table)
		{
			$table->renameColumn('first_name','name');
		});
	}
```

 3. Ejecuta `php artisan migrate` para añadir la nueva columna.

 4. Puedes comprobar en la base de datos que en la tabla `users` la columna `name` a pasado a llamarse `first_name`.

   Si además echas un vistazo a la tabla migrations, verás estos registros:
   
| *migration*                                     	| *batch* 	|
|-------------------------------------------------	|---------	|
| 2014_10_12_000000_create_users_table            	| 1       	|
| 2014_10_12_100000_create_password_resets_table  	| 1       	|
| 2015_04_30_001943_users_add_phone               	| 2       	|
| 2015_04_30_002300_users_rename_name             	| 3       	| 

 Como ves, se ha creado un registro por cada una de las migraciones ejecutadas y el campo `batch` permite al sistema de migraciones conocer el orden en el que se han ejecutado esas migraciones.

- Volviendo a la situación inicial.

 Para deshacer las dos últimas migraciones, ejecuta `php artisan migrate:rollback` dos veces. 
 
 Puedes comprobar en la base de datos que la tabla `Users` ha vuelto a su estado original. 

 Y puedes borrar los dos ficheros que hemos creado anteriormente.

###Otros comandos de migraciones.

Estos son otros comandos relacionados con las migraciones:

- `migrate:status`

 Muestra todas las migraciones indicando cuales han sido ejecutadas.

- `migrate:install`

 Creará la tabla migrations en nuestra base de datos. Habitualmente no es necesario porque si no existe, se crea automáticamente al 
ejecutar las migraciones.

- `migrate:refresh`

 Deshace todas las migraciones y las ejecuta otra vez. Puede ser útil si tienes que modificar alguna de las migraciones.
 
 > **Este comando BORRARÁ todos tus datos**. Usa este comando únicamente en entornos de desarrollo y cuando no tengas datos valiosos en la base de datos.

- `migrate:reset`

 Deshace todas las migraciones.

 > **Este comando BORRARÁ todos tus datos**. Usa este comando únicamente en entornos de desarrollo y cuando no tengas datos valiosos en la base de datos.

<br/> #ToDo:Check Style
> Si tienes creadas las clases de poblado de datos, puedes llenar tus tablas de datos al mismo tiempo que ejecutas tus migraciones añadiendo el parámetro `--seed`. Por ejemplo: `php artisan migrate --seed`.

###Aplicaciones prácticas:

####¿Puedo usar una nomenclatura propia para los nombres de los ficheros de las migraciones?

La respuesta la tienes en el código del framework de Laravel, en la clase `Migrator` que está en el fichero: `framework/src/Illuminate/Database/Migrations/Migrator.php`.

Investigando en esa clase y concretamente en el método  `getMigrationFiles()`, puedes ver que asume que los nombres de los ficheros empezarán por un timestamp y por tanto, los **ordena alfabéticamente** y los ejecuta en ese orden. De modo, que, **en principio**, puedes seguir cualquier convención, nomenclatura o nombres siempre que los archivos se ejecuten en el orden que necesites cuando sean ordenados alfabéticamente.

Sin embargo, si deseas elegir otra notación o nomenclatura para los nombres, tienes que tener en cuenta otra restricción. En ese mismo fichero, el método `resolve()` es el encargado de resolver el nombre de la clase de tu migración a partir del nombre del fichero.  Y tomará como nombre de esa clase todo lo que haya después del cuarto carácter `_`. 

Por tanto, las restricciones son:

- Puedes utilizar cualquier convención que incluya cuatro guiones bajos `_` antes del nombre de la migración.

- El nombre de la migración debe coincidir con el nombre de la clase declarada en el fichero.

- Recuerda que los ficheros se ordenarán alfabéticamente.

####Hacer Rollback sólo de una migración.

Cuando estás desarrollando tu aplicación, es habitual crear nuevas migraciones y hacer varias veces `rollback` de esa migración mientras la pruebas. Esos pasos son sencillos:

1. Ejecutar: `php artisan migrate`.
2. Comprobar los cambios en la base de datos.
3. Ejecutar: `php artisan migrate:rollback`.
4. Modificar la migración para solucionar los posibles errores.
5. Volver al paso 1 mientras la migración no funcione correctamente.

Finalmente tu migración funciona perfectamente y este es el estado de la tabla `migrations`:

| *migration*                                     	| *batch* 	|
|-------------------------------------------------	|---------	|
| 2014_10_12_000000_create_users_table            	| 1       	|
| 2014_10_12_100000_create_password_resets_table  	| 1       	|
| 2015_04_30_001943_users_add_phone               	| 2       	|
| 2015_04_30_002300_users_rename_name             	| 3       	| 



Imagina ahora que en algún momento has ejecutado `php artisan migrate:reset` y después `php artisan migrate`. O bien, `php artisan migrate:refresh`. En ese caso, todas tus migraciones han acabado configuradas juntas en la misma transacción:

| *migration*                                     	| *batch* 	|
|-------------------------------------------------	|---------	|
| 2014_10_12_000000_create_users_table            	| 1       	|
| 2014_10_12_100000_create_password_resets_table  	| 1       	|
| 2015_04_30_001943_users_add_phone               	| 1       	|
| 2015_04_30_002300_users_rename_name             	| 1       	| 

Si ahora quieres modificar únicamente la última migración, no puedes ejecutar `php artisan migrate:rollback` por que haría `rollback` de todas las migraciones.

El truco en este caso, es editar la tabla `migrations` y cambiar el campo `batch` a un número mayor que todos los existentes:

| *migration*                                     	| *batch* 	|
|-------------------------------------------------	|---------	|
| 2014_10_12_000000_create_users_table            	| 1       	|
| 2014_10_12_100000_create_password_resets_table  	| 1       	|
| 2015_04_30_001943_users_add_phone               	| 1       	|
| 2015_04_30_002300_users_rename_name             	| 2       	| 

De ese modo, si ahora ejecutas `php artisan migrate:rollback`, sólo los scripts de la última transacción (la 2) serán ejecutados en ese rollback.

Por supuesto, puedes aplicar este truco para agrupar o separar tus migraciones en las transacciones que quieras. O incluso para hacer rollback de migraciones anteriores aunque no sean la última. En este último caso, antes de ejecutar la migración, comprueba las posibles inconsistencias que podría provocar.

> **Nota:** Si prefieres un método más afín al sistema de migraciones, puedes crear una nueva migración dejando vacío el método `up()` y copiar en el método `down()` el código de la migración que quieres deshacer. Ejecutas la migración, y, de nuevo, creas una migración completa de la forma habitual, con las nuevas modificaciones en el método `up()` y el código para deshacer esas modificaciones en el método `down()`.

####Preservando las tablas actuales antes de ejecutar una migración.

Puede haber ocasiones en las que una migración va a realizar grandes modificaciones en la estructura de las tablas y queremos mantener los datos y la estructura de las tablas actuales antes de realizar esa migración.

En ese caso podemos crear una migración “especial” que copie o haga un backup de la estructura actual de la base de datos desde un modelo Eloquent a otro. La buena noticia es que con Laravel es tan sencillo como esto:

```
use Illuminate\Database\Migrations\Migration;

use NuevoModelo;

class DataConvert extends Migration {

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        foreach(AntiguoModelo::all() as $item)
        {
            NuevoModelo::create(array(...));
        }

    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        NuevoModelo::truncate();
    }

}
```

####Utilizando las migraciones en tablas relacionadas.

Cuando tenemos varias tablas relacionadas, el modo más sencillo de crear las migraciones es crear una migración por cada tabla y hacerlo en el orden correcto: Primero la migración para crear/borrar la tabla padre y después la migración para la tabla hija.

Pero si tenemos un sistema de base de datos complejo, quizás tratemos de mantener el menor número de ficheros de migraciones posibles. En ese caso, puedes escribir una única migración, pero colocando los procesos de creado y borrado de tablas en el orden adecuado dentro de las funciones `up()` y `down()`.

Por ejemplo, si vamos a crear una tabla `Clientes` y una tabla `Facturas` hija de `Clientes`, el código de la migración sería este:

```
<?php
 
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
 
class CreateProjectsAndTasksTables extends Migration {
 
	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('clientes', function(Blueprint $table)
		{
			$table->increments('id');
			$table->string('nombre')->default('');
            (...)
			$table->timestamps();
		});
 
		Schema::create('facturas', function(Blueprint $table) {
			$table->increments('id');
			$table->integer('clientes_id')->unsigned()->default(0);
			$table->foreign('clientes_id')->references('id')->on('clientes')->onDelete('cascade');
			$table->text('descripcion')->default('');
            (...)
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
		Schema::drop('facturas');
		Schema::drop('clientes');
	}
 
}
```

> De este modo, te evitarás el error "Integrity constraint violation: 1452" al hacer un Rollback.
> Si a pesar de todo, sigue apareciéndote ese error, puedes añadir esta línea al comienzo de tus migraciones: `DB::statement('SET FOREIGN_KEY_CHECKS=0;')` antes de hacer el Rollback.

###Algunas utilidades que te facilitarán las migraciones:

- [Xethron/migrations-generator](https://github.com/Xethron/migrations-generator)  
Este paquete te permitirá generar tus migraciones automáticamente a partir de la estructura de tus bases de datos.

- [nWidart/DbExporter](https://github.com/nWidart/DbExporter)  
Te permite exportar tus bases de datos actuales como migraciones de Laravel, y los datos como Seeders.

###Errores comunes

- PHP Fatal error:  Class '<NombreDeTuClase>'.

Si al ejecutar tu migración te aparece un error similar a este:

```
PHP Fatal error:  Class '<NombreDeTuClase>' not found in <app_path>\vendor\laravel\framework\src\Illuminate\Database\Migrations\Migrator.php on line 328

  [Symfony\Component\Debug\Exception\FatalErrorException]
  Class '<NombreDeTuClase>' not found

```

  - Ejecuta `composer dump-autoload` si no lo habías hecho.
  - Comprueba que el nombre del fichero de migración es igual al nombre de la clase <NombreDeTuClase>.

### Ventajas de usar el sistema de migraciones:

* Gestión de la estructura de la base de datos sin tener que usar sentencias SQL.

* Control de versiones de esos cambios para poder revertirlos, replicarlos, etc..

* Migrar con facilidad nuestra base de datos a otros servidores.

* Cambiar nuestra base de datos a otro gestor de base de datos: PostgreSQL, SQL Server, SqlLite, Oracle, etc..

* Facilita mantener una versión actualizada de la base de datos cuando trabajamos en equipo.

###Fuentes y más información:

[Migraciones en Laravel - Introducción](Migraciones-en-Laravel---Introducci%C3%B3n)

[Migraciones en Laravel - Ejecutar Migraciones](Migraciones-en-Laravel---Ejecutar-Migraciones)

[Styde.net - Creando Migraciones en Laravel 5](https://styde.net/creando-migraciones-en-laravel-5/)  

[Funciones anónimas o Closures en la documentación oficial de PHP 5](http://php.net/manual/es/functions.anonymous.php)  

[Migraciones - Documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/migrations)  

[Constructor de esquemas - Documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/schema)   

[Database Abstraction Layer — Doctrine Project](http://www.doctrine-project.org/projects/dbal.html)  