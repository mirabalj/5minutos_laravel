Laravel_Migraciones

##Migraciones de datos

El sistema de migraciones de Laravel nos permite desde nuestra aplicaci�n y, s�lo con php, sin tener que usar sentencias SQL, crear, modificar y borrar tablas y registros en nuestras bases de datos.

La ventaja de usar un sistema de migraciones, es que permite realizar cambios en los datos a lo largo del tiempo de vida de la aplicaci�n, y gestionarlos con un control de versiones. Nos permite implementar un VCS (Version Control Sistem) igual que hacemos con nuestro c�digo fuente cuando usamos un repositorio Git, Mercury o Subversion. De este modo, si fuera necesario, podr�amos revertir esos cambios (hacer un Rollback) en cualquier momento y/o tener una visi�n de los cambios que se han ido realizando en la estructura de nuestra base de datos.

Por tanto, nos d� varias ventajas: La de facilitarnos la gesti�n de la estructura de la base de datos desde nuestra aplicaci�n y la mantener un historial de las modificaciones en esa estructural. Son ventajas que pueden simplificar mucho nuestro trabajo cuando trabajamos en equipo y tambi�n aportan seguridad a la hora de hacer cambios estructurales en bases de datos de producci�n.

Y, por supuesto, como su propio nombre indica, este sistema nos permite migrar nuestra base de datos a otros servidores, gestores de bases de datos o soportes manteniendo la integridad de los datos.

Es posible que en alg�n momento hayas programado aplicaciones completas sin usar un Sistema de Control de Versiones para tu c�digo fuente. Igualmente, puedes hacer lo mismo con las Bases de Datos. El sistema de migraciones **es totalmente opcional** y puedes desarrollar una aplicaci�n web (y/o con Laravel) sin utilizarlo. Aunque yo lo recomiendo por las much�simas ventajas que aporta.

##Las Migraciones en Laravel
### Configurar el sistema de migraciones.

Los scripts php de Laravel que forman parte del sistema de migraciones se encuentran en la carpeta `database\migrations`

En esa carpeta se encuentran las clases PHP que permiten a Laravel crear y actualizar la estructura de nuestra base de datos manteniendo un repositorio de versiones. 

> Para que el sistema funcione correctamente, se recomienda usar el comando `migrate:make` de __Artisan__ en lugar de crear las clases manualmente.

Cuando creamos un proyecto con Laravel 5, ya tenemos por defecto dos migraciones en esa carpeta:
	AAAA_MM_DD_000000_create_users_table.php
	AAAA_MM_DD_000000_create_password_resets_table.php
	
El primer fichero contiene la clase CreateUsersTable que nos permitir� crear una tabla 'est�ndar' de usuarios y el segundo, la clase CreatePasswordResetsTable que es una tabla auxiliar que utiliza Laravel en el sistema de recuperaci�n de contrase�as perdidas que trae implementado por defecto.

Si no vas a usar el sistema de recuperaci�n de contrase�as puedes borrar el fichero de migraciones correspondiente y, en caso de no tener una tabla en tus bases de datos puedes borrar tambi�n el fichero correspondiente.

####Estructura de los ficheros de migraciones.

Como pod�is ver en los nombres anteriores, los ficheros de migraciones tienen siempre esta estructura: `TimeStamp_Verbo_Tabla_table.php`.

* **TimeStamp** representa la fecha y hora en que se cre� la migraci�n y est� en el formato A�o (4 d�gitos), gui�n bajo, Mes (2 d�gitos), gui�n bajo, D�a (2 d�gitos), gui�n bajo, Hora (2 d�gitos), gui�n bajo, Minutos (2 d�gitos), gui�n bajo, Segundos (2 d�gitos).
* **Verbo** representa el verbo  &&&
* **Tabla** es el nombre de la tabla que se va a modificar en esa migraci�n.
* **table** y finalmente, acaban con la palabra '__table__'.

####Estructura de las clases de las migraciones.

Este es el contenido de la migraci�n `create_users_table.php`

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
 
Como pod�is ver, la clase encargada de la migraci�n hereda de `Illuminate\Database\Migrations\Migration`y tiene dos funciones: `up()` que se encarga de aplicar la migraci�n (en este caso, crear la tabla) y `down()` que se encarga de revertirla o hacer un __rollback__ (que en este caso ser�a borrar la tabla).

###M�todos disponibles en las migraciones.

Habitualmente para las migraciones llamaremos a algunos de estos m�todos de la clase Schema&&:

		// Crea una nueva tabla.
	public function create($table, Closure $callback)		
		// Borra una tabla.
	public function drop($table)
		// Borra una tabla si existe.
	public function dropIfExists($table)
		// Renombra una tabla
	public function rename($from, $to)
		// Modifica la estructura de una tabla.
	public function table($table, Closure $callback)

