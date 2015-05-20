###Composer. Resumen sobre configuración de las versiones, el origen y la estabilidad de los paquetes.

####Distribuciones.

Con Composer, puedes instalar un paquete de dos formas distintas: Desde el origen (`source`) y desde su versión de distribución (`dist`).

- `dist` (Distribución)

 - Este es el modo por defecto. 
 - En este modo, los paquetes se instalarán desde su repositorio de distribución. 
 - Habitualmente ese repositorio será **Packagist**.
 - Si los paquetes ya se descargaron anteriormente, los tomará de la cache local.
 - Esta versión descarga el paquete en formato zip si está disponible.

 > **Nota:** Usa esta opción para **acelerar** el proceso de instalación y/o si tienes problemas con la configuración de Git.

- `source` (Fuente u Origen)

 - Este modo es el contrario a `dist`. 
 - Permite descargar el paquete desde su repositorio de origen. 
 - Habitualmente ese repositorio será **GitHub, BitBucket o cualquier otro repositorio público**
 - Este sistema es útil si estás modificando ficheros fuente del paquete, ya que puedes indicar incluso un repositorio local.
 - Se usa por ejemplo, cuando quieres contribuir con un 'patch' a un proyecto open source.
 
 > **Nota:** Esta opción **requiere tener configurado Git** o un sistema compatible con el tipo de repositorio en el que estén los paquetes.  

> Puedes modificar la opción por defecto en el fichero de configuración global `config.json` o para un proyecto concreto en el fichero `composer.json`:
>
> `"preferred-install": "source"`

####Estabilidad.

- Configurar el nivel de estabilidad por defecto: Parámetro `minimum-stability` del fichero `composer.json`.
- Las opciones posibles son (En orden ascendente de estabilidad): `dev, alpha, beta, RC, y stable`.
- El valor por defecto es `stable`.

> **Nota:** Si composer no encuentra una versión que cumpla con el valor configurado por defecto en `minimum-stability`, **no instalará el paquete**.

- Añade `"prefer-stable": true` si necesitas usar `"minimum-stability": "dev"`.
- O especifica `dev` sólo para el paquete que lo necesite:

```
	"require-dev": {
		(...)
		"fzaninotto/faker": "1.4.*@dev",
		"barryvdh/laravel-ide-helper": "~2.0@dev"
	},
```

> **Nota:** Si modificas el parámetro `minimum-stability` de un fichero `composer.json`, y tienes algún error: Borrar el directorio `vendor` y el fichero `composer.json` y ejecuta `composer install`.

####Versiones de los paquetes:

Ejemplos:

`"laravel/framework": "5.0.12"`  
Instala específicamente la versión `5.0.12` del framework de Laravel.

`"laravel/framework": "4.2.*"`  
Instala cualquier versión mayor o igual a `4.2` y menor de `4.3`.

`"laravel/framework": "~5.0"`  
Instala cualquier versión mayor o igual a `5.0` y menor de `6.0.0`.

`"laravel/framework": "~5.1.2"`  
Instala cualquier versión mayor o igual a `5.1.2` y menor de `5.2.0`.

> **Recuerda:** `laravel/laravel` y `laravel/framework` son dos paquetes distintos. 


El sistema de comodines incluye más combinaciones. Tienes toda la información completa en la documentación oficial de Composer:  https://getcomposer.org/doc/01-basic-usage.md#package-versions

Tienes más información en el artículo que he publicado en Styde.net: [Controla las versiones de los componentes de tu proyecto con Composer](https://styde.net/control-de-versiones-origen-y-estabilidad-de-componentes-con-composer/)

####Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)

[Sistema de comodines para calcular la versión a instalar](https://getcomposer.org/doc/01-basic-usage.md#package-versions)