##Solucionar conflictos de versiones y requerimientos en Composer (Alias)

Composer es una herramienta compleja y, en ocasiones, afinarlo para que funcione puede ser difícil. Un problema habitual es el **conflicto de versiones** cuando instalamos nuevos paquetes.

Probablemente, te hayas encontrado ya alguna vez con el mensaje de error `Your requirements could not be resolved to an installable set of packages.`.

Como necesitamos un ejemplo real, qué mejor que usar la flamante versión 5.1 de Laravel.

Vamos a aprovechar que Laravel 5.1 aún está en fase de desarrollo, y que los requerimientos de muchos paquetes de los que usamos habitualmente aún no están actualizados a esa versión. Y es muy habitual encontrarnos este error cuando instalamos paquetes en Laravel 5.1

Si además, estás creando aplicaciones con Laravel 5.1, y quieres usar tus paquetes habituales, en este post, te vamos a enseñar a instalarlos sin problemas.

Como comentaba antes, el principal problema con el que te vas a encontrar es con la incompatibilidad de versiones entre los requerimientos de los paquetes y los requerimientos del Framework de Laravel 5.1. Ya que muchos paquetes tienen definidos sus requerimientos a la versión `laravel/framework:5.0.x`, es decir, cualquier versión **menor** que la 5.1.

> **Nota:** Originalmente este post estaba escrito con el paquete `barryvdh/laravel-ide-helper`. Avisé a Barry vd. Heuvel, su autor, del problema y por si acaso lo corregía muy pronto, elegí el paquete `barryvdh/laravel-debugbar` para que pudiéras hacer las pruebas.
>
> Sin embargo, me ha contestado en GitHub para avisarme de que ya ha actualizado ambos paquetes para que acepten la versión 5.1 de `laravel/framework`. En **Packagist** aún no ha sido actualizado, pero es probable que cuando vayas a hacer las pruebas, ya no dé error al instalarlo y no puedas hacer las pruebas de este post con la última versión de ese paquete. En ese caso, si quieres seguir los pasos de este post, sustituye el comando:
>```
composer require barryvdh/laravel-debugbar --dev
```
> Por:
>```
composer require barryvdh/laravel-debugbar:2.0.3 --dev
```
> Para que instale una versión en la que el fichero `composer.json` es incompatible con Laravel 5.1


###Crear una aplicación con Laravel 5.1

Como ya vimos en el post anterior!!!, para crear una nueva aplicación y movernos al directorio de esta, ejecuta:

```
composer create-project laravel/laravel test51 dev-develop
cd test51
```

###Instalar un paquete con composer

Ahora, vamos a probar con uno de mis paquetes preferidos: `barryvdh/laravel-debugbar`.
```
composer require barryvdh/laravel-debugbar --dev
```

> Dado que es un paquete de desarrollo, he añadido `--dev` para que se instale únicamente en la sección de desarrollo.

###Error: Your requirements could not be resolved to an installable set of packages
Y tendremos un error como este:

```
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Conclusion: remove laravel/framework 5.1.x-dev
    - Conclusion: don't install laravel/framework 5.1.x-dev
    - barryvdh/laravel-debugbar 2.0.x-dev requires illuminate/support ~5.0.17 -> satisfiable by illuminate/support[5.0.x-dev, v5.0.22, v5.0.25, v5.0.26, v5.0.28].
    - barryvdh/laravel-debugbar v2.0.0 requires illuminate/support 5.0.* -> satisfiable by illuminate/support[5.0.x-dev, v5.0.0, v5.0.22, v5.0.25, v5.0.26, v5.0.28, v5.0.4].
    - barryvdh/laravel-debugbar v2.0.1 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, v5.0.0, v5.0.22, v5.0.25, v5.0.26, v5.0.28, v5.0.4].
    - barryvdh/laravel-debugbar v2.0.2 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, v5.0.0, v5.0.22, v5.0.25, v5.0.26, v5.0.28, v5.0.4].
    - barryvdh/laravel-debugbar v2.0.3 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, v5.0.0, v5.0.22, v5.0.25, v5.0.26, v5.0.28, v5.0.4].
    - don't install illuminate/support 5.0.x-dev|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.22|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.25|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.26|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.28|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.0|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support v5.0.4|don't install laravel/framework 5.1.x-dev
    - Installation request for laravel/framework 5.1.* -> satisfiable by laravel/framework[5.1.x-dev].
    - Installation request for barryvdh/laravel-debugbar ^2.0@dev -> satisfiable by barryvdh/laravel-debugbar[2.0.x-dev, v2.0.0, v2.0.1, v2.0.2, v2.0.3].


Installation failed, reverting ./composer.json to its original content.
```

