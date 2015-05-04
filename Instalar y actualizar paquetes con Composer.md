##Instalar y actualizar paquetes con Composer

Composer es un gestor de paquetes y dependencias. Esto quiere decir que se encarga automáticamente de gestionar las dependencias de tus proyectos e instalar los paquetes necesarios.

Composer usa un sistema de versiones como **Git** (o Subversion, Mercurial, etc..) para descargar los paquetes. Y su repositorio central por defecto es [Packagist](packagist.org). Con lo que cualquier paquete que encuentres en esa web, puedes instalarlo usando Composer.

###Instalar composer

Para instalar Composer, ejecuta el siguiente comando:

```
curl -sS https://getcomposer.org/installer | php
```

O, si no tienes instalado `curl` o estás usando Windows:

```
php -r "readfile('https://getcomposer.org/installer');" | php
```

El instalador comprobará si tienes configurados correctamente algunos parámetros del fichero `php.ini` y te avisará si no son correctos o descargará la última versión del fichero `composer.phar`.

> En este [Gist](https://gist.github.com/jatubio/d5c30606328c370d5640) tienes un instalador para instalarlo en Windows a modo 'portable' en una carpeta y usarlo desde ahí a través de un fichero `.bat`.
>
> Si quieres instalarlo al estilo 'Windows', puedes descargarte un instalador en la [Página de Composer](https://getcomposer.org/download/) que instalará el programa y configurará el PATH para que puedas usarlo globalmente.

Composer es un fichero `.phar`, por lo que para usarlo hay que ejecutar: `php composer.phar [opciones] [comandos]`.

En este post y en los siguientes utilizaremos la notación abreviada `composer`. 

> En Linux y Mac tienes la opción de configurar el sistema para poder utilizar únicamente `composer` en lugar de `php composer.phar`.  
>
> En Windows puedes crear un fichero `.bat` que haga lo mismo con el siguiente comando: 
>```
echo @php "%~dp0composer.phar" %*>composer.bat
```

###Crear un proyecto con Composer

Para crear un nuevo proyecto utilizando un paquete existente, utiliza el siguiente comando:

```
composer create-project <paquete> <nombre del proyecto> [<version>]
```

Estos son los pasos que lleva a cabo el comando `composer create-project`:

  1. Crea el directorio `<nombre del proyecto>` si no existe.

  2. Usa Git para hacer un clone y checkout del repositorio desde packagist o desde el repositorio central que tengas configurado. Descargando en ese directorio el paquete de nombre `<paquete>`.

  3. Lee el fichero `composer.json` del `<paquete>` y descarga e instala todas sus dependencias dentro de la carpeta `/vendor`. 
 
> Si no especificas una versión, se instalará la última versión que se adapte a tu configuración de estabilidad.
 
Por ejemplo, el comando con el que comienzan la mayoría de aplicaciones basadas en Laravel es:
 
```
composer create-project laravel/laravel nombre_del_proyecto
```	
 
También podrías crear tu aplicación desde la versión 5.0.28:
 
```
composer create-project laravel/laravel nombre_del_proyecto 5.0.28
```	
 
O, por ejemplo, crearla a partir de la última versión beta:
 
```
composer create-project laravel/laravel nombre_del_proyecto --stability=beta
```	

**Opciones:**
 
- `--stability` (Por defecto:stable)

 Define el mínimo nivel de estabilidad para los paquetes instalados.
 
 Tiene preferencia sobre el valor de "minimum-stability" en el fichero `composer.json`.
	
 Las opciones posibles son (En orden ascendente de estabilidad): `dev, alpha, beta, RC, y stable`.
 
- `--repository-url`

 Te permite especificar un repositorio en lugar de **Packagist**. Puede ser la `url http` de un repositorio compatible con Composer (es decir, que tenga un fichero composer.json). O una ruta o path a un fichero local con estructura de `composer.json`.

###Instalar paquetes

Para descargar e instalar los paquetes configurados en el fichero `composer.json` de la aplicación, usa el comando:

```
composer install
``` 

Este comando, lleva a cabo los siguientes pasos:

1. Lee el fichero `composer.lock` o, si no existe, el fichero `composer.json`. 

2. Busca en **Packagist** los paquetes especificados en ese fichero.

3. Resuelve la versión a instalar de cada paquete a partir de las versiones indicadas y la configuración de estabilidad.

4. Resuelve todas las dependencias para esas versiones.

5. Instala todos los paquetes y todas las dependencias.

6. Una vez instalados los paquetes, si no existe `composer.lock`, lo crea para dejar 'una foto fija' del entorno de ejecución de la aplicación.

 También crea los ficheros de autocarga de clases de la aplicación.

> El directorio `vendor` no suele distribuirse con las aplicaciones. Por lo tanto, cuando haces un `clone` (o un `fork` y un `clone`) de un proyecto o aplicación, el primer paso suele ser ejecutar `composer install` para generar ese directorio e instalar en él todas las dependencias de la aplicación.

**Opciones:**

- `--stability` (Por defecto:stable)

 Define el mínimo nivel de estabilidad para los paquetes instalados.
 
 Tiene preferencia sobre el valor de "minimum-stability" en el fichero `composer.json`.
	
 Las opciones posibles son (En orden ascendente de estabilidad): `dev, alpha, beta, RC, y stable`.
 
- `--dev`

 Instala también los paquetes incluidos en la sección `require-dev`.
 
 > En las últimas versiones de Composer no es necesario especificarlo porque es el valor por defecto.

- `--no-dev`

 Lo contrario de la opción anterior. Excluye los paquetes incluidos en la sección `require-dev`.
 
 Cuando se generan los ficheros autoload, no se ejecutan las reglas de `autoload-dev`.

- `--prefer-dist`

 Instala los paquetes desde el repositorio de distribución. (Opción por defecto).

- `--prefer-source`

 Si se ha definido un repositorio origen y está disponible, se instalan los paquetes desde ese repositorio.

- `--no-scripts`

 No ejecuta los scripts definidos en el fichero `composer.json`.

- `--dry-run`

 Simula el proceso de instalación o actualización pero no modifica ningún fichero.
 
- `--optimize-autoloader` o `-o`

 Equivalente al parámetro `--optimize` del comando `composer dump-autoload`.
 
 Convierte las clases referenciadas en `PSR-0` y `PSR-4` a clases `classmap` para obtener un autoloader más rápido.
 
###Actualizar paquetes

Si ya tienes instalada tu aplicación y lo que quieres es actualizar los paquetes instalados, usa el siguiente comando:

``` 
composer update [<proveedor/paquete1> <proveedor/paquete2> <proveedor/*>]
```

Este comando, lleva a cabo los siguientes pasos:

1. Lee **SIEMPRE** el fichero `composer.json`. 

2. Busca en **Packagist** los paquetes especificados en ese fichero.

3. Resuelve la versión a instalar de cada paquete a partir de las versiones indicadas y la configuración de estabilidad.

4. Resuelve todas las dependencias para esas versiones.

5. Para aquellos paquetes que exista una nueva versión disponible, la descarga y la instala sustituyendo a la versión actual. 

6. Una vez instalados los paquetes, si no existe `composer.lock`, lo crea para dejar 'una foto fija' del entorno de ejecución de la aplicación. En caso de que exista, lo actualiza.

 También crea los ficheros de autocarga de clases de la aplicación.

> Aunque existan nuevas versiones de un paquete, es posible que no se instalen debido a las restricciones de versión y estabilidad configuradas.

- Puedes actualizar todos los paquetes:

 ```
composer update
```

- Actualizar únicamente uno o varios paquetes separándolos por espacios:

 ``` 
composer update doctrine/dbal laravel/framework
```
 
- O actualizar todos los paquetes de un proveedor ('vendor') usando un asterisco:
 
 ```
composer update doctrine/*
```

**Opciones:**
 
- `--prefer-lowest`
  
  Preferencia a las versiones mínimas de las dependencias.  
  Se usa con `--prefer-stable` y es útil para testar la versiones mínimas de las dependencias.

- `--prefer-stable`

 Preferencia a las versiones 'estables' de los paquetes.

> Aunque `composer install` y `composer update` pueden parecer similares, en realidad se usan para situaciones muy distintas. Tienes más información sobre la diferencia entre `composer install` y `composer update` en: [Diferencias entre composer install y composer update](Diferencias-entre-composer-install-y-composer-update).

**Opciones comunes con `composer install`:**

- `--dev`

- `--no-dev`

- `--prefer-dist`

- `--prefer-source`

- `--no-scripts`

- `--dry-run`

- `--optimize-autoloader` o `-o`


###Añadir e instalar nuevos paquetes en nuestra aplicación

Cuando tienes una aplicación instalada y quieres añadir nuevos paquetes, tienes dos opciones.

Editando el fichero `composer.json`:

1. Edita el fichero `composer.json`.

2. Añade los nuevos paquetes.

3. Ejecutas `composer update` para instalar los nuevos paquetes y **actualizar** los paquetes que ya tenías instalados.

O bien, usando el siguiente comando:

```
composer require <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]
```

Este comando añade los paquetes especificados al fichero `composer.json` y los instala.

 Por ejemplo, para instalar el paquete `Doctrine DBAL` a partir de la versión 2.3:  


```
composer require doctrine/dbal:~2.3
```
 
**Opciones:**

- `--dev`

 Añade los paquetes a la sección `require-dev`.
  
- `--no-update`  

 No actualiza las dependencias de los paquetes.
   
- `--update-no-dev`

 Ejecuta la actualización con la opción `--no-dev`. 
  
- `--update-with-dependencies`

 Actualiza también las dependencias de los nuevos paquetes requeridos.

> **Truco:** Si editas el fichero `composer.json`, pero quieres instalar únicamente los nuevos paquetes que has añadido, `composer install` **no hará nada** porque únicamente lee desde el fichero `composer.lock`.  
>
> Si no quieres actualizar los paquetes que ya tenías instalados, en lugar de utilizar `composer update`, ejecuta el siguiente comando:

>```
>composer update --lock
>```
>Ese comando en teoría, **sólo** actualiza el Hash MD5 del fichero `composer.lock`.   
>
>Pero cuando compruebe que hay paquetes nuevos, los instalará automáticamente sin actualizar los existentes.

**Opciones comunes con `composer install`:**

- `--prefer-dist`

- `--prefer-source`

###Desinstalar paquetes

```
composer remove <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]
```

 Borra los paquetes especificados del fichero `composer.json` y los desinstala.

**Opciones:**
 
 Admite los mismos parámetros que `composer require`.

###Otros comandos de Composer

- `composer dump-autoload`

 Actualiza los ficheros de autocarga de tu aplicación.  
 
 Cuando creas nuevas clases en tus aplicaciones, si están en rutas que no estén referenciadas por el estándar `PSR-4` en el fichero `composer.json`, tienes que añadirlas manualmente a ese fichero y ejecutar `composer dump-autoload` para que se incluyan en el sistema de autocarga de clases de la aplicación.
 
  > **Nota:** `dump-autoload` **no descarga nada**. Simplemente vuelve a generar el listado de todas las clases que necesitan ser incluidas en tu proyecto.

  **Parámetros:**

  - `--optimize` o `-o`: Convierte las clases referenciadas en `PSR-0` y `PSR-4` a clases `classmap` para obtener un autoloader más rápido. Está recomendado especialmente para entornos de producción. Según el número de clases y tamaño de tu proyecto, puede tardar un poco más en ejecutarse y por eso no está activado por defecto.

     > Usando `--optimize` en tu aplicación en el entorno de producción, puedes mejorar su rendimiento entre un **20% y un 25%**.

  - `--no-dev`: No tiene en cuenta las clases referenciadas en la clave: `autoload-dev`.
  
- `composer search <palabra>`

 Busca en packagist y en la caché local y te muestra todos los paquetes que contengan `<palabra>`.

 Por ejemplo, para buscar todos los paquetes relacionados con laravel, ejecuta:

```
composer search laravel
```
 
- `composer show [--installed]`

 Muestra información sobre los paquetes disponibles en **Packagist**.

 Para ver todas las versiones disponibles de un paquete y sus dependencias usa el comando `composer show <paquete>`. Por ejemplo:
 
```
composer show laravel/framework
```

 Utiliza `--installed` para obtener información sobre un paquete instalado.  
 
```
composer show laravel/framework --installed
```

 Y, para mostrar todos los paquetes instalados y sus versiones:
 
```
composer show --installed
```

- `composer init`
 
 Crea un fichero `composer.json` inicial en el directorio actual.
 
- `composer validate`
 
 Comprueba que la sintaxis de tu fichero `composer.json` es correcta.
 
- `composer --help` o `composer -h`

 Te muestra la ayuda de Composer y todas las opciones disponibles.

**Opciones generales:**

- `--verbose` o `-v`

 En cualquier comando puedes utilizarlo para aumentar el nivel de detalle de los mensajes que muestra el comando. El máximo nivel es `-vvv`.
 
 > Si composer se te queda 'colgado' o te da errores, puedes usar esta opción para ver con más detalle el proceso de instalación de los paquetes y ver dónde falla.

###Configurar opciones de instalación a nivel global.

Puedes configurar opciones de Composer a nivel global con el comando `composer global` o modificando el fichero `config.json` que está en el directorio 'HOME' de Composer. Puedes cambiar ese directorio con la variable de entorno `COMPOSER_HOME`.
	
> Habitualmente, el directorio 'HOME' de Composer es:   
> 
> En Windows: `C:\Users\<tu_usuario>\AppData\Roaming\Composer`.  
> En Linux: `/home/<tu_usuario>/.composer`.  
> En Mac: `/Users/<tu_usuario>/.composer`.  

- `preferred-install`

 Por ejemplo, para configurar de forma global los repositorios orígenes como preferentes, ejecuta:

 `composer config --global preferred-install source`

 Ese comando, modificará el fichero global `config.json` y añadirá las siguientes líneas:

 ```
	"config": {
		"preferred-install": "source"
	},
```

- `cache-dir`

 Y para configurar el directorio que Composer usará como cache para los paquetes descargados, puedes usar:
 
```
composer config --global cache-dir <ruta_de_la_cache>
``` 

 > También puedes configurar ese directorio con la variable de entorno `COMPOSER_CACHE_DIR`.

- Dependencias globales.

 Para añadir de forma global una dependencia y que de ese modo siempre se instalen y actualicen esos paquetes en la caché global, ejecuta:

	`composer global require "laravel/framework=~1.1"`

###El fichero `composer.json`

Es el fichero en el que se configuran las dependencias de tu aplicación.

Contiene además otros parámetros como el nombre de tu aplicación, la descripción, licencia, etc...

Composer lo crea con `composer init` y `composer create-project` y lo actualiza cuando utilizamos `composer update`, `composer require` o `composer remove`. También lo puedes modificar manualmente.

###El fichero `composer.lock`
	
Contiene una lista de todos los paquetes instalados en tu aplicación y la versión de cada uno.

Se crea con el comando `composer install` y se actualiza siempre que ejecutamos `composer update`.

Si existe, `composer install` usará la información de este fichero en lugar de usar el fichero `composer.json`.

> Tienes más información sobre ambos ficheros en: [Diferencias entre composer install y composer update](Diferencias-entre-composer-install-y-composer-update) y [Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload)).

###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Instalar Composer como Portable en Windows](https://gist.github.com/jatubio/d5c30606328c370d5640)   

[Diferencias entre composer install y composer update.](Diferencias-entre-composer-install-y-composer-update)

[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload))

[Packagist](packagist.org)

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)
