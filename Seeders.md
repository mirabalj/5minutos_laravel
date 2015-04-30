###Seeders: Cargar información en la Base de Datos.

Los seeders son especialmente útiles para probar tu aplicación. Te permiten introducir la misma información en tu base de datos de forma automática.

Los seeders son clases que se almacenan en `database/seeds`.

La clase principal es la clase `DatabaseSeeder` que viene por defecto y está en el fichero `database/seeds/DatabaseSeeder.php`.

Esa clase se encarga de ejecutar el resto de clases de tipo seeder.



###Tablas relacionadas.

Cuando queremos cargar datos en tablas relacionadas, podemos hacerlo en una única clase seeder.

Recibimos en una varible el `id` generado en la tabla padre, y lo usamos al crear los registros relacionados en las tablas hijos.




###Faker: Generar datos falsos.

Si vas a usar pocos datos de prueba, puedes crearlos manualmente. A partir de cierta cantidad, es preferible usar un sistema automático.

**Faker** es un paquete que te permite generar datos de pruebas de forma automática.

Estos son los pasos para usarlo:

- Para instalar el paquete, ejecuta: `composer install fzaninotto/Faker`.

- Ahora, para usarlo en tus seeders, edita el fichero con la clase seeder y añade al comienzo: `use Faker\Factory as Faker;`

Faker genera cada vez que se le llame un registro aleatorio con datos de pruebas.

Por tanto, si lo que queremos es ejecutar varios registros, podemos llamarlo dentro de un bucle `for`:



> Faker tiene proveedores para varios idiomas, entre ellos el castellano en versión Española (es_ES), Peruana (es_PE) y Venezolana (es_VE).
> Tan sólo tienes que indicarle el idioma cuando llamas al método `create`. Por ejemplo: `$faker = Faker\Factory::create('es_ES');`.
> Como no todos los métodos están traducidos aún, puedes comprobar cuales están disponibles en el [Directorio de proveedores](https://github.com/fzaninotto/Faker/tree/master/src/Faker/Provider).


> También puedes usar [faker-cli](https://github.com/bit3/faker-cli) para generar datos desde la línea de comandos.


http://laravel.com/docs/5.0/migrations

https://github.com/fzaninotto/Faker

https://styde.net/seeders-y-el-componente-faker-en-laravel-5/

[fzaninotto/Faker](https://github.com/fzaninotto/Faker)

[faker-cli - Una herramienta de la línea de comandos para Faker](https://github.com/bit3/faker-cli)   


###ToDo:
Faker->paragraph(rand(2,5)),1.5.0,15:03