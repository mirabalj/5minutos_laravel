##Eloquent. Diferencia entre `fillable` y `guarded`.

Ambas propiedades se usan en los 'asignamientos de datos en masa' en los modelos Eloquent y permiten protegerse en caso de que un usuario 'malicioso' haya modificado los campos del formulario para incluir campos que existían.

Estas propiedades te permiten especificar qué datos se asignarán al modelo en los métodos en los que se usan 'asignamientos en masa'. Y, por tanto, qué datos se guardarán posteriormente en la base de datos.

Los métodos en los que se utilizan estas propiedades son: `create(array[])`, `new(array[])`, `firstOrNew(array[])` y `firstOrCreate(array[])`.

> **Nota:** Ten en cuenta que cuando se asignan valores a las propiedades directamente, no se comprueban las propiedades `$fillable` ni `$guarded` del modelo.

`$guarded` permite especificar qué campos no queremos que se asignen al modelo.
Es decir, se asignan **todos** excepto los especificados en este array.

```
<?php
 // ...
 class User extends Eloquent{
      public $timestamps = false;
      protected $guarded = ['is_admin'];
 } 
```

Y `$fillable` te permite especificar qué campos sí quieres que se guarden en la base de datos.
Es decir, se asignan **únicamente** los especificados en este array.

```
<?php
 // ...
 class User extends Eloquent{
      public $timestamps = false;
      protected $fillable = ['name', 'email'];
 }
```

Ambos son excluyentes y, por tanto, deberías utilizar sólo uno de ellos. Si declaras ambos, sólo se tendrá en cuenta el contenido de `$fillable`.
 
La diferencia fundamental entre usar uno u otro, está en cómo Laravel procesa ambas propiedades. Si usamos `$fillable`, cada vez que modifiquemos un formulario para añadir un nuevo campo, tendremos que añadirlo al array `$fillable` para que lo procese. Sin embargo, usando `$guarded` en un modelo, cada campo que añadimos a un formulario es procesado automáticamente e insertado en la Base de Datos. Esto aunque es muy cómodo, tiene varios inconvenientes:

- Si se nos olvida incluir un campo `crítico` que no debería ser modificado desde el formulario, un usuario malicioso podría aprovechar la vulnerabilidad.
- Si añadimos campos auxiliares del flujo del formulario que no existen en la base de datos, obtendremos un error de SQL porque Eloquent asumirá que existen e intentará asignarles el valor.

Por tanto, es recomendable utilizar `$fillable` en lugar de `$guarded`.

Por último, un apunte más: Este proceso puede deshabilitarse o habilitarse temporalmente utilizando los métodos `unguard()` y `reguard()`. Y es útil para cuando necesitamos realizar un asignamiento en masa desde la propia aplicación sin tener en cuenta estas reglas. Si has utilizado los `Seeders`, ahora ya sabes para qué sirve la siguiente línea que aparece en la clase `DatabaseSeeder`:

```
class DatabaseSeeder extends Seeder {

	/**
	 * Run the database seeds.
	 *
	 * @return void
	 */
	public function run()
	{
		Model::unguard();

		(...)
```
Si deseas deshabilitar permanentemente este sistema de protección, declara ambas propiedades con un array vacío en tu modelo:

```
	protected $guarded =[];

	protected $fillable = [];
```

De ese modo, Laravel asignará a tu modelo todos los atributos recibidos excluyendo aquellos que comiencen por un guión bajo `_`, como por ejemplo `_token`.

> **Nota:** Por defecto el valor de $guarded es `[*]`. De modo que mientras el array `$fillable` esté vacío, tu aplicación estará **blindada** contra los asignamientos en masa y, por tanto, protegida.
> Si intentas usar los asignamientos en masa sin declarar `$guarded` como un array vacío o rellenar el array `$fillable`, recibirás una excepción de tipo `MassAssignmentException`.
> 
> **Si sigues los pasos anteriores para deshabilitar esta protección, implementa algún mecanismo propio que mantenga tu aplicación protegida.**


Puedes obtener más información de todo el proceso, estudiando el método `fill()` en el archivo   
`vendor\laravel\framework\src\Illuminate\Database\Eloquent\Model.php`.

####Más información.

En esta página tienes una implementación de este sistema de protección adaptado a varios perfiles/contextos: http://laravel.io/bin/MOBxQ
No confundas estas propiedades con `$hidden` que se usa en el proceso de serialización a JSON del modelo. (http://laravel.com/docs/master/eloquent-serialization#hiding-attributes-from-json)