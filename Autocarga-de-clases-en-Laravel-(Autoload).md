##Autocarga de clases en Laravel (Autoload)
En este post vamos a ver con detalle el sistema de autocarga de clases de Laravel 5. (La mayoría de lo expuesto es aplicable también a Laravel 4)

Laravel utiliza **Composer** para generar los ficheros .php que se encargan del proceso de autocarga de las clases.

Vamos a ver un poco de historia sobre la carga de archivos (**include**) y los sistemas de autocarga en php:

###Autocarga en PHP5
Para usar las clases definidas en otros ficheros .php distintos al que estamos ejecutando, tenemos que usar la directiva `include` al comienzo de cada uno de nuestros ficheros.

A partir de PHP 5, se introdujo un nuevo sistema de 'Autocarga de clases' (Autoload) que permite usar nuestras clases sin tener que escribir los 'include' correspondientes. 

La primera implementación, fue crear una función global llamada `__autoload()`. Cada vez que nuestro código hace referencia a una clase que no existe, se llama a la función `__autoload()`. Y nosotros podemos implementar esa función para definir un sistema personalizado de carga de clases. Por ejemplo:

**Ejemplo #1 Ejemplo de autocarga**

Este ejemplo intenta cargar las clases MiClase1 y MiClase2 desde los ficheros MiClase1.php y MiClase2.php respectivamente.

	<?php
	function __autoload($nombre_clase) {
		include $nombre_clase . '.php';
	}

	$obj  = new MiClase1();
	$obj2 = new MiClase2();
	?>

Esa primera implementación tenía el inconveniente de que la función `__autoload()` sólo podía definirse una vez en todo nuestro código. Por eso, a partir de PHP 5.1.2, se creó un nuevo método que permitía flexibilizar la Autocarga de clases: La nueva función `spl_autoload_register()`, que permite definir múltiples funciones de autocarga y llamarlas dentro de una cola o lista de funciones de autocarga.

Este es un posible ejemplo de uso que aparece en la documentación oficial de PHP:

**Ejemplo #1 spl_autoload_register() como sustituto de una función __autoload()**

	<?php

	// function __autoload($clase) {
	//     include 'clases/' . $clase . '.clase.php';
	// }

	function mi_autocargador($clase) {
		include 'clases/' . $clase . '.clase.php';
	}

	spl_autoload_register('mi_autocargador');

	// O, usando una función anónima a partir de PHP 5.3.0
	spl_autoload_register(function ($clase) {
		include 'clases/' . $clase . '.clase.php';
	});

	?>

Ahora ya teníamos un método flexible y eficiente para la autocarga de clases, pero hay muchas formas de usar e implementar ese método.

###Estándar PSR-0
	
Por eso, el php-fig (Grupo de interoperatibilidad para Frameworks PHP), ha creado y propuesto un estándar común para la autocarga de clases. El primer estándar que crearon se llamó **PSR-0**. A partir del 21/10/2014 el estándar **PSR-0** se ha marcado como obsoleto y recomiendan **PSR-4** como alternativa.

El estándar PSR-0, proponía una analogía entre el sistema de clases y el sistema de ficheros. Los Namespaces se convierten en directorios y las clases en ficheros con la extensión __.php__. De modo que:

La clase `Illuminate\Database\Seeder` se convierte en `Illuminate/Database/Seeder.php`

PSR-0 se definió para que fuera compatible con el sistema para nombrar las clases en __PEAR__. De modo, que también están soportados los guiones bajos en los nombres de las clases:
 
La clase `Illuminate_Database_Seeder` se convierte en `Illuminate/Database/Seeder.php`

###Estándar PSR-4

PSR-4 es una evolución del estándar PSR-0.

El estándar PSR-4 mantiene la misma idea de crear una relación directa entre el sistema de clases y el sistema de ficheros que proponía PSR-0.

Las diferencias más notables son que ya no es compatible con __PEAR__ (ya no convierte el guión bajo en una barra o slash) y que ahora, puedes 'abreviar' o reducir el nombre de las rutas cuando defines las clases.

