###Migraciones - Resumen

`php artisan make:model <modelo>` crea al mismo tiempo el modelo y la migración.

Por ejemplo, `php artisan make:model Cliente` creará los siguientes archivos:

- `app/Cliente.php`

 ```
<?php namespace Curso;

use Illuminate\Database\Eloquent\Model;

class Cliente extends Model {

	//

}
```

- `database/migrations/2015_04_30_145531_create_clientes_table.php`

```
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateClientesTable extends Migration {

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
		Schema::drop('clientes');
	}

}

```



> Recuerda escribir los nombres de los modelos con la primera letra en mayúsculas y en singular.

Los modelos se crean por defecto en la carpeta `app`. Es una buena práctica tener una carpeta propia para los modelos. Por ejemplo, si vas a usar el patrón 'Repositorio', puedes llamar a esa carpeta 'Entities'.


###Glosario:

- 'Boilerplate' es el nombre que se usa para el código que genera Laravel por defecto. Por ejemplo generas un modelo. También se le suele llamar 'Stub'.
