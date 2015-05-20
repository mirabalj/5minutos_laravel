###Composer. Resumen sobre instalación y actualización de paquetes

- Composer usa **Git** (o Subversion, Mercurial, etc..) para descargar los paquetes.
- Su repositorio central por defecto es [Packagist](packagist.org).

####Instalación de composer

```
curl -sS https://getcomposer.org/installer | php
```

O, si no tienes instalado `curl` o estás usando Windows:

```
php -r "readfile('https://getcomposer.org/installer');" | php
```

Para instalar composer manualmente en Windows: https://gist.github.com/jatubio/d5c30606328c370d5640

Para ejecutarlo: `php composer.phar [opciones] [comandos]`.

###Crear un proyecto con Composer

```
composer create-project <paquete> <nombre del proyecto> [<version>]
```
 
Ejemplos:
 
```
composer create-project laravel/laravel nombre_del_proyecto
```	
 
```
composer create-project laravel/laravel nombre_del_proyecto 5.0.28
```	
 
```
composer create-project laravel/laravel nombre_del_proyecto --stability=beta
```	

**Opciones:**
 
- `--stability` (Por defecto:stable)

 Define el valor para el campo "minimum-stability" del fichero `composer.json`.
	
 Las opciones posibles son (En orden de estabilidad): `dev, alpha, beta, RC, y stable`.
 
- `--repository-url`

 Te permite especificar un repositorio en lugar de **Packagist**. Puede ser la `url http` de un repositorio compatible con Composer (es decir, que tenga un fichero composer.json). O una ruta o path a un fichero local con estructura de `composer.json`.

###Instalar paquetes

```
composer install
``` 

- Lee el fichero `composer.lock` o, en su defecto, `composer.json`
- Crea o actualiza `composer.lock` para dejar 'una foto fija' del entorno de ejecución de la aplicación.
- Ejecuta un `dump-autoload`.

> **Nota:** Después de clonar un repositorio, ejecuta `composer install` para crear el directorio vendor.

**Opciones:**

- `--stability` (Por defecto:stable)

- `--no-dev`

 Excluye los paquetes incluidos en la sección `require-dev`.
 Cuando se generan los ficheros autoload, no se ejecutan las reglas de `autoload-dev`.

- `--prefer-dist`

 Instala los paquetes desde el repositorio de distribución. (Opción por defecto).

- `--prefer-source`

 Instala los paquetes desde el repositorio de origen.

- `--no-scripts`

 No ejecuta los scripts definidos en el fichero `composer.json`.

- `--dry-run`

 Simula el proceso de instalación o actualización pero no modifica ningún fichero.
 
- `--optimize-autoloader` o `-o`

 Equivalente al parámetro `--optimize` del comando `composer dump-autoload`.
 
 Convierte las clases referenciadas en `PSR-0` y `PSR-4` a clases `classmap` para obtener un autoloader más rápido.
 
###Actualizar paquetes

``` 
composer update [<proveedor/paquete1> <proveedor/paquete2> <proveedor/*>]
```

- Utiliza siempre el fichero `composer.json` de la aplicación.

Ejemplos:

 ```
composer update
```

 ``` 
composer update doctrine/dbal laravel/framework
```
 
 ```
composer update doctrine/*
```

**Opciones:**
 
- `--prefer-lowest`
  
  Preferencia a las versiones mínimas de las dependencias. Se usa con `--prefer-stable` y es útil para testar la versionés mínimas de las dependencias.

- `--prefer-stable`

 Preferencia a las versiones 'estables' de los paquetes.

**Opciones comunes con `composer install`**:

- `--dev`

- `--no-dev`

- `--prefer-dist`

- `--prefer-source`

- `--no-scripts`

- `--dry-run`

- `--optimize-autoloader` o `-o`


###Añadir e instalar nuevos paquetes en nuestra aplicación

```
composer require <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]
```

Ejemplo:

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

> **Truco:** Usa `composer update --lock` para instalar los paquetes que hayas añadido en el fichero `composer.json`.

**Opciones comunes con `composer install`**:

- `--prefer-dist`

- `--prefer-source`

###Desinstalar paquetes

```
composer remove <proveedor/paquete1[:version]> [<proveedor/paquete2[:version]>]
```

Admite los mismos parámetros que `composer require`.

###Otros comandos de Composer

- `composer dump-autoload`

 Actualiza los ficheros de autocarga de tu aplicación.  
 
  > **Nota:** `dump-autoload` **no descarga nada**. Simplemente vuelve a generar el listado de todas las clases que necesitan ser incluidas en tu proyecto.

  **Parámetros:**

  - `--optimize` o `-o`: Convierte las clases referenciadas en `PSR-0` y `PSR-4` a clases `classmap` para obtener un autoloader más rápido. 
   > Usando `--optimize` en tu aplicación en el entorno de producción, puedes mejorar su rendimiento entre un **20% y un 25%**.

  - `--no-dev`: No tiene en cuenta las clases referenciadas en la clave: `autoload-dev`.
  
- `composer search <palabra>`

 Busca en packagist y en la caché local y te muestra todos los paquetes que contengan `<palabra>`.
 
- `composer show --installed`

 Muestra los paquetes instalados.  
 
 Para ver todas las versiones disponibles de un paquete y sus dependencias usa el comando `composer show -v <paquete>`. Por ejemplo:
 
 	`composer show -v laravel/framework`

- `composer validate`
 
 Comprueba que la sintaxis de tu fichero `composer.json` es correcta.
 
**Opciones generales:**

- `-vvv`

 Ver con más detalle el proceso de instalación de los paquetes.

###Configurar opciones de instalación a nivel global.

`composer global` o en el fichero `COMPOSER_HOME/config.json`

- `preferred-install`

 Ejemplo:

 `composer config --global preferred-install dist`


- `cache-dir`

 `composer config --global cache-dir <ruta_de_la_cache>` 

 > También puedes configurar ese directorio con la variable de entorno `COMPOSER_CACHE_DIR`.

- Dependencias globales.

	`composer global require "laravel/framework=~1.1"`

Tienes más información en el artículo que he publicado en Styde.net: [Instalar y actualizar paquetes con Composer](https://styde.net/instalar-y-actualizar-paquetes-con-composer/)
	
###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Instalar Composer manualmente en Windows](https://gist.github.com/jatubio/d5c30606328c370d5640)   

[Diferencias entre composer install y composer update.](Composer.-Resumen---Diferencias-entre-composer-install-y-composer-update)

[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload))

[Packagist](packagist.org)

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)