El primer par�metro de estos m�todos suele ser el nombre de la tabla. 
El segundo par�metro cuando tenemos que pasarle la estructura de la tabla es una **Closure** (Funci�n an�nima)  que recibe un objeto de tipo `Illuminate\Database\Schema\Blueprint` con el que definiremos la estructura.

###M�todos para definir la estructura.

Para definir la estructura de la tabla, lo haremos concatenando m�todos que nos permitir�n definir el tipo de la columna y asignarle las propiedades necesarias.

Algunos ejemplos ser�an:

		// Define una columna autoincremental llamado id.
	$table->increments('id');
		// Define una columna de tipo boolean llamado confirmed.
	$table->boolean('confirmed');
		// Define una columna de tipo cadena llamado email que no debe tener duplicados en la base de datos.
	$table->string('email')->unique()->index();
		// Define una columna de tipo fecha llamado created_ad.
	$table->timestamp('created_at');
		// Define una columna de tipo cadena llamado country que puede contener nulos
		// y tendr� por defecto el valor 'Spain'.
	$table->string('country')->nullable()->default('Spain');
			
	
La ventaja de usar estos m�todos, es que el c�digo es totalmente independiente del gestor de base de datos que estemos utilizando. Por ejemplo, una columna de tipo `boolean`ser�a declarado internamente como 'BOOLEAN' si us�ramos un gestor que soporte ese tipo de campos, y como 'integer', o 'bit' en gestores que no lo soporten.

Hay algunos m�todos especiales que incluye Laravel como:

		// Crea los campos de tipo fecha 
	$table->timestamps();


QQQQ



https://aprendelaravel.slack.com/archives/preguntas/p1425923357000019
https://aprendelaravel.slack.com/archives/preguntas/p1424374215000042
https://aprendelaravel.slack.com/archives/preguntas/p1424288833000002
https://aprendelaravel.slack.com/archives/preguntas/p1424337961000010
https://aprendelaravel.slack.com/archives/general/p1423667873000531
https://aprendelaravel.slack.com/archives/general/p1423667341000528
https://aprendelaravel.slack.com/archives/general/p1423620510000460
https://aprendelaravel.slack.com/archives/general/p1423613866000448
https://aprendelaravel.slack.com/archives/general/p1423601553000422
http://duilio.me/creando-migraciones-en-laravel-5/#comment-1941108799

QQQ

Imagino por tanto que al crear una nueva migraci�n lo que hacemos es automatizar el borrado de esa columna en la base de datos de producci�n de una forma organizada e incluso reversible. Es decir, posteriormente, gracias al sistema de migraciones de Laravel, podr�amos deshacer ese cambio si fuera necesario.

XXX

Una vez que creas una migraci�n y tienes 2 tablas relacionadas en este caso, sections y pages, me marca error al momento de hacer un rollback (porque estan relacionadas con la clave foranea), como le hago para poder borrarlas o actualizarlas? Saludos
 � Responder�Compartir � 
Avatar
Duilio Palacios Moderador  Edgar Morales � hace 7 meses
Tienes que borrarlas en el orden correcto, por ej. si p�ginas est� relacionada con secciones (en p�ginas tengo un seccion_id) deber�a borrar primero p�ginas (drop -> paginas) y luego borrar secciones.

Sino tambi�n puedes hacer:

DB::statement('SET FOREIGN_KEY_CHECKS = 0');

AAA

##Resumen:
### Ventajas

* Gesti�n de la estructura de la base de datos sin tener que usar sentencias SQL.
* Control de versiones de esos cambios para poder revertirlos, replicarlos, etc..
* Migrar nuestra base de datos a otros servidores.
* Cambiar nuestra base de datos a otro gestor de base de datos: PostgreSQL, SQL Server, SqlLite, Oracle, etc..
* Facilita mantener una versi�n actualizada de la base de datos cuando trabajamos en equipo.


**Fuentes**

**Migraciones**
[Migraciones en laravel - Curso de Laravel 5 de Duilio (Lecci�n 7)](http://duilio.me/migraciones-en-laravel/)   
[Migraciones - Documentaci�n oficial de Laravel 5](http://laraveles.com/docs/5.0/migrations)
[Migraci�n de Datos en la Wikipedia](http://es.wikipedia.org/wiki/Migraci%C3%B3n_de_datos)  
[Mejores pr�cticas para migraci�n de Bases de Datos](http://es.slideshare.net/carlosgruiz.arahat/mejores-prcticas-para-migracin-de-bases-de-datos)  

[Constructor de esquemas - Documentaci�n oficial de Laravel 5](http://laraveles.com/docs/5.0/schema)

**

**Otras**
[Funciones an�nimas o Closures en la documentaci�n oficial de PHP 5](http://php.net/manual/es/functions.anonymous.php)  
[Generador de Consultas en la documentaci�n oficial de Laravel 5](http://laraveles.com/docs/5.0/queries)
