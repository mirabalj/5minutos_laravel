###Definir relaciones externas en las migraciones:

Se definen con `$table->foreign('<claveprimaria>')->references('<claveprimaria_padre>')->on('<tabla_padre>');`

Por ejemplo, para relacionar la tabla actual con la `users` a través del campo `user_id`, este sería el código a añadir en la función `up()` de la migración:

`$table->foreign('user_id')->references('id')->on('users');`

Si además queremos que 	

onDelete('cascade')

###Definiendo las relaciones:

Suponiendo que tengamos las tablas `Tiendas`, `Clientes`, `Promociones`, `Pedidos` y `Facturas`. De modo que:

- Un **Pedido** sólo puede tener una **Factura**.

- Una **Factura** sólo puede tener un **Cliente**.

- Un **Cliente*+ puede tener una o varias **Facturas**.

- Una **Factura** sólo pertenece a un cliente. 

- Un **Cliente** puede tener activa una **Promoción**.

- Una **Promoción** puede estar activa para varios **Clientes**.

- En una **Factura** sólo puede aplicarse una **Promoción**.

- Una **Tienda** puede tener uno o varios **Clientes**.

- Un **Cliente** puede ir a una o varias **Tiendas**.

De modo que tenemos **Relaciones uno a uno** como Pedido y Factura. **Relaciones uno a varios** como Cliente y Factura. Y **Relaciones varios a varios** como Cliente y Tienda.

En Laravel, esas relaciones se definen de este modo:

- Relaciones uno a uno:
 - Factura **has_one** Promoción. (Factura tiene una Promoción)

- Cliente **has_one** Promocion. (Cliente tiene una Promoción)  

- 

- Cliente **has_many** Factura. (Cliente puede tener muchas Facturas)

- Factura **belongs_to** Cliente.(Factura pertenece a Cliente)

- Promoción **has_one** Factura . (Promoción tiene una Factura)  

- 