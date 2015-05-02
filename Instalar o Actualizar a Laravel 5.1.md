###Laravel 5.1 ya está aquí!!

Y, si eres tan impaciente como yo, probablemente quieras empezar a probarlo en tus aplicaciones.

Probar Laravel 5.1 es muy sencillo:

###Crear una nueva aplicación con Laravel 5.1

Si creas una nueva aplicación con Composer utilizando el modo habitual: `composer create-project laravel/laravel PROJECT_NAME`, obtendrás las siguientes versiones instaladas:

```
laravel/laravel                       v5.0.22 The Laravel Framework. 
laravel/framework                     v5.0.16 The Laravel Framework.
```

Dado que aún no hay una versión estable de Laravel 5.1, se instalan las últimas versiones estables disponibles para ambos paquetes.

Además, el 'require' de `laravel/laravel` es `laravel/framework: 5.0.*`. Eso quiere decir que siempre se instalará una versión **menor** que la 5.1.

Si bajamos la estabilidad mínima requerida a `dev`: `composer create-project laravel/laravel PROJECT_NAME --stability=dev`, obtendrás las siguientes versiones instaladas:

```
laravel/laravel                    dev-master The Laravel Framework. 
laravel/framework                     v5.0.28 The Laravel Framework.
```

Hemos conseguido instalar la versión `v5.0.28` del Framework, pero no la `5.1`.

Eso es porque si te vas al [fichero](https://github.com/laravel/laravel/blob/master/composer.json)  `composer.json` de la rama `dev-master`, verás que el require del Framework sigue siendo: `laravel/framework: 5.0.*`.

El truco está en `forzar` a instalar la rama `dev-develop`:  

```
composer create-project laravel/laravel PROJECT_NAME dev-develop
```

Ahora si ejecutas `composer show --installed` verás la siguiente línea:

```
laravel/framework          dev-master ea0fc7f The Laravel Framework.
```

Por fín hemos conseguido instalar la última versión del Framework de Laravel. Como ves, es una versión de desarrollo `dev-master`, y el siguiente código `ea0fc7f` es el Hash del commit que hemos instalado, que en tu caso puede ser distinto. Esto quiere decir que aún es una versión inestable y te puede dar errores. Y sobre todo: **No es una versión para instalar en producción**.

###Actualizar una aplicación a Laravel 5.1

Si ya tienes una aplicación y quieres actualizarla a Laravel 5.1:

Edita tu fichero `composer.json` y cambia los requerimientos del framework para laravel 5.1:
```
	"require": {
		"laravel/framework": "5.1.*"
	},
```

Y añade las siguientes líneas al final de tu fichero para cambiar tus requerimientos mínimos de estabilidad a desarrollo (`dev`) pero, cuando sea posible, que se instalen versiones estables:

```
	"minimum-stability": "dev",
	"prefer-stable": true
```

>**Nota:** Asegúrate de añadir una coma después de la llave anterior a `minimum-stability`:   
>
>```	
"config": {
		"preferred-install": "dist"
	},
	"minimum-stability": "dev",
	"prefer-stable": true
}
```
<br>

> Tienes en mi [Gist](https://gist.github.com/jatubio/e51415c59008cc2cc956) un nuevo fichero `composer.json` adaptado a Laravel 5.1. Pero ten en cuenta que si ya has instalado paquetes en tu aplicación, tendrás que editar tu fichero en lugar de reemplazarlo.

Ahora, ejecuta `composer update` para que actualice todos tus paquetes incluído el framework de Laravel a la versión 5.1

¡Ohhh! ¿Qué ha pasado?
Es probable que al final del proceso de actualización tengas un error como este: 

```
PHP Fatal error:  Call to undefined method Illuminate\Foundation\Application::getCachedCompilePath() in Ruta_DE_TU_PROYECTO\vendor\laravel\framework\src\Illuminate\Foundation\Console\ClearCompiledCommand.php on line 28
```

El proceso de actualización se ha llevado a cabo, pero han fallado los scripts encargados de actualizar los ficheros de autocarga de Laravel.

Eso es porque hay algunos cambios en la estructura de la aplicación respecto a Laravel 5.0 y cuando creamos un proyecto nuevo, esos cambios ya por defecto, pero al actualizar una aplicación, tenemos que hacer manualmente los cambios.

No te preocupes porque son muy sencillos:

- Crea el directorio `bootstrap/cache` y asígnale permisos de escritura.

- Dentro de ese directorio, crea un fichero `.gitignore` con estas dos líneas:

 ```
*
!.gitignore
```

- Por último, edita tu fichero `bootstrap/autoload.php` y actualiza la línea que contiene la variable `$compiledPath` como en la siguiente línea:

```
$compiledPath = __DIR__.'/cache/compiled.php';
```

Ahora ya tenemos adaptada nuestra aplicación a Laravel 5.1. Sólo nos falta ejecutar `composer dump-autoload`. Para que se generen los ficheros de autocarga.

Y listo!!

> Para tener la certeza de que todo está correcto, puedes ejecutar de nuevo `composer update` y esta vez deberías obtener símplemente el mensaje de: `Nothing to install or update`.

A disfrutar de tus nuevas aplicaciones con Laravel 5.1!

> Ahora que sabes cómo actualizar la estructura de tu aplicación a Laravel 5.1, puedes adaptarla antes de modificar el fichero `composer.json` y ejecutar sólo una vez `composer update` para actualizarla.

Para más información sobre el proceso de actualización, puedes ir a la documentación oficial: http://laravel.com/docs/master/upgrade

Y aquí tienes un adelanto a las novedades de Laravel 5.1: https://laravel-news.com/2015/04/laravel-5-1/