Vamos a ver qué significa este mensaje de error. Como dice Duilio, un buen programador es aquel que sabe leer e interpretar los errores. Y yo estoy totalmente de acuerdo.

```
Conclusion: don't install laravel/framework 5.1.x-dev
``` 

Este es el paquete que es incompatible con el paquete que estamos instalando. 

```
- barryvdh/laravel-debugbar 2.0.x-dev requires illuminate/support ~5.0.17 -> satisfiable by illuminate/support[5.0.x-dev, v5.0.22, v5.0.25, v5.0.26, v5.0.28].
```

Este mensaje nos dice para la versión `2.0.x-dev` de `barryvdh/laravel-debugbar` que sus requerimientos son `illuminate/support ~5.0.17` y que cualquiera de las versiones indicadas `[5.0.x-dev, v5.0.22, v5.0.25, v5.0.26, v5.0.28]` podrían instalarse con esos requerimientos.

Esa es la primera versión de la lista porque es la que más encaja dentro de nuestros requerimientos actuales de estabilidad. Ya que si miras el fichero `composer.json` de la aplicación que acabas de crear, verás que el mínimo nivel de estabilidad está marcado a `dev`.

```
	},
	"minimum-stability": "dev",
	"prefer-stable": true
}
```

> Puedes instalar una versión distinta, especificándola en el comando `composer require`. 
> Por ejemplo: `composer require barryvdh/laravel-debugbar 2.0.3 --dev`

Volviendo al mensaje de error, además de la última versión (`2.0.x-dev`) nos muestra la información de requerimientos de las versiones más recientes del paquete, desde la `v2.0.0` a la `v2.0.3`.

Pasamos ahora al siguiente apartado:

```
- don't install illuminate/support 5.0.x-dev|don't install laravel/framework 5.1.x-dev
- don't install illuminate/support v5.0.22|don't install laravel/framework 5.1.x-dev
- don't install illuminate/support v5.0.25|don't install laravel/framework 5.1.x-dev
(...)
```

Estos mensajes nos avisan de incompatibilidades entre paquetes. Es decir, `illuminate/support 5.0.x-dev` es incompatible con `laravel/framework 5.1.x-dev`, etc...

Y por último, nos dice a partir de las versiones que hemos indicado en nuestra línea de comandos o en nuestro fichero `composer.json`, qué versiones se han seleccionado para instalar de cada paquete:

```
- Installation request for laravel/framework 5.1.* -> satisfiable by laravel/framework[5.1.x-dev].
- Installation request for barryvdh/laravel-debugbar ^2.0@dev -> satisfiable by barryvdh/laravel-debugbar[2.0.x-dev, v2.0.0, v2.0.1, v2.0.2, v2.0.3].
```

Como ves, tenemos mucha información que nos ayudará a resolver el problema de incompatibilidades.

Un gran porcentaje de las veces en las que aparece este problema, es debido a utilizar versiones genéricas o ramas (como `dev-master`) en los requerimientos en lugar de usar versiones exactas (como `5.0.28`). En esas ocasiones, a partir de la información del mensaje de error, prueba a modificar el requerimiento en el comando `composer require` o en el fichero `composer.json` por una versión concreta de las que te ha mostrado el mensaje como disponibles.

###Usando los Alias de Composer

Cuando la opción de indicar una versión exacta no funciona, tenemos como solución los **Alias** de Composer.

