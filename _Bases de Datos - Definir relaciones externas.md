###Definir relaciones externas en las migraciones:

Se definen con `$table->foreign('<claveprimaria>')->references('<claveprimaria_padre>')->on('<tabla_padre>');`

Por ejemplo, para relacionar la tabla actual con la `users` a través del campo `user_id`, este sería el código a añadir en la función `up()` de la migración:

`$table->foreign('user_id')->references('id')->on('users');`

Si además queremos activar el 'borrado en cascada' automático de los registros, concatenamos al final de la línea una llamada al método:

`onDelete('cascade')`

> Un borrado en cascada es un término habitual en bases de datos para decir que cuando se borre un registro padre, se borren todos sus registros hijos. Por ejemplo, si borramos un Cliente, se borrarían todas sus Facturas asociadas.

###Definiendo las relaciones:

Suponiendo que tengamos las tablas `Tiendas`, `Clientes`, `Pedidos` y `Facturas`. De modo que:

- Un **Pedido** sólo puede tener una **Factura**.

- Una **Factura** sólo puede tener un **Pedido**.

- Un **Cliente* puede tener una o varias **Facturas**.

- Una **Factura** sólo pertenece a un cliente. 

- Una **Tienda** puede tener uno o varios **Clientes**.

- Un **Cliente** puede ir a una o varias **Tiendas**.

De modo que tenemos **Relaciones uno a uno** como Pedido y Factura. **Relaciones uno a varios** como Cliente y Factura. Y **Relaciones varios a varios** como Cliente y Tienda.

En Laravel, esas relaciones se definen de este modo:

- Relaciones uno a uno:

 - Pedido **has_one** Factura. (Pedido tiene una Factura)

 - Factura **has_one** Pedido. (Factura tiene un Pedido)
 
- Relaciones uno a varios:

 - Cliente **has_many** Factura. (Cliente tiene varias Facturas)

 - Factura **belongs_to** Cliente.(Factura pertenece a Cliente)
 
- Relaciones varios a varios:

- Tienda **belongs_to_many** Cliente. (Tienda pertenece a varios Cliente)

- Cliente **belongs_to_many** Tienda.(Cliente pertenece a varios Tienda)

Para las relaciones **uno a uno** y **uno a varios**, creamos un campo clave (clave foránea o 'foreign key') en la tabla hija que se relaciona con el campo clave de la tabla padre. Si la relación es **varios a varios**, se crea una tabla intermedia (tabla pivot o 'pivot table') con una clave foránea para cada tabla de la relación.

Por tanto, y siguendo las [Convenciones de Laravelxxxx]() para nombrar los modelos y tablas, tendremos las siguientes tablas:

 `tiendas,clientes,pedidos,facturas y clientes_tiendas`.

###Migraciones con relaciones
 
Estas serían las correspondientes migraciones: (En orden para que no tengamos errores si ejecutamos un RollBack)

- `database\migrations\2015_04_04_200000_create_tiendas_table.php`

 ```
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateTiendasTable extends Migration
{
    /**
     * Run the migrations.
     */
    public function up()
    {
        Schema::create('tiendas', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down()
    {
        Schema::drop('tiendas');
    }
}
```

<br>
- `database\migrations\2015_04_04_200100_create_clientes_table.php`  

 ```


```


<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateTicketVotesTable extends Migration
{
    /**
     * Run the migrations.
     */
    public function up()
    {
        Schema::create('ticket_votes', function (Blueprint $table) {
            $table->increments('id');

            $table->integer('user_id')->unsigned();
            $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');

            $table->integer('ticket_id')->unsigned();
            $table->foreign('ticket_id')->references('id')->on('tickets')->onDelete('cascade');

            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down()
    {
        Schema::drop('ticket_votes');
    }

 
 polymorphic relationship
 
 
 ###Gestionar el borrado de registros en tablas relacionadas.

¿Qué ocurre cuando borras un registro de una tabla padre que tiene asociados registros en una tabla hija? Por ejemplo, si borras un Cliente que tiene facturas.

Hay varios métodos para gestionar este tipos de escenarios.

- Restricciones o Constraints en la Base de Datos.

El método más 'puro' consiste en gestionar ese escenario a nivel de Base de Datos. Se crea una restricción o 'constraint' en la base de datos para indicar que no pueden borrarse registros padres mientras tengan hijos. La mayoría de gestores de bases de datos gestionan esto automáticamente creando índices y constraints cuando defines campos incrementales y relaciones entre las tablas.

Por ej, cuando usas los métodos 'xxxx' en Laravel, se crean esas constraints.
#ToDo:Ejemplo

De ese modo, si intentas borrar un registro padre que tiene hijos, obtendrás un 'error de constraints'. Y a la hora de borrar registros, tendrás que hacerlo siempre de forma ordenada borrando primero los hijos y después los padres para evitar ese error.

En la mayoría de gestores de datos se puede definir también lo que se llama un 'Borrado en cascada' que consiste en que cuando borras un registro padre, el sistema de la base de datos borra automáticamente sus registros hijos asociados.

Por ej, en Laravel, podemos configurar borrados en cascada usando el método `->onDelete('cascade');` cuando definimos la estructura de nuestra base de datos en las migraciones.
#ToDo:Ejemplo