Para eso, en el fichero composer.json, puedes especificar que ciertos Namespaces, se carguen desde una carpeta concreta.

	{
		"autoload": {
			 "psr-4": {
				"Namespace1": "ruta_a_la_carpeta1"
			 }
		}
	}

Vamos a verlo mejor con un ejemplo:

Si tienes una estructura de directorios como esta:

	app.php
	composer.phar
	vendor/
	modules/
		mail/
			src/
				Custom/
					Systems/
						Mail.php		# Mail.php define la clase: Custom\Systems\Mail
			tests/
				Custom/
					Systems/
						MailTest.php	# MailTest.php define la clase: Custom\Systems\MailTest

Y, por ejemplo, el fichero `Mail.php` define la clase: `Custom\Systems\Mail`:

	<?php namespace Custom\Systems;
	
	class Mail {
		
	}

En PSR-0, **tienes que mantener** esa estructura de directorios y usar esta configuración en el fichero `composer.json`:

	{
		"autoload": {
			 "psr-0": {
				"Custom\\Systems\\Mail": "modules/mail/src/",
				"Custom\\Systems\\Mail": "modules/mail/tests/"
			 }
		}
	}

Sin embargo, con PSR-4, podrías ahorrarte las carpetas Custom y Systems


	app.php
	composer.phar
	vendor/
	modules/
		mail/
			src/
				Mail.php		# Mail.php define la clase: Custom\Systems\Mail
			tests
				MailTest.php	# MailTest.php define la clase: Custom\Systems\MailTest
			
Con esta configuración en el fichero `composer.json`:

	{
		"autoload": {
			 "psr-4": {
				"Custom\\Systems\\": "modules/mail/src/Custom/Systems",
				"Custom\\Systems\\": "modules/mail/tests/Custom/Systems"
			 }
		}
	}

De modo que le estás diciendo que las clases que cargue desde las carpetas `modules/mail/src/Custom/Systems` y `modules/mail/test/Custom/Systems` tendrán el namespace `Custom\Systems`.

> Fíjate en cómo los namespaces acaban con dos barras dobles invertidas `'\\\'`. 
> Esto no es obligatorio pero suele ser una convención para evitar errores cuando luego automáticamente se concatenan 
> esas cadenas a otras.

Estos cambios se llevaron a cabo para facilitar la 'convivencia' entre paquetes de distintos proveedores instalados en una aplicación.

###Composer

Composer tiene su propia implementación de autocarga de clases que admite PSR-0 y PSR-4, además de un sistema de mapa de clases (classmap) y un sistema para cargar ficheros individuales (files)

>La ventaja de usar PSR-4 en nuestros proyectos es que podemos usar **Composer** para que cargue automáticamente nuestras clases.

Las clases que definamos en la clave 'psr-4' de la propiedad `autoload` del fichero `composer.json`, se procesan durante los comandos `install` y `update` para crear un array asociativo para cargar esas clases.

	{
		"autoload": {
			"psr-4": {
				"Monolog\\": "src/",
				"Vendor\\Namespace\\": ""
			}
		}
	}
	
Ese array se crea en el fichero: `vendor/composer/autoload_psr4.php`

Y las clases referenciadas por 'psr-0', en el fichero: `vendor/composer/autoload_namespaces.php`

En la clave `classmap` dentro de `autoload`, podemos definir rutas o ficheros. Durante los comandos `install` y `update` se buscarán clases en todos los ficheros `.php` y `.inc` que encuentre en esas carpetas para crear un array asociativo con esas clases.

	"autoload": {
		"classmap": [
			"database"
		]
	},

En este caso, el array se crea en el fichero: `vendor/composer/autoload_classmap.php`

Por último, también podemos añadir ficheros que necesitamos que se carguen siempre que ejecutemos nuestra aplicación pero no puede incluirse en ninguno de los casos anteriores. Habitualmente, porque no son ficheros que declaren clases. Para eso usamos la clave `files`.

    "autoload": {
        "files": ["app/miconfig.php"]
    }