Un Alias es un modo de engañar a un paquete y 'colarle gato por liebre' dándole una versión que no es la que esperaban. En el ejemplo que estamos viendo, `barryvdh/laravel-debugbar` pide una versión `5.0.x` que, al parecer es incompatible con la que tenemos instalada. Vamos crear un alias a 'engañar' a `barryvdh/laravel-debugbar` y decirle que la que tenemos instalada es una versión `5.0.x`.

El primer paso es saber qué versión tenemos instalada, ya que los Alias hay que definirlos con una versión exacta. Para eso ejecuta el comando:

```
composer show --installed
```

Con ese comando obtenemos una lista de todos los paquetes instalados y su versión. Revisando el listado, deberíamos encontrar una línea parecida a esta:

```
illuminate/support              5.1.x-dev        Illuminate Support Component
```

> También puedes ejecutar `composer show illuminate/support --installed` para obtener sólo la información de un paquete.

Sorprendentemente, no aparece ese paquete por ningún sitio. Esto es porque es un caso especial que voy a explicar más adelante. Por ahora, tendrás que creerme y aceptar que la versión que tenemos instalada es la `5.1.x-dev`.

Los alias se definen en el fichero `composer.json`. Vamos a tomar la información que nos daba esta línea:

```
    - barryvdh/laravel-debugbar 2.0.x-dev requires illuminate/support ~5.0.17 -> satisfiable by illuminate/support[5.0.x-dev, v5.0.22, v5.0.25, v5.0.26, v5.0.28].
```

Y le vamos a decir que tenemos instalada la versión `illuminate/support:5.0.17`.

Modifica tu fichero `composer.json` y añade el Alias:

```
	"require": {
		"laravel/framework": "5.1.*",
		"illuminate/support": "5.1.x-dev as 5.0.17"
	},
```

> Cuando añades una nueva línea, recuerda añadir la ',' en la línea anterior.

Ya estamos preparados para instalar de nuevo `barryvdh/laravel-debugbar`. Ejecuta otra vez el siguiente comando:

```
composer require barryvdh/laravel-debugbar --dev
```

!!!Volvemos a tener un error! Aunque esta vez al menos el mensaje es más pequeño. 

```
Using version ^2.0@dev for barryvdh/laravel-debugbar
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Conclusion: remove laravel/framework 5.1.x-dev
    - Conclusion: don't install laravel/framework 5.1.x-dev
    - barryvdh/laravel-debugbar 2.0.x-dev requires illuminate/support ~5.0.17 -> satisfiable by illuminate/support[5.0.x-dev, 5.0.17].
    - barryvdh/laravel-debugbar v2.0.0 requires illuminate/support 5.0.* -> satisfiable by illuminate/support[5.0.x-dev, 5.0.17].
    - barryvdh/laravel-debugbar v2.0.1 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, 5.0.17].
    - barryvdh/laravel-debugbar v2.0.2 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, 5.0.17].
    - barryvdh/laravel-debugbar v2.0.3 requires illuminate/support 5.0.x -> satisfiable by illuminate/support[5.0.x-dev, 5.0.17].
    - don't install illuminate/support 5.0.x-dev|don't install laravel/framework 5.1.x-dev
    - don't install illuminate/support 5.0.17|don't install laravel/framework 5.1.x-dev
    - Installation request for laravel/framework 5.1.* -> satisfiable by laravel/framework[5.1.x-dev].
    - Installation request for barryvdh/laravel-debugbar ^2.0@dev -> satisfiable by barryvdh/laravel-debugbar[2.0.x-dev, v2.0.0, v2.0.1,
```

En situaciones normales, ya habríamos solucionado el conflicto y la mayoría de las veces, será suficiente con los pasos anteriores para resolver tus conflictos de paquetes utilizando Alias en Composer.

###Dependencias y requerimientos en paquetes 'subtree'.

Sin embargo, he elegido este ejemplo porque como comentaba anteriormente, el paquete `illuminate/support` es un caso especial. Y de ese modo, puedo enseñarte también cómo resolver estos casos especiales. 

> Si eres un lector avispado, seguro que el título de este apartado, ya te ha dado una 'pista' de porqué es especial este caso.

El mensaje anterior nos da una información bastante similar a la que ya teníamos. Simplemente, como hemos usado una versión más exacta, la `5.0.17` en el paquete `illuminate/support`, sólo nos proporciona información referente a esa versión.

