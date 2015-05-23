##Estándares de programación en PHP (PSR-1 y PSR-4)

*TLDR:* Puedes ir directamente a un [Resumen de estos estándaes](https://github.com/jatubio/5minutos_laravel/wiki/Estandares-de-programacion.-Laravel).

He querido comenzar esta serie por uno de los aspectos, habitualmente más descuidados, y, sin embargo, más importantes de la programación: El estilo del código.

El estilo del código es especialmente importante si estamos en un equipo de desarrollo o si nuestro proyecto lo van a usar en algún momento otros desarrolladores. Pero, cuando trabajamos en un proyecto propio, también es una buena costumbre usar un estilo de código claro y optimizado. Nos ayudará a revisar mejor el código y a entenderlo si en algún momento tenemos que modificarlo o queremos reutilizarlo.

Una vez escuché a un programador decir: ‘Hoy sé lo que hace este código, mañana sólo Dios lo sabe’. Esta propuesta es para que mañana tú también sepas con facilidad lo que hace tu código.

En esta serie vamos a repasar los estándares de programación hasta llegar a las recomendaciones para Laravel.

Empezamos con los estándares de programación en PHP propuestos por el [php-fig (Grupo de interoperatibilidad para Frameworks PHP)](http://www.php-fig.org).

###PSR-1 - Estándar básico de estilos de código.

Orientado al contenido de los ficheros PHP y a los nombres de las clases y métodos. 

Su objetivo es garantizar un alto nivel técnico de interoperabilidad entre el código PHP.

 - **DEBEN** usarse únicamente las etiquetas `<?php` y `<?=`.

 - **DEBE** usarse sólo `UTF-8 without BOM` para código PHP.  

 - Un fichero debería contener o bien estructuras y símbolos (clases, funciones, constantes, etc...) o bien partes de la lógica secundaria (informes, configuración, etc..) pero **NO DEBERÍAN** hacerse las dos cosas. 

 - Los Namespaces y las clases **DEBEN** cumplir el estándar PSR-4.

 - Los nombres de las clases **DEBEN** utilizar la notación `StudlyCaps`.
	```
	class MySqlBuilder extends Builder {...}
	class CreateUsersTable extends Migration {...}
```
 - Las constantes de las clases **DEBEN** declararse en MAYÚSCULAS usando guiones bajos como separadores.
	```
    const VERSION = '1.0';
	const DATE_APPROVED = '2012-06-01';
    ```

 - Los nombres de los métodos **DEBEN** declararse en notación `camelCase`. (Ej: `getColumnListing()`, `compileTableExists()`)
	```
	public function hasTable($table){...}
	public function getAuthPassword(){...}
    ```

 - Para los nombres de las propiedades no se define una recomendación concreta. A excepción de que la convención que se elija se mantenga para todo el proyecto, clase o método. 

<a name="StudlyCaps"></a>
>`StudlyCaps` es una notación en la que se alternan mayúsculas y minúsculas por algún patrón concreto.  

>En `camelCase` se escribe la primera letra de cada palabra con mayúsculas (Habitualmente la primera letra de todas suele ir en minúsculas).  

>En `snake_case` se separan las palabras sustituyendo los espacios por guiones bajos.   
En en este estándar `StudlyCaps` y `camelCase` se diferencian únicamente en el uso de la primera letra en minúsculas para `camelCase`.
	
###PSR-4 - Estándar de Autocarga.

Orientado a los nombres de los Namespaces, Clases y Ficheros. Su objetivo es facilitar la carga automática de clases.

 - Las rutas completas de los Namespaces **DEBEN** tener la estructura `Proveedor\Namespace\NombreDeLaClase`. `(VendorName\Namespace\ClassName)`
 
 Por ejemplo:  
	```
    use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	```
 Proveedor: `Illuminate`  
 Namespace: `Database\Migrations`  
 Nombre de la Clase: `Blueprint y Migration`  
 
 - Todas las rutas **DEBEN** tener un Proveedor (Vendor Name).
 
 - Los Namespaces pueden estar compuestos de todos los Sub-Namespaces que se desee.  
 
 - Todas las rutas **DEBEN** acabar en un Nombre de Clase (Class Name).
 
 - Los guiones bajos no tienen ningún significado especial (modificación sobre PSR-0).
 
 - Se pueden combinar mayúsculas y minúsculas.
 
 - Todos los nombres de clase **DEBEN** usar un estilo 'case-sensitive'.
   
Cada namespace puede tener tantos sub-namespaces como se quiera.

Todos los archivos deben tener la extensión .php.

Los nombres de los namespaces o clases deben ser ordenados alfabéticamente.

###Fuentes y más información:
[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload)

[Estándares de programación en PHP (PSR-2)](Est%C3%A1ndares-de-programaci%C3%B3n-PSR-2)

<a href="https://styde.net/laravel-5/">Curso de Laravel 5 en español desde cero</a>:  <a href="https://styde.net/curso-de-laravel-5-que-es-psr-4-y-uso-de-los-namespaces/">Introducción: PSR-4 y Namespaces</a>.   


[php-fig - Grupo de interoperatibilidad para Frameworks PHP](http://www.php-fig.org)  

[Estándar PSR-1](http://www.php-fig.org/psr/psr-1/)  

[Estándar PSR-2](http://www.php-fig.org/psr/psr-2/)  

[Estándar PSR-3](http://www.php-fig.org/psr/psr-3/)  

[Estándar PSR-4](http://www.php-fig.org/psr/psr-4/)  

[Ejemplos de implementación de PSR-4](http://www.php-fig.org/psr/psr-4/examples/)  
