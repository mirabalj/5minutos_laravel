##Probando las migraciones y los Rollback.

Si quieres probar un Rollback en las migraciones. Puedes realizar los siguientes pasos:

###Creando una nueva columna 'phone' en la tabla users.
Ejecutas `php artisan make:migration users_add_phone`   

Editas el nuevo fichero `AAAA_MM_DD_HHMMSS_users_add_phone.php`:  

	public function up()
	{
			// Creo un campo nuevo para el teléfono
		Schema::table('users', function(Blueprint $table)
		{
			$table->string('phone',35);
		});
	}

	public function down()
	{
			// Borro el nuevo campo creado
		Schema::table('users', function(Blueprint $table)
		{
			$table->dropColumn('phone');
		});
	}
	
Ejecuta `php artisan migrate` para añadir la nueva columna.

Puedes comprobar en la base de datos en la tabla `users` que ahora hay una nueva columna `phone`.

###Renombrando la columna 'name' a first_name
Ejecuta `php artisan make:migration users_rename_name`   

Edita el nuevo fichero `AAAA_MM_DD_HHMMSS_users_rename_name.php`:  

	public function up()
	{
			// Renombro la columna name a first_name
		Schema::table('users', function(Blueprint $table)
		{
			$table->renameColumn('name','first_name');
		});
	}

	public function down()
	{
			// Vuelvo a renombrar la columna con su antiguo nombre
		Schema::table('users', function(Blueprint $table)
		{
			$table->renameColumn('first_name','name');
		});
	}

Ejecuta `php artisan migrate` para añadir la nueva columna.

Puedes comprobar en la base de datos en la tabla `users` que ahora hay una la columna `name` se llama `first_name`.

###Volviendo a la situación inicial
Ejecuta `php artisan migrate:rollback` dos veces, para revertir cada una de las migraciones anteriores.
