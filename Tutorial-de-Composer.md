##Tutorial de Composer

Composer es un gestor de paquetes y dependencias. Esto quiere decir que se encarga automáticamente de gestionar las dependencias de tus proyectos e instalar los paquetes necesarios.

Composer usa **Git** (o Subversion, Mercurial, etc..) para descargar los paquetes y su repositorio central por defecto es [Packagist](packagist.org). Con lo que cualquier paquete que encuentres en esa web, puedes instalarlo usando Composer.

<a name="instalar-composer"></a>
###Instalar composer.

Puedes instalarlo desde su página web: https://getcomposer.org/download/

Si quieres instalarlo en windows 'al estilo Unix', puedes descargarte mi [Gist](https://gist.github.com/jatubio/d5c30606328c370d5640) para instalarlo en una carpeta y usarlo desde ahí.


###Distribuciones.

Puedes instalar un paquete de dos formas distintas: `source` y `dist`.

- `dist` (Distribución)

 Este es el modo por defecto. En este modo, los paquetes se instalarán desde su repositorio de distribución.
 
 Si los paquetes ya se descargaron anteriormente, los tomará de la cache local.
 
 Esta versión descarga el paquete en formato zip si está disponible.
 
  > Usa esta opción para **acelerar** el proceso de instalación y/o si tienes problemas con la configuración de Git.

- `source` (Fuente)

 Este modo es el contrario a `dist`. Cuando usas `source`, si el repositorio indicado en la configuración como `source` está disponible, Composer lo usará para instalar los paquetes. 

 Este sistema es útil si estás modificando ficheros fuente del paquete, ya que puedes indicar incluso un repositorio local.
 
 Se usa por ejemplo, cuando quieres contribuir con un 'patch' a un proyecto open source.
 
 > **Nota:** Esta opción **requiere tener configurado Git** o un sistema compatible con el tipo de repositorio en el que estén los paquetes.  

 <br>
Extracto del fichero `composer.json` de laravel/framework:

 ```
    "support": {
        "issues": "https://github.com/laravel/framework/issues",
        "source": "https://github.com/laravel/framework"
    },
```

 > Puedes modificar la opción por defecto en el fichero de configuración global `config.json` o para un proyecto concreto en el fichero `composer.json`:
>
> `"preferred-install": "source"`

###Estabilidad.

Cuando instalas los paquetes, puedes elegir qué nivel de estabilidad quieres como mínimo para las versiones instaladas de cada paquete. Composer instalará la última versión disponible que cumpla con el mínimo indicado de estabilidad.

También puedes configurar el nivel de estabilidad por defecto con el parámetro `minimum-stability` del fichero `composer.json`.

Por ejemplo, puedes configurar el mínimo nivel de estabilidad ('desarrollo') en el fichero `composer.json` de proyectos que sólo vas a ejecutar en tu entorno local. Por ejemplo, para paquetes de desarrollo.

`"minimum-stability": "dev"`

Y como mínimo 'Release Candidate' en proyectos que vas a pasar a tu entorno de producción:

`"minimum-stability": "RC"`

Las opciones posibles son (En orden ascendente de estabilidad): `dev, alpha, beta, RC, y stable`.

El valor por defecto es `stable`.

> **Nota:** Si composer no encuentra una versión que cumpla con el valor configurado por defecto en `minimum-stability`, **no instalará el paquete**.

Antes de que Composer incluyera la opción `prefer-stable`, la solución 'más sencilla' para no tener problemas al instalar paquetes era configurar `minimum-stability` al nivel más bajo posible, es decir, a `dev`.

Sin embargo, esa configuración tiene dos inconvenientes:

 - Estás incluyendo código que 'acaba de ser programado'. Incluso es posible que estés incluyendo el último 'commit'. Por tanto, estás poniendo en riesgo la estabilidad de tu aplicación.
 
 - Para cada nivel de estabilidad, Composer recorre todas las versiones posibles de ese paquete. Por lo que la velocidad de instalación y actualización de paquetes se reduce enormemente.
 
Sin embargo, a veces necesitamos un paquete para el que no existen recientes versiones marcadas como 'estables', o, peor aún, las únicas versiones recientes que existen son versiones `dev` que no se instalarán si nuestro `minimum-stability` está configurado por encima de `dev`.

La solución fue la nueva opción `"prefer-stable": true`.

Con esta opción, le decimos a Composer que **preferimos** los paquetes estables, pero que si no los hay, **puede instalar** aquellos que cumplan con `minimum-stability`. De ese modo aumentamos el porcentaje de paquetes que se instalarán en su versión `stable` y sólo se instalará una versión de menor estabilidad cuando sea un requisito de alguna dependencia. 

Por eso, ambas opciones se suelen usar en combinación.

Por ejemplo, con el siguiente fichero `composer.json`, se buscará para todos los paquetes la versión `stable`. Pero si para alguna dependencia no existe esa versión, se buscará la versión más estable posible `RC` o `beta`.

```
{
	"name": "laravel/laravel",
	"description": "The Laravel Framework.",
	"keywords": ["framework", "laravel"],
	"license": "MIT",
	"type": "project",
	"require": {
		"laravel/framework": "5.0.*"
	},
	"require-dev": {
		"phpunit/phpunit": "~4.0",
		"phpspec/phpspec": "~2.1",
		"fzaninotto/faker": "1.4.*",
		"barryvdh/laravel-ide-helper": "~2.0"
	},
	"config": {
		"preferred-install": "dist"
	}
	"minimum-stability": "beta",
	"prefer-stable": true
}
```

Si necesitas la versión `dev` de uno o varios paquetes, configura esa estabilidad únicamente para esos paquetes:

```
{
	"name": "laravel/laravel",
	"description": "The Laravel Framework.",
	"keywords": ["framework", "laravel"],
	"license": "MIT",
	"type": "project",
	"require": {
		"laravel/framework": "5.0.*"
	},
	"require-dev": {
		"phpunit/phpunit": "~4.0",
		"phpspec/phpspec": "~2.1",
		"fzaninotto/faker": "1.4.*@dev",
		"barryvdh/laravel-ide-helper": "~2.0@dev"
	},
	"config": {
		"preferred-install": "dist"
	}
	"minimum-stability": "beta",
	"prefer-stable": true
}
```

Por ejemplo, en el fichero anterior, para usar las versiones de desarrollo de los paquetes `fzaninotto/faker` y `barryvdh/laravel-ide-helper`, hemos añadido `@dev` a sus requerimientos en lugar de modificar la opción global `"minimum-stability": "beta"` a `"minimum-stability": "dev"`.

> **Nota:** Si usas `"minimum-stability": "dev"`, añade siempre `"prefer-stable": true` a tu fichero `composer.json`.
>
> **Nota:** Si modificas el parámetro `minimum-stability` de un fichero `composer.json`, cambiarás los requerimientos de los paquetes instalados. Es posible que tengas que borrar el directorio `vendor` y el fichero `composer.json` y ejecutar `composer install` para que tu aplicación funcione de nuevo.

####Versiones de los paquetes:

Composer usa un sistema de comodines para calcular la versión de un paquete que instalará en tu aplicación. Ese sistema se aplica a los paquetes configurados en el fichero `composer.json` y también se puede utilizar en la línea de comandos cuando especificamos un paquete.

Por ejemplo:

`"laravel/framework": "5.0.12"`

Instala específicamente la versión `5.0.12` del framework de Laravel.

`"laravel/framework": "4.2.*"`

Instala cualquier versión mayor o igual a `4.2` y menor de `4.3`.

`"laravel/framework": "~5.0"`

Instala cualquier versión mayor o igual a `5.0` y menor de `6.0.0`.

`"laravel/framework": "~5.1.2"`

Instala cualquier versión mayor o igual a `5.1.2` y menor de `5.2.0`.

> `laravel/laravel` y `laravel/framework` son dos paquetes distintos. El primero es el 'esqueleto' de una aplicación Laravel para que lo uses como base de tu aplicación (contiene las carpetas `app`, `config`, `public`, `database`, etc..). El segundo es una dependencia del paquete `laravel/laravel` y contiene el framework, es decir, todo el núcleo y funciones internas de Laravel. Se instala en el directorio `vendor`. 

Una vez calculadas las posibles versiones, se comprobará tu configuración de estabilidad y de las posibles opciones, se instalará la versión que cumpla con esas condiciones.

El sistema de comodines incluye más combinaciones. Tienes toda la información completa en la documentación oficial de Composer:  https://getcomposer.org/doc/01-basic-usage.md#package-versions

###Comandos

<a name="create-project"></a>
- `composer create-project <paquete> <nombre del proyecto> [<version>]`

 Crea un proyecto nuevo descargando y replicando el `<paquete>` especificado:

  1. Crea la carpeta `<nombre del proyecto>` si no existe.

  2. Usa Git para hacer un clone y checkout del repositorio desde packagist o desde el repositorio central que tengas configurado. Descargando en esa carpeta el `<paquete>`.

  3. Lee el fichero `composer.json` del `<paquete>` y descarga e instala todas sus dependencias dentro de la carpeta `/vendor`. 
 
 Si no especificas una versión, se instalará la última versión que se adapte a tu configuración de estabilidad.
 
 Por ejemplo, el comando con el que comienzan la mayoría de aplicaciones basadas en Laravel es:
 
 `composer create-project laravel/laravel nombre_del_proyecto`	
 
 También podrías crear tu aplicación desde la versión 5.0.28:
 
 `composer create-project laravel/laravel nombre_del_proyecto 5.0.28`	
 
 O crearla a partir de la última versión beta:
 
 `composer create-project laravel/laravel nombre_del_proyecto --stability=beta`	

  **Parámetros:**
 
  - `--stability` (Por defecto:stable)

  Define el valor para el campo "minimum-stability" del fichero `composer.json`.
	
 Las opciones posibles son (En orden de estabilidad): `dev, alpha, beta, RC, y stable`.
 
- `composer install`

 Descarga e instala los paquetes que hayamos configurado en el fichero `composer.json` de la aplicación.
 
 > **Nota:** El directorio `vendor` no suele distribuirse con las aplicaciones. Por tanto, cuando haces un `clone` de un proyecto o aplicación, el primer paso suele ser ejecutar `composer install` para generar ese directorio e instalar en él todas las dependencias de la aplicación.

- `composer update [<proveedor/paquete1> <proveedor/paquete2> <proveedor/*>]`

 Para los paquetes que tenemos instalados, comprueba su configuración en el fichero `composer.json` de la aplicación. Y aquellos en los que se ha configurado la opción de actualizar automáticamente la versión, los actualiza a la última versión disponible (según las restricciones de versión configuradas).

 - Puedes actualizar todos los paquetes:

   `composer update`

 - Actualizar únicamente uno o varios paquetes separándolos por espacios:

   `composer update doctrine/dbal laravel/framework`
 
 - O actualizar todos los paquetes de un proveedor ('vendor') usando un asterisco:
 
   `composer update doctrine/*`

  **Parámetros:**
 
  - `--prefer-lowest`
  
  Preferencia a las versiones mínimas de las dependencias. Se usa con `--prefer-stable` y es útil para testar la versionés mínimas de las dependencias.

  Define el valor para el campo "minimum-stability" del fichero `composer.json`.
	

 > Tienes más información sobre la diferencia entre `composer install` y `composer update` en [este post](Diferencias-entre-composer-install-y-composer-update).

- `composer require <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]`

 Añade los paquetes especificados al fichero `composer.json` y los instala o actualiza.

 Por ejemplo, para instalar el paquete `Doctrine DBAL` a partir de la versión 2.3:  

 `composer require doctrine/dbal:~2.3`
 
 Opciones:
  - `--dev`: Añade los paquetes a la sección `require-dev`.
  
  - `--no-update`: No actualiza las dependencias de los paquetes.
   
  - `--update-no-dev`: Ejecuta la actualización con la opción `--no-dev`. 
  
  - `--update-with-dependencies`: Actualiza también las dependencias de los nuevos paquetes requeridos.

- `composer remove <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]`

 Borra los paquetes especificados del fichero `composer.json` y los desinstala.
 
 Admite los mismos parámetros que `composer require`.

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

 Por ejemplo, para buscar todos los paquetes relacionados con laravel, ejecuta: `composer search laravel`
 
- `composer show --installed`

 Muestra los paquetes instalados.  
 
 Para ver todas las versiones disponibles de un paquete y sus dependencias usa el comando `composer show -v <paquete>`. Por ejemplo:
 
 	`composer show -v laravel/framework`

- `composer init`
 
 Crea un fichero `composer.json` inicial en el directorio actual.
 
- `composer validate`
 
 Comprueba que la sintaxis de tu fichero `composer.json` es correcta.
 
###Opciones aplicables a `composer install`, `composer update`, y `create-project`.

- `--stability` (Por defecto:stable)

 Define el valor para el campo "minimum-stability" del fichero `composer.json`.
	
 Las opciones posibles son (En orden de estabilidad): `dev, alpha, beta, RC, y stable`.

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
 
###Opciones globales

- `--verbose` o `-v`

 Aumenta el nivel de detalle de los mensajes que muestra el comando. El máximo nivel es `-vvv`.
 
 > Si composer se te queda 'colgado' o te da errores, puedes usar esta opción para ver con más detalle el proceso de instalación de los paquetes y ver dónde falla.
 
- `--help` o `-h`

 Te muestra la ayuda de Composer y todas las opciones disponibles.
 
- `--optimize-autoloader` o `-o`

 Equivalente al parámetro `--optimize` del comando `composer dump-autoload`.
 
####Configurar opciones de instalación a nivel global.

Puedes configurar opciones a nivel global con el comando `composer global` o modificando el fichero `config.json` que está en el directorio 'HOME' de Composer. Puedes cambiar ese directorio con la variable de entorno `COMPOSER_HOME`.
	
> Habitualmente, el directorio 'HOME' de Composer es:   
> 
> En Windows: `C:\Users\<tu_usuario>\AppData\Roaming\Composer`.  
> En Linux: `/home/<tu_usuario>/.composer`.  
> En Mac: `/Users/<tu_usuario>/.composer`.  

- `preferred-install`

 Por ejemplo, para configurar de forma global que los paquetes a instalar por defecto sean los de las distribuciones, ejecuta:

 `composer config --global preferred-install dist`

 Ese comando, modificará el fichero global `config.json` y añadirá las siguientes líneas:

 ```
	"config": {
		"preferred-install": "dist"
	},
```

- `cache-dir`

 Y para configurar el directorio que Composer usará como **cache** para los paquetes descargados, puedes usar: `composer config --global cache-dir <ruta_de_la_cache>` 

 > También puedes configurar ese directorio con la variable de entorno `COMPOSER_CACHE_DIR`.

- Dependencias globales.

 Para añadir de forma global una dependencia y que de ese modo siempre se instalen y actualicen esos paquetes en la caché global, ejecuta:

	`composer global require "laravel/framework=~1.1"`

###Fichero `composer.json`

Es el fichero en el que se configuran las dependencias de tu aplicación.

Contiene además otros parámetros como el nombre de tu aplicación, la descripción, licencia, etc...

Composer lo crea con `composer init` y `composer create-project` y lo modifica cuando utilizamos `composer require` o `composer remove`. También lo puedes modificar manualmente.

###Fichero `composer.lock`
	
Contiene una lista de todos los paquetes instalados en tu aplicación y la versión de cada uno.

Si existe, `composer update` usará la información de este fichero en lugar de usar el fichero `composer.json`.

Tienes más información de ambos ficheros en [Diferencias entre composer install y composer update](Diferencias-entre-composer-install-y-composer-update) y [Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload)).
	
