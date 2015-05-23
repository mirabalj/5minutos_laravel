##Composer. Resumen sobre resolución de problemas

###Problemas generales de Composer

1. **Revisa la configuración del sistema.**
 `composer diagnose`

2. **Actualiza tu versión de Composer.**
 `composer self-update`
 
3. **Aumenta el nivel de detalle de los mensajes.** 
 `-vvv`

4. **Borra la carpeta `vendor` y el fichero `composer.lock` y ejecuta `composer install`.** 
 
5. **Vacía la caché interna.**

 `composer clear-cache`
 `composer install` o `composer update`

###Errores concretos.

- **Composer es muy lento al actualizar/instalar los paquetes.**
 **Desactiva xDebug** en tu fichero `php.ini` mientras usas Composer comentando las líneas correspodientes. 

 > Con xDebug activado, el proceso de instalación y actualización de Composer puede durar **hasta 20 veces más!!**.
 
 > **Nota**: Comprueba si tienes xDebug activado. (Aparecerá `xdebug`) 
 > Windows: `php -m | findstr xdebug`.
 > Unix: `php -m | grep xdebug`.
 
 Utiliza `--prefer-dist` para descargar los paquetes en formato .zip.
 
- **Error de Timeout.**

 `composer config --global process-timeout 1500` 

 > También puedes configurar el Timeout con la variable de entorno `COMPOSER_PROCESS_TIMEOUT`.

- **Warning: Fichero lock desactualizado.**

 ```
 Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, 
 run update to update them.
```
 `composer update --lock`.

- **Warning: extensión openssl.**

 `openssl extension is missing` o `must enable the openssl extension`
 Habilita la extensión `php_openssl.dll` en tu fichero `php.ini`.
 
- **'Your requirements could not be resolved to an installable set of packages'.**

 > **Nota:** Comprueba en primer lugar que los nombres de los paquetes están escritos correctamente.

 1. Borra `"prefer-stable": true` de tu fichero `composer.json`.
 2. Añade `"minimum-stability": "dev"`
 3. Sube estabilidad y prueba.
 4. Añade `"prefer-stable": true`.

 Define un [Alias](Solucionar-conflictos-de-versiones-y-requerimientos-en-Composer-(Alias).

- **Error 503 al descargar los ficheros .zip de GitHub.**

 `composer install --prefer-source` o  `composer update --prefer-source`.
 
###Importante

- Añade los ficheros `composer.json` y `composer.lock` a tu repositorio Git.
- Usa `composer install` en lugar de `composer update`.
- En producción, usa `--no-dev` a no ser que necesites instalar las librerías de desarrollo.
- En producción, usa `--optimize-autoloader`.
- Combina `"minimum-stability": "dev"` con `"prefer-stable": true`.
- Recuerda utilizar el parámetro `--dev` del comando `composer require`.
- Antes de un `push`, ejecuta `composer validate` para validar tu fichero `composer.json`.
- Añade siempre la carpeta `vendor` a tu fichero `.gitignore`.

Tienes más información en el artículo que he publicado en Styde.net: [Composer: Resolviendo problemas](https://styde.net/composer-resolviendo-problemas/)

###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Diferencias entre composer install y composer update.](Composer.-Resumen---Diferencias-entre-composer-install-y-composer-update)

[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload)

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)

[Solucionar conflictos de versiones y requerimientos en Composer (Alias)](Solucionar-conflictos-de-versiones-y-requerimientos-en-Composer-(Alias)