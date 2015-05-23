###Instalar o Actualizar tu aplicación a Laravel 5.1

####Crear una nueva aplicación con Laravel 5.1

Hasta que lancen la versión estable, instala la rama `dev-develop`:  

```
composer create-project laravel/laravel PROJECT_NAME dev-develop
```

###Actualizar una aplicación a Laravel 5.1

Si ya tienes una aplicación y quieres actualizarla a Laravel 5.1:

1. Crea el directorio `bootstrap/cache` y asígnale permisos de escritura.

2. Dentro de ese directorio, crea un fichero `.gitignore` con estas dos líneas:

 ```
*
!.gitignore
```

3. Edita tu fichero `bootstrap/autoload.php` y actualiza la línea que contiene la variable `$compiledPath` como en la siguiente línea:

 ```
$compiledPath = __DIR__.'/cache/compiled.php';
```

4. Edita tu fichero `composer.json` y cambia los requerimientos del framework para laravel 5.1:
 ```
	"require": {
		"laravel/framework": "5.1.*"
	},
```

 Y añade las siguientes líneas al final :

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

 > Tienes en este [Gist](https://gist.github.com/jatubio/e51415c59008cc2cc956) un nuevo fichero `composer.json` adaptado a Laravel 5.1. Pero ten en cuenta que si ya has instalado paquetes en tu aplicación, tendrás que editar tu fichero en lugar de reemplazarlo.

5. Genera de nuevo los ficheros de autocarga:

 ```
composer dump-autoload
```

 > Para tener la certeza de que todo está correcto, puedes ejecutar de nuevo `composer update` y esta vez deberías obtener símplemente el mensaje de: `Nothing to install or update`.

Tienes información más detallada sobre el proceso en el artículo que he publicado en Styde.net: [Cómo instalar o actualizar una aplicación a Laravel 5.1](https://styde.net/como-crear-o-actualizar-una-aplicacion-a-laravel-5-1/)
