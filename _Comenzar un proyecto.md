##Comenzar una aplicación Laravel

###Crear la estructura de la aplicación.

Para crear la estructura inicial de la aplicación:

- Ve al directorio raíz de tu servidor web.

`cd <web_server_root_folder>`

- Crea tu aplicación con composer:

 ```
composer create-project laravel/laravel PROJECT_NAME
```

 o si quieres usar la última versión del framework:

 ```
composer create-project laravel/laravel PROJECT_NAME dev-develop
```
 
 > Si quieres crear tu aplicación con las últimas versiones de un repositorio puedes bajar los requerimientos de estabilidad y añadir el parámetro `--stability=dev` o `--stability=beta`, por ejemplo.
 
- ¿¿Crear una carpeta dentro de app con el nombre de la aplicación y meter ahí toda la lógica de la aplicación.??

###Configura tu IDE:

- PhpStorm:

  - `Setting, Editor, Code Style, Scheme: Laravel`   
  (Bájate la plantilla de aquí: https://github.com/deringer/phpstorm-laravel-code-style) 

> Para PhpStorm, puedes copiar los ficheros de la carpeta .idea de otra aplicación o importar la configuración.

###Configura Git:
 
- Edita el fichero .gitattributes:
  Si vas a compartir el proyecto con otros usuarios que usen Linux: `* text=lf`

###Configura tu aplicación:

- Para cambiar el namespace por defecto de nuestra aplicación

```
php artisan app:name <NombreApp>
```
  
###Configura la Base de Datos:
- Crea la Base de datos en MySQL  
- Copia el fichero `.env.example` a `.env`  
```
copy .env.example .env
```
- Configura la Base de datos en el fichero .env  
  
###Instala los paquetes que sueles usar.

####Desarrollo:

**DebugBar**   
```
composer require barryvdh/laravel-debugbar --dev
```

**Ide Helper**

- Instala el paquete:

  ```
composer require barryvdh/laravel-ide-helper --dev
``` 
  

- Edita el fichero `config/app.php` y añade la siguiente línea al array `providers`:

  ```
'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider',
```

- Usa `php artisan` para generar el fichero `_ide_helper.php`:

  ```
php artisan ide-helper:generate
```

- Edita el fichero `composer.json` y añade la siguiente línea a la secciones `scripts`,`post-install-cmd` y `scripts`,`post-update-cmd`:

  ```
"php artisan ide-helper:generate",
```

  Debe quedar así:

  ```
"scripts": {
	"post-install-cmd": [
		"php artisan clear-compiled",
		"php artisan ide-helper:generate",
		"php artisan optimize"
	],
	"post-update-cmd": [
		"php artisan clear-compiled",
		"php artisan ide-helper:generate",
		"php artisan optimize"
	],
```

  Más información en: https://github.com/barryvdh/laravel-ide-helper


###Configura tu aplicación en el servidor Web:  

- Crear un symbolic link de tu proyecto en el web_server_root_folder.
		
- Añade la aplicación a la configuración de IIS (**Apuntando a la carpeta public**).

- Añade el fichero web.config


###Crea las Bases de datos:

- Para crear las tablas necesarias en la base de datos con el comando: `php artisan migrate`.  

> Si vas a utilizar el patrón repositorio, utiliza el comando `php artisan make:model` para crear al mismo tiempo las migraciones y los modelos.



###Inicializar proyectos Laravel

- Asegurarse de que en php.ini están habilitados los drivers para la BD que vayamos a usar, por ej:

	`extension=php_pdo_mysql.dll`


	
	Creará todas 

	Puedo crear la carpeta app\Http\Routes para crear ahí mis routes personalizadas
	
Para ejemplos sobre cómo funcionan las redirecciones, comprobaciones de login y de si la llamada es ajax, ver, el fichero: Authenticate.php

###Posibles errores

`require(<webfolder>\bootstrap/../vendor/autoload.php): failed to open stream: No such file or directory in <webfolder>\bootstrap\autoload.php on line 17`

Si acabas de clonar el proyecto, es porque hay que hacer un `composer.update` para que se cree la carpeta `vendor`.