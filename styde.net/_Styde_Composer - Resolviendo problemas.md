##Composer: Resolviendo problemas

Si usas habitualmente Composer, sabrás que los problemas con Composer no son fáciles de resolver.

En esta guía te daremos toda la información necesaria para resolver los problemas más habituales que te puedes encontrar con Composer.

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

 - **Desactiva xDebug** en tu fichero `php.ini` mientras usas Composer comentando las líneas correspondientes. 

 > Con xDebug activado, el proceso de instalación y actualización de Composer puede durar **hasta 20 veces más!!**.
 
 > **Nota**: Para comprobar si tienes xDebug activado, puedes usar el siguiente comando en Windows: `php -m | findstr xdebug` o `php -m | grep xdebug` en Unix. Si lo tienes activado, aparecerá la palabra `xdebug`.
 
 - Usar el mayor nivel de estabilidad posible aumentará ligeramente la velocidad de instalación/actualización de los paquetes.
 
 - Cambiar las dependencias para que sean lo más específicas posibles, también ayuda. Por ejemplo, cambia `"laravel/framework": "5.*"` por `"laravel/framework": "5.0.*"` o mejor aún, por `"laravel/framework": "5.0.28"`.
 
 - Si tienes configurado `"preferred-install": "source"`, utiliza `--prefer-dist` para descargar los paquetes en formato .zip y aumentar la velocidad.
 
 - Por último, puedes instalar [Satis](https://github.com/composer/satis) para mantener una copia estática en local de los paquetes que utilizas habitualmente. Y [utilizar un script php](http://melp.nl/2013/09/composer-create-a-local-package-repository-to-improve-speed/) para configurarlo.

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
 
  >**Truco:** Si no quieres instalar ni actualizar tus paquetes. Por ejemplo, porque has añadido un comentario al fichero `composer.json` o un parámetro de configuración.
  >
  >El comando `composer update --lock` actualiza únicamente el hash del fichero `composer.lock` y suprime ese warning.

- **Warning: extensión openssl.**

 Si obtienes un Warning de tipo `openssl extension is missing` o `must enable the openssl extension` al instalar o usar Composer, asegúrate de tener configurada la extensión `php_openssl.dll` en tu fichero `php.ini`.
 
 Busca la línea: `;extension=php_openssl.dll` y quita el `;` inicial, o, si no existe, añade la línea: 
 
 ```
 extension=php_openssl.dll
 ```

- **'Your requirements could not be resolved to an installable set of packages'.**

 > **Nota:** Comprueba en primer lugar que los nombres de los paquetes están escritos correctamente.

 Este error indica que según tus requerimientos mínimos de estabilidad, hay una dependencia para la que no se ha encontrado ninguna versión que cumpla esos requerimientos.
 
 Empieza configurando `"minimum-stability": "dev"` en tu fichero `composer.json`. Y borrando (si existiera) `"prefer-stable": true`. Si no te da problemas, puedes probar con mayores niveles de estabilidad y cuando hayas encontrado el mínimo necesario, añade `"prefer-stable": true` para asegurarte de que ese nivel se aplique únicamente en los paquetes que lo requieran, o configura el nuevo nivel de estabilidad de forma individual para cada paquete que lo necesite (puedes usar `composer show --installed` para ver las versiones instaladas).

 Si ninguno de los pasos anteriores te funciona, la solución es definir un [Alias](Solucionar-conflictos-de-versiones-y-requerimientos-en-Composer-(Alias)).

- **Error 503 al descargar los ficheros .zip de GitHub.**

 Prueba a usar `composer install --prefer-source` o  `composer update --prefer-source`.
 
###Recomendaciones

- Cuando distribuyas tu proyecto, recuerda añadir los ficheros `composer.json` y `composer.lock`.

- Generalmente usa `composer install` para instalar aplicaciones a no ser que tengas la certeza de que necesitas usar `composer update`.

- Utiliza `composer require` (en lugar de editar manualmente el fichero `composer.json`) para añadir paquetes a tu aplicación.

- Usa `--no-dev` a no ser que necesites instalar las librerías de desarrollo.

- Usa `--optimize-autoloader` en el entorno de producción para mejorar el rendimiento de tu aplicación.

- No uses `"minimum-stability": "dev"` en tus ficheros.json. En su lugar, configura las dependencias `dev` para los paquetes que las necesiten. O, al menos, si lo usas, combinalo con `"prefer-stable": true`.

- Utiliza el parámetro `--dev` del comando `composer require` cuando instales paquetes que vas a usar únicamente en el entorno de desarrollo. Como los paquetes de tests, seeders, depuración, complementos del IDE, etc..

- Antes de subir tus cambios al repositorio, ejecuta el comando `composer validate` para asegurarte de que tu fichero `composer.json` es correcto.

- No distribuyas ni subas a tu repositorio la carpeta `vendor`. Añade siempre esa carpeta a tu fichero `.gitignore`.

###Fuentes y más información:

[Descargar Composer](https://getcomposer.org)   

[Diferencias entre composer install y composer update.](Diferencias-entre-composer-install-y-composer-update)

[Autocarga de clases en Laravel (Autoload)](Autocarga-de-clases-en-Laravel-(Autoload))

[Hoja Resumen interactiva sobre Composer - By JoliCode](http://composer.json.jolicode.com/)

[Composer en castellano en LibrosWeb](http://librosweb.es/libro/composer/)

[Solucionar conflictos de versiones y requerimientos en Composer (Alias)](Solucionar-conflictos-de-versiones-y-requerimientos-en-Composer-(Alias))