Necesitamos por tanto más información. Como tenemos tres paquetes (`laravel/framework`, `barryvdh/laravel-debugbar` y `illuminate/support`) y hay tres modos de obtener la información que necesitamos, vamos a usar un modo distinto para cada uno de ellos y así conoces los tres y puedes elegir cual de ellos prefieres.

####Obteniendo información de dependencias con Composer.

Dado que estamos con Composer, vamos a utilizar Composer para obtener esa información. Ejecuta el siguiente comando:

```
composer show barryvdh/laravel-debugbar
```

Y tendremos toda la información sobre ese paquete, junto con sus requerimientos:

```
name     : barryvdh/laravel-debugbar
descrip. : PHP Debugbar integration for Laravel
keywords : debug, debugbar, laravel, profiler, webprofiler
versions : * v2.0.3
type     : library
license  : MIT License (MIT) (OSI approved) http://spdx.org/licenses/MIT#licenseText
source   : [git] https://github.com/barryvdh/laravel-debugbar.git 77be5170f3777e2e899ec98105ce5686cd4aa63b
dist     : [zip] https://api.github.com/repos/barryvdh/laravel-debugbar/zipball/77be5170f3777e2e899ec98105ce5686cd4aa63b 77be5170f3777e2e899ec98105ce5686cd4aa63b
names    : barryvdh/laravel-debugbar

autoload
psr-4
Barryvdh\Debugbar\ => src/
files

requires
illuminate/support 5.0.x
maximebf/debugbar ~1.10.2
php >=5.4.0
symfony/finder ~2.6
```

Vemos, que efectivamente, tal y como nos muestra el mensaje de error, este paquete tiene definida la versión `5.0.x` (`illuminate/support 5.0.x`) como requerimiento.

####Obteniendo información de dependencias desde el fichero `composer.json` del repositorio de origen.

Para el paquete `laravel/framework`, vas a utilizar otro sistema. Sabemos el nombre del paquete y la versión instalada (`5.1.x-dev`) gracias a este mensaje:

```
Conclusion: remove laravel/framework 5.1.x-dev
```

1. Abrimos el navegador y nos vamos a la web de **Packagist**.

