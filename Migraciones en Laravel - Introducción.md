###Migraciones en Laravel - Introducción.

En este post vamos a ver qué son las migraciones de datos y cómo Laravel las gestiona.

##Migraciones de datos.

El sistema de migraciones de Laravel nos permite crear, modificar y borrar tablas y registros en nuestras bases de datos. Desde nuestra aplicación y sólo con php. Sin tener que utilizar sentencias SQL.

La ventaja de usar un sistema de migraciones, es que permite realizar cambios en los datos a lo largo del tiempo de vida de la aplicación, y gestionarlos con un control de versiones. 

Es decir, nos permite implementar un VCS (Version Control System) igual que hacemos con nuestro código fuente cuando usamos un repositorio Git, Mercury o Subversion. De este modo, si fuera necesario, podríamos revertir esos cambios (hacer un Rollback) en cualquier momento y/o tener una visión de los cambios que se han ido realizando en la estructura de nuestra base de datos.

Por tanto, nos da varias ventajas: La de facilitarnos la gestión de la estructura de la base de datos desde nuestra aplicación y la de mantener un historial de las modificaciones en esa estructura. Son ventajas que pueden simplificar mucho nuestro trabajo cuando trabajamos en equipo y también aportan seguridad a la hora de hacer cambios estructurales en bases de datos de producción.

Y, por supuesto, como su propio nombre indica, este sistema nos permite migrar nuestra base de datos a otros servidores, gestores de bases de datos o soportes manteniendo la integridad de los datos.

Es posible que en algún momento hayas programado aplicaciones completas sin usar un Sistema de Control de Versiones para tu código fuente. Igualmente, puedes hacer lo mismo con las Bases de Datos. 

> El sistema de migraciones **es totalmente opcional** y puedes desarrollar una aplicación web (y/o con Laravel) sin utilizarlo. Aunque es recomendable por las ventajas que aporta.

##Las Migraciones en Laravel
### Configurar el sistema de migraciones.

Los scripts php de Laravel que forman parte del sistema de migraciones se encuentran en la carpeta `database\migrations`.

En esa carpeta se encuentran las clases PHP que permiten a Laravel crear y actualizar la estructura de nuestra base de datos manteniendo un repositorio de versiones. 

> Para que el sistema funcione correctamente, se recomienda usar el comando `migrate:make` de __Artisan__ en lugar de crear las clases manualmente.

Cuando creamos un proyecto con Laravel 5, ya tenemos por defecto dos migraciones en esa carpeta:

```
AAAA_MM_DD_000000_create_users_table.php
AAAA_MM_DD_000000_create_password_resets_table.php
```
	
El primer fichero contiene la clase `CreateUsersTable` que nos permitirá crear una tabla 'estándar' de usuarios y el segundo, la clase `CreatePasswordResetsTable` que es una tabla auxiliar que utiliza Laravel en el sistema de recuperación de contraseñas olvidadas que trae implementado por defecto.

Si no vas a usar el sistema de recuperación de contraseñas, puedes borrar el fichero de migraciones correspondiente y, en caso de no tener una tabla de usuarios en tus bases de datos puedes borrar también el fichero correspondiente.

####Estructura de los ficheros de migraciones.

Como puedes ver en los nombres anteriores, los ficheros de migraciones tienen siempre esta estructura: 

`<TimeStamp>_<Verbo>_<Tabla>_table.php`.

- `<TimeStamp>` representa la fecha y hora en que se creó la migración y está en el formato `AAAA_MM_DD_000000`. Año (4 dígitos), guión bajo, Mes (2 dígitos), guión bajo, Día (2 dígitos), guión bajo, Hora (2 dígitos), guión bajo, Minutos (2 dígitos), guión bajo, Segundos (2 dígitos).

El resto corresponde con el nombre que indicamos cuando creamos una migración y en el caso de los dos ficheros comentados, sigue la siguiente convención (cada elemento se separa con un guión bajo):
 
- `Verbo` representa el verbo principal de la acción. Por ejemplo: `create`,`rename` o `delete` en inglés o `crear`,`renombrar` o `borrar` en castellano.

- `<Tabla>` es el nombre de la tabla que se va a modificar en esa migración.

- Y finalmente, acaban con la palabra '__table__'.

> Esa estructura es una convención y no es obligatoria por lo que eres libre de utilizar el sistema de nombres que consideres oportuno.

####Estructura de las clases para las migraciones.

Este es el contenido de la migración para crear la tabla de usuarios `create_users_table.php`.

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
 
Como puedes ver, la clase encargada de la migración hereda de `Illuminate\Database\Migrations\Migration` y tiene dos funciones: `up()` que se encarga de aplicar la migración (en este caso, crear la tabla) y `down()` que se encarga de revertirla o hacer un __rollback__ (que en este caso sería borrar la tabla).

Dentro de `up()` escribimos todos los pasos que sean necesarios para tener nuestra tabla con la estructura deseada. Y en `down()` es muy importante que escribamos los pasos necesarios para devolver la tabla al estado en que estaba antes de ejecutar `up()`. Dependiendo de la complejidad de la función `up()`, habitualmente será suficiente con escribir los métodos 'antagónicos' en orden inverso.

> Es muy importante que todas las modificaciones que se realicen en la función `up()` se deshagan en la función `down()` para no tener un estado inconsistente en nuestra base de datos en caso de que ejecutemos algún rollback.  

> Dado que cuando creas la migración será cuando tengas más reciente los nuevos cambios en la estructura de la base de datos y cómo se integrarán en la base de datos existente, se recomienda implementar también en ese momento la función down().

En el próximo post veremos cómo crear y ejecutar nuestras propias migraciones.

###Fuentes y más información:

[Styde.net - Creando Migraciones en Laravel 5](https://styde.net/creando-migraciones-en-laravel-5/)  

[Migración de Datos en la Wikipedia](http://es.wikipedia.org/wiki/Migraci%C3%B3n_de_datos)  

[Migraciones - Documentación oficial de Laravel 5](http://laravel.montogeek.co/5.0/migrations)  

[Mejores prácticas para migración de Bases de Datos](http://es.slideshare.net/carlosgruiz.arahat/mejores-prcticas-para-migracin-de-bases-de-datos)  