Y ese array irá al fichero `vendor/composer/autoload_files.php`

####El comando composer dump-autoload

Cuando creemos nuevas clases en nuestros proyectos, debemos añadirlas al fichero composer.json y podemos
usar `composer dump-autoload` para refrescar los ficheros 'autoload'. (A no ser que estén en rutas referenciadas por el estándar `PSR-4`*)

>Si usamos `classmap`, al crear nuevas clases o ficheros tendremos que actualizar siempre los ficheros 'autoload'.

>Sin embargo, otra de las ventajas de usar `PSR-4` es que si las nuevas clases o ficheros están en directorios que ya estaban incluidos en el fichero `composer.json`, serán cargadas automáticamente.

Como hemos visto, los comandos `install` y `update` de composer crean los ficheros necesarios para el mecanismo de autocarga de clases de php.

Cuando añadimos referencias a nuevas clases en nuestro fichero `composer.json` en lugar de usar esos comandos, podemos usar el comando `dump-autoload` que vuelve a generar los ficheros 'autoload' sin tener que instalar o actualizar los paquetes ya instalados. Es decir, `dump-autoload` **no descarga nada**. Simplemente vuelve a generar el listado de todas las clases que necesitan ser incluidas en tu proyecto.

>Si usamos el parámetro `--optimize`, todas las clases referenciadas con el estándar `PSR-4` se incluirán en el fichero `vendor/composer/autoload_classmap.php` de modo que en tiempo de ejecución, php puede cargarlas directamente y no tiene que buscarlas en las carpetas optimizando mucho los tiempos de carga de la aplicación.

**El comando `dump-autoload` admite dos parámetros:**

- `--optimize (-o)`: Convierte las clases referenciadas en `PSR-0` y `PSR-4` a clases `classmap` para obtener un autoloader más rápido. Está recomendado especialmente para entornos de producción. Según el número de clases y tamaño de tu proyecto, puede tardar un poco más en ejecutarse y por eso no está activado por defecto.

- `--no-dev`: No tiene en cuenta las clases referenciadas en la clave: `autoload-dev`.

### **Error:** [ReflectionException] Class UserTableSeeder does not exist

Si aparece este error al ejecutar tu aplicación Laravel, es posible que hayas creado una nueva clase y no esté correctamente referenciada en el fichero `composer.json` y __Laravel__ no consigue cargarla.

##Fuentes:

###Fuentes y más información:

[Curso básico de Laravel 5: Qué es PSR-4 y uso de los namespaces](http://duilio.me/curso-de-laravel-5-que-es-psr-4-y-uso-de-los-namespaces)  

[Autoload en la documentación oficial de PHP](http://php.net/autoload)  

[Estándar PSR-0](http://www.php-fig.org/psr/psr-0/es/)  

[spl_autoload_register() en la documentación oficial de PHP](http://php.net/manual/es/function.spl-autoload-register.php)      
	
<br>
**Más información (En Inglés):**    

[dump-autoload en la Documentación Oficial de Composer](https://getcomposer.org/doc/03-cli.md#dump-autoload)  

[autoload en la Documentación Oficial de Composer](https://getcomposer.org/doc/04-schema.md#autoload)  

[php-fig](http://www.php-fig.org)  

[Estándar PSR-4](http://www.php-fig.org/psr/psr-4/)  

[Ejemplos de implementación de PSR-4](http://www.php-fig.org/psr/psr-4/examples/)  

[Stackoverflow: What exactly is the difference between PSR-0 and PSR-4?](http://stackoverflow.com/questions/24868586/what-exactly-is-the-difference-between-psr-0-and-psr-4)  

[Battle of the Autoloaders: PSR-0 vs. PSR-4](http://www.sitepoint.com/battle-autoloaders-psr-0-vs-psr-4/)  

[PSR-4 Autoloading](https://laracasts.com/lessons/psr-4-autoloading)   

[Composer Command-line interface / Commands - Documentación Oficial](https://getcomposer.org/doc/03-cli.md)   