2. Buscamos el paquete: (https://packagist.org/packages/laravel/framework). Buscamos nuestra versión en la lista de paquetes y vemos que la versión `5.1.x-dev` se corresponde con la rama `master` del repositorio de origen: `dev-master / 5.1.x-dev reference: ea0fc7f`.

3. Hacemos click en el enlace de `Source` para irnos al código fuente. (Source: https://github.com/laravel/framework). 

4. Buscamos la rama que tenemos instalada `master` y en ella, el commit cuyo hash comienza por `ea0fc7f`. (https://github.com/laravel/framework/commit/ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4)

5. Hacemos click en `Browse files`.

> Si el paquete está en GitHub, un truco para ir directamente al código del commit que estamos buscando, es escribir directamente la url de este modo: `https://github.com/<paquete>/tree/<hash>`. Por ejemplo, en nuestro caso, con la referencia que nos dio Packagist, el link sería: https://github.com/laravel/framework/tree/ea0fc7ff

6. Vamos a mirar ahora el [fichero](https://github.com/laravel/framework/blob/ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4/composer.json]) `composer.json`.

```
    "replace": {
        "illuminate/auth": "self.version",
		(...)
        "illuminate/routing": "self.version",
        "illuminate/session": "self.version",
        "illuminate/support": "self.version",
		(...)
        "illuminate/view": "self.version"
    },
```

 Vemos que el paquete `illuminate/support` no aparece en la sección `require`, sino en otra sección llamada replace. Este era nuestro 'caso especial'. Ya que `laravel/framework` está utilizando `illuminate/support` como un `subtree`, es decir, como un 'subpaquete' y por eso está declarado en la sección `replace`. Por tanto, cuando instalamos `laravel/framework`, `illuminate/support` se instala como parte de este y por eso no nos aparecía como paquete instalado.
 
 La expresión `"self.version"` quiere decir que tomará la versión del paquete padre, por eso al comienzo de este apartado os pedí que aceptarais que la versión que teníamos instalada de `illuminate/support` era la `5.1.x-dev`. (Que es la que tenemos instalada de `laravel/framework`).

> La tercera opción para buscar esta información es la página de **Packagist** del paquete. En ella aparece también toda la información sobre los requerimientos.
> 
> Por ejemplo, para el paquete barryvdh/laravel-debugbar, verás que los requerimientos que aparecen son los mismos que en el fichero `composer.json`, ya que se extraen directamente de él:
>
>```
Requires
maximebf/debugbar: ~1.10.2
php: >=5.4.0
symfony/finder: ~2.6
illuminate/support: ~5.0.17
```
> Y en la página de `laravel/framework` aparece, por supuesto, `illuminate/support` en la sección `replace`.
>
> Habrá ocasiones en las que estemos instalando ramas o paquetes que no aparecen en **Packagist** y por eso hemos visto cómo hacerlo mirando el fichero `composer.json`.

Esta es la versión 'detective'. Si prefieres la versión rápida de Composer que hemos visto en el apartado anterior, podrías haber obtenido la misma información con Composer, ejecutando `composer show laravel/framework --installed`:

```
name     : laravel/framework
descrip. : The Laravel Framework.
keywords : framework, laravel
versions : * dev-master, 5.1.x-dev
type     : library
license  : MIT License (MIT) (OSI approved) http://spdx.org/licenses/MIT#licenseText
source   : [git] https://github.com/laravel/framework.git ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4
dist     : [zip] https://api.github.com/repos/laravel/framework/zipball/ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4 ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4

(...)

requires
classpreloader/classpreloader ~1.2
danielstjules/stringy ~1.8
(...)
symfony/translation 2.7.*
symfony/var-dumper 2.7.*
vlucas/phpdotenv ~1.0

(...)

replaces
illuminate/auth self.version
(...)
illuminate/routing self.version
illuminate/session self.version
illuminate/support self.version
illuminate/translation self.version
(...)
```

Como ves, tenemos igualmente la versión instalada, que es la que tiene un asterisco: `versions : * dev-master, 5.1.x-dev`.

Tenemos el repositorio de origen `source   : [git] https://github.com/laravel/framework.git ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4` incluso con el hash del commit que tenemos instalado, con lo que podríamos construir fácilmente el link para ver ese código en github: https://github.com/laravel/framework/tree/ea0fc7ff2fc4f0319142ad7decc9528f3cfd96b4

Y tenemos incluso la información de los 'requires' y en este caso, podemos ver cómo `illuminate/support` aparece en los 'replaces'.

####Definiendo un Alias para un subtree.

De nuevo, vamos a 'engañar' al paquete `barryvdh/laravel-debugbar` y le vamos a decir que tenemos instalada la versión `illuminate/support:5.0.17`. Pero, en este caso, dado que ese paquete es un `subtree` de `laravel/framework` y toma la versión de este, definimos el alias en `laravel/framework`.

Modifica tu fichero `composer.json` y añade el Alias:

```
	"require": {
		"laravel/framework": "5.1.x-dev as 5.0.17"
	},
```

Ya estamos preparados para instalar de nuevo `barryvdh/laravel-debugbar`. Ejecuta otra vez el siguiente comando:

```
composer require barryvdh/laravel-debugbar --dev
```

Y, finalmente tienes instalado el paquete `barryvdh/laravel-debugbar` y has aprendido a utilizar los Alias de Composer y a solucionar conflictos de requerimientos incluso en los casos más especiales, como en los subtrees.

Ahora ya puedes instalar tus paquetes preferidos sobre aplicaciones Laravel 5.1 aunque no estén actualizadas.

```
Using version ^2.0@dev for barryvdh/laravel-debugbar
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing maximebf/debugbar (v1.10.4)
    Loading from cache

  - Installing barryvdh/laravel-debugbar (v2.0.3)
    Loading from cache

maximebf/debugbar suggests installing kriswallsmith/assetic (The best way to manage assets)
maximebf/debugbar suggests installing predis/predis (Redis storage)
Writing lock file
Generating autoload files
Generating optimized class loader
```
