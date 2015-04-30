###Definir relaciones externas en las migraciones:

Se definen con `$table->foreign('<claveprimaria>')->references('<claveprimaria_padre>')->on('<tabla_padre>');`

Por ejemplo, para relacionar la tabla actual con la `users` a través del campo `user_id`, este sería el código a añadir en la función `up()` de la migración:

`$table->foreign('user_id')->references('id')->on('users');`

Si además queremos que 	

onDelete('cascade')

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




 polymorphic relationship