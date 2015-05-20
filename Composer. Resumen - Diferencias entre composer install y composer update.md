###Composer. Resumen - Diferencias entre composer install y composer update.


####Comando install

1. La primera vez (no existe `composer.lock`), lee las dependencias de nuestra aplicación del fichero `composer.json`.
2. Instala los paquetes.
3. Crea en el directorio donde se ha ejecutado el fichero `composer.lock` en el que anota todos los paquetes instalados y la versión instalada de cada uno de ellos.
4. Las próximas veces instala los paquetes especificados en `composer.lock`.

####Comando update.

1. Lee **SIEMPRE** el fichero `composer.json` e instala las dependencias de ese fichero.
2. Instala los paquetes.
3. Crea o actualizar el fichero `composer.lock`.
4. Las próximas veces ejecuta siempre desde el paso 1.

####Diferencias.

Por tanto, la diferencia fundamental es que `composer install` a excepción de la primera vez que se ejecuta, está pensado para que todos los usuarios y desarrolladores de ese paquete **compartan el mismo entorno** y las mismas versiones de cada paquete.

Mientras que `composer update` te permite actualizar todos los paquetes que utiliza tu aplicación.

> **Nota:** No confundas estos comandos con `composer dump-autoload` que **no descarga nada**.

Tienes más información en el artículo que he publicado en Styde.net: [Diferencias entre composer install y composer update](https://styde.net/diferencias-entre-composer-install-y-composer-update/)

###Fuentes y más información:

[install command - Composer official docs](https://getcomposer.org/doc/03-cli.md#install)   

[update command - Composer official docs](https://getcomposer.org/doc/03-cli.md#update)  