###Resolviendo problemas generales de Composer

1. **Revisa la configuración del sistema.**

 El primer paso si tienes problemas, es ejecutar `composer diagnose`. De ese modo, Composer revisará automáticamente tu sistema y te avisará si encuentra algún problema.

2. **Actualiza tu versión de Composer.**

 El siguiente paso es asegurarte de que tienes la última versión con el comando `composer self-update`.
 
3. **Aumenta el nivel de detalle de los mensajes que muestra el comando.** 

 Añade la opción `-vvv` al comando que estés utilizando para ver todo el proceso con detalle.

4. **Borra la carpeta `vendor` y el fichero `composer.lock` y ejecuta `composer install`.** 
 
5. **Vacía la caché interna.**

 Si tienes problemas con Composer y no te funciona nada de lo anterior, usa el comando `composer clear-cache` para vaciar tu caché. De ese modo, se volverán a descargar las versiones originales de los paquetes la próxima vez que utilices `composer install` o `composer update`.

###Resolviendo errores concretos.

- **Composer es muy lento al actualizar/instalar los paquetes.**

 **Desactiva xDebug** en tu fichero `php.ini` mientras usas Composer comentando las líneas correspodientes. 

 > Con xDebug activado, el proceso de instalación y actualización de Composer puede durar **hasta 20 veces más!!**.
 
 > **Nota**: Para comprobar si tienes xDebug activado, puedes usar el siguiente comando en Windows: `php -m | findstr xdebug` o `php -m | grep xdebug` en Unix. Si lo tienes activado, aparecerá la palabra `xdebug`.
 
 Usar el mayor nivel de estabilidad posible aumentará ligeramente la velocidad de instalación/actualización de los paquetes.
 
 Cambiar las dependencias para que sean lo más específicas posibles, también ayuda. Por ejemplo, cambia `"laravel/framework": "5.*"` por `"laravel/framework": "5.0.*"` o mejor aún, por `"laravel/framework": "5.0.28"`.
 
 Si tienes configurado `"preferred-install": "source"`, utiliza `--prefer-dist` para descargar los paquetes en formato .zip y aumentar la velocidad.
 
 Por último, puedes instalar [Satis](https://github.com/composer/satis) para mantener una copia estática en local de los paquetes que utilizas habitualmente. Y [utilizar un script php](http://melp.nl/2013/09/composer-create-a-local-package-repository-to-improve-speed/) para configurarlo.

- **Error de Timeout.**

 Si tienes errores de Timeout porque tu conexión va muy lenta, configura el parámetro `process-timeout` para aumentar el tiempo por defecto que es de 300 segundos (5 minutos).
 
 Por ejemplo, para 15 minutos: 

 `composer config --global process-timeout 1500` 

 > También puedes configurar el Timeout con la variable de entorno `COMPOSER_PROCESS_TIMEOUT`.

- **Warning: Fichero lock desactualizado.**

 ```
 Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, run update to update them.
```

 Cuando Composer genera el fichero `composer.lock`, guarda en él un **Hash MD5** del fichero `composer.json`. Si posteriormente modificas el fichero `composer.json`, Composer te mostrará ese mensaje hasta que ejecutes `composer install` o `composer update`.
 
  > Si no quieres instalar ni actualizar tus paquetes. Por ejemplo, porque has añadido un comentario al fichero `composer.json` o un parámetro de configuración. El comando `composer update --lock` actualiza únicamente el hash del fichero `composer.lock` y suprime ese warning.

- **Warning: extensión openssl.**

 Si obtienes un Warning de tipo `openssl extension is missing` o `must enable the openssl extension` al instalar o usar Composer, asegúrate de tener configurada la extensión `php_openssl.dll` en tu fichero `php.ini`.
 
 Busca la línea: `;extension=php_openssl.dll` y quita el `;` inicial, o añade esta línea: `extension=php_openssl.dll`.

- **Incompatibilidad de versiones entre paquetes.**

 Si al instalar o actualizar tus paquetes tienes un problema de compatibilidad de versiones entre ellos, la solución es definir un [Alias](https://getcomposer.org/doc/articles/aliases.md).
 
- **'Your requirements could not be resolved to an installable set of packages'.**

 > **Nota:** Comprueba en primer lugar que los nombres de los paquetes están escritos correctamente.

 Este error indica que según tus requerimientos mínimos de estabilidad, hay una dependencia para la que no se ha encontrado ninguna versión que cumpla esos requerimientos.
 
 Empieza configurando `"minimum-stability": "dev"` en tu fichero `composer.json`. Y borrando (si existiera) `"prefer-stable": true`. Si no te da problemas, puedes probar con mayores niveles de estabilidad y cuando hayas encontrado el mínimo necesario, añade `"prefer-stable": true` para asegurarte de que ese nivel se aplique únicamente en los paquetes que lo requieran, o configura el nuevo nivel de estabilidad de forma individual para cada paquete que lo necesite (puedes usar `composer show --installed` para ver las versiones instaladas).
 
- **Error 503 al descargar los ficheros .zip de GitHub.**

 Prueba a usar `composer install --prefer-source` o  `composer update --prefer-source`.
 
###Importante

- Cuando distribuyas tu proyecto, recuerda añadir los ficheros `composer.json` y `composer.lock`.

- Generalmente usa `composer install` para instalar aplicaciones a no ser que tengas la certeza de que necesitas usar `composer update`.

- Usa `--no-dev` a no ser que necesites instalar las librerías de desarrollo.

- Usa `--optimize-autoloader` en el entorno de producción para mejorar el rendimiento de tu aplicación.

- No uses `"minimum-stability": "dev"` en tus ficheros.json. En su lugar, configura las dependencias `dev` para los paquetes que las necesiten. O, al menos, si lo usas, combínalo con `"prefer-stable": true`.

- Utiliza el parámetro `--dev` del comando `composer require` cuando instales paquetes que vas a usar únicamente en el entorno de desarrollo. Como los paquetes de tests, seeders, depuración, complementos del IDE, etc..

- Antes de subir tus cambios al repositorio, ejecuta el comando `composer validate` para asegurarte de que tu fichero `composer.json` es correcto.

- No distribuyas ni subas a tu repositorio la carpeta `vendor`. Añade siempre esa carpeta a tu fichero `.gitignore`.

###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Instalar Composer como Portable en Windows](https://gist.github.com/jatubio/d5c30606328c370d5640)   

[Diferencias entre composer install y composer update.](Diferencias-entre-composer-install-y-composer-update)

[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload))

[Packagist](packagist.org)

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)

[Sistema de comodines para calcular la versión a instalar](https://getcomposer.org/doc/01-basic-usage.md#package-versions)