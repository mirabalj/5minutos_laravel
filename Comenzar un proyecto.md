###Comenzar una aplicación Laravel

####Crear la estructura de la aplicación.

Para crear la estructura inicial de la aplicación:

- Ve al directorio raíz de tu servidor web.

`cd <web_server_root_folder>`

- Crea tu apliación con composer:

 `composer create-project laravel/laravel PROJECT_NAME`
 

- 
- 
- - Crearla con composer o bien Clonar el repo.

- Crear una carpeta dentro de app con el nombre de la aplicación y meter ahí toda la lógica de la aplicación.

- Copiar los ficheros de la carpeta .idea de otra aplicación (Para el IDE PhpStorm)

##Pasos para configurar una aplicación:

- Para cambiar el namespace por defecto de nuestra aplicación

`php artisan app:name <NombreApp>`

- Crear la Base de datos en MySQL

- Configurar la Base de datos en el fichero .env

- Crear un junction en las carpetas de IIS.
		
- Añadir la aplicación a la configuración de IIS (**carpeta public**).

- Añadir el fichero web.config
      
Para crear las tablas necesarias en la base de datos, podemos ejecutar: php artisan migrate

###Inicializar proyectos Laravel

- Asegurarse de que en php.ini están habilitados los drivers para la BD que vayamos a usar, por ej:

	`extension=php_pdo_mysql.dll`

	Creará todas 

	Puedo crear la carpeta app\Http\Routes para crear ahí mis routes personalizadas
	
Para ejemplos sobre cómo funcionan las redirecciones, comprobaciones de login y de si la llamada es ajax, ver, el fichero: Authenticate.php

###Posibles errores

`require(<webfolder>\bootstrap/../vendor/autoload.php): failed to open stream: No such file or directory in <webfolder>\bootstrap\autoload.php on line 17`

Si acabas de clonar el proyecto, es porque hay que hacer un composer.update para que se cree la carpeta `vendor`.