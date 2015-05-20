##Composer: Configurando las versiones, el origen y la estabilidad de los paquetes.

Composer utiliza un complejo sistema para abarcar el mayor número de escenarios posibles.

En este post vas a aprender a configurar los requerimientos de los paquetes de tu aplicación, la estabilidad mínima para esos paquetes (y, por tanto, para tu aplicación) y a utilizar orígenes alternativos para descargar esos paquetes.

###Distribuciones.

Con Composer, puedes instalar un paquete de dos formas distintas: Desde el origen (`source`) y desde su versión de distribución (`dist`).

- `dist` (Distribución)

 Este es el modo por defecto. En este modo, los paquetes se instalarán desde su repositorio de distribución. Habitualmente ese repositorio será **Packagist**.
 
 Si los paquetes ya se descargaron anteriormente, los tomará de la cache local.
 
 Esta versión descarga el paquete en formato zip si está disponible.
 
  > Usa esta opción para **acelerar** el proceso de instalación y/o si tienes problemas con la configuración de Git.

- `source` (Fuente u Origen)

 Este modo es el contrario a `dist`. Cuando usas `source`, si el repositorio indicado en la configuración como `source` está disponible, Composer lo usará para instalar los paquetes. Habitualmente ese repositorio será **GitHub, BitBucket o cualquier otro repositorio público**

 Este sistema es útil si estás modificando ficheros fuente del paquete. Ya que puedes indicar incluso un repositorio local.
 
 Se usa por ejemplo, cuando quieres contribuir con un 'patch' a un proyecto open source.
 
 > **Nota:** Esta opción **requiere tener configurado Git** o un sistema compatible con el tipo de repositorio en el que estén los paquetes.  

 <br>
Extracto del fichero `composer.json` de `laravel/framework` dónde se muestra como source su repositorio en GitHub.

 ```
    "support": {
        "issues": "https://github.com/laravel/framework/issues",
        "source": "https://github.com/laravel/framework"
    },
```

 > Puedes modificar la opción por defecto en el fichero de configuración global `config.json` o para un proyecto concreto en el fichero `composer.json` con:
>
> `"preferred-install": "source"`

###Estabilidad.

Cuando instalas los paquetes, puedes elegir qué nivel de estabilidad quieres como mínimo para las versiones instaladas de cada paquete. Composer instalará la última versión disponible que cumpla con el mínimo nivel indicado de estabilidad.

También puedes configurar el nivel de estabilidad por defecto con el parámetro `minimum-stability` del fichero `composer.json`.

Por ejemplo, puedes configurar el mínimo nivel de estabilidad a `desarrollo` en el fichero `composer.json` de proyectos que sólo vas a ejecutar en tu entorno local. Por ejemplo, para paquetes de pruebas y desarrollo.

`"minimum-stability": "dev"`

Y como mínimo 'Release Candidate' en proyectos que vas a pasar a tu entorno de producción:

`"minimum-stability": "RC"`

Las opciones posibles son (En orden ascendente de estabilidad): `dev, alpha, beta, RC, y stable`.

El valor por defecto es `stable`.

> **Nota:** Si composer no encuentra una versión que cumpla con el valor configurado por defecto en `minimum-stability`, **no instalará el paquete**.

Antes de que Composer incluyera la opción `prefer-stable`, la solución 'más sencilla' para no tener problemas al instalar paquetes era configurar `minimum-stability` al nivel más bajo posible, es decir, a `dev`. (Un ejemplo en estos momentos es la versión 5.1 de `laravel/framework`).

Pero, esa configuración tiene dos inconvenientes:

 - Estás incluyendo código que 'acaba de ser programado'. Incluso es posible que estés incluyendo el último 'commit'. Por tanto, estás poniendo en riesgo la estabilidad de tu aplicación.
 
 - Para cada nivel de estabilidad, Composer recorre todas las versiones posibles de ese paquete. Por lo que la velocidad de instalación y actualización de paquetes se reduce enormemente.
 
Sin embargo, a veces necesitamos un paquete para el que no existen recientes versiones marcadas como 'estables', o, peor aún, las únicas versiones recientes que existen son versiones `dev` que no se instalarán si nuestro `minimum-stability` está configurado por encima de `dev`.

**La solución fue la nueva opción `"prefer-stable": true`.**

> Esta opción funciona de un modo tan transparente que en algunos entornos se le llama 'la opción mágica'.

Con esta opción, le decimos a Composer que **preferimos** los paquetes estables, pero que si no los hay o los que hay no cumplen con los requerimientos o dependencias, **puede instalar** aquellos que cumplan con `minimum-stability`. De ese modo aumentamos el porcentaje de paquetes que se instalarán en su versión `stable` y sólo se instalará una versión de menor estabilidad cuando sea un requisito de alguna dependencia. 

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
> **Nota:** Si modificas el parámetro `minimum-stability` de un fichero `composer.json`, cambiarás los requerimientos de los paquetes instalados. 
>
> Si la aplicación te da problemas después de esa modificación, prueba a borrar el directorio `vendor` y el fichero `composer.lock` y ejecutar `composer install` para que se instalen todos los paquetes de nuevo.

####Versiones de los paquetes:

Composer usa un sistema de comodines para calcular la versión de cada uno de los paquetes que instalará en tu aplicación. Ese sistema se aplica a los paquetes configurados en el fichero `composer.json` y también se puede utilizar en la línea de comandos cuando especificamos un paquete.

Por ejemplo:

`"laravel/framework": "5.0.12"`

Instala específicamente la versión `5.0.12` del framework de Laravel.

`"laravel/framework": "4.2.*"`

Instala cualquier versión mayor o igual a `4.2` y menor de `4.3`.

`"laravel/laravel": "~5.0"`

Instala cualquier versión mayor o igual a `5.0` y menor de `6.0.0`.

`"laravel/laravel": "~5.1.2"`

Instala cualquier versión mayor o igual a `5.1.2` y menor de `5.2.0`.

> `laravel/laravel` y `laravel/framework` son dos paquetes distintos. 
>
>El primero es el 'esqueleto' de una aplicación Laravel para que lo uses como base de tu aplicación (contiene las carpetas `app`, `config`, `public`, `database`, etc..).   
>Se instala en el directorio raíz de tu aplicación. 
>
>El segundo es una dependencia del paquete `laravel/laravel` y contiene el framework, es decir, todo el núcleo y funciones internas de Laravel.   
>Se instala en el directorio `vendor`. 

Una vez calculadas las posibles versiones, se comprobará tu configuración de estabilidad y de las posibles opciones, se instalará la versión que cumpla con esas condiciones.

El sistema de comodines incluye más combinaciones. Tienes toda la información completa en la documentación oficial de Composer:  https://getcomposer.org/doc/01-basic-usage.md#package-versions

###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)

[Sistema de comodines para calcular la versión a instalar](https://getcomposer.org/doc/01-basic-usage.md#package-versions)