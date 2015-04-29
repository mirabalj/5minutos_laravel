###Diferencias entre composer Install y Update.

En las aplicaciones web cuyas dependencias están gestionadas con **Composer**, la convención habitual es no subir a los repositorios los paquetes que no forman parte de la aplicación. Es decir, los que están en la carpeta `vendor`.

De hecho, habitualmente los ficheros `.gitignore` de ese tipo de aplicaciones, como por ejemplo las desarrolladas en Laravel, suelen tener este contenido:

```
/vendor
/node_modules
.env
```

Por tanto, si hemos clonado una aplicación en nuestro ordenador, el primer paso para poder ejecutarla es generar esa carpeta.

Para eso tenemos dos comandos de Composer: `install` y `update`.

###Comando install

Para gestionar las dependencias de nuestra aplicación con Composer, se configuran en el fichero `composer.json`.

La primera vez que ejecutamos `composer install` en un proyecto, Composer lee ese fichero, resuelve las dependencias que hay en él  e instala los paquetes en el directorio `vendor`.

> La versión de cada paquete depende de los parámetros proporcionados y las versiones configuradas para cada paquete y la configuración de estabilidad especificadas en el fichero `composer.json`. 

Después de instalar los paquetes, crea en el directorio donde se ha ejecutado el fichero `composer.lock` en el que anota todos los paquetes instalados y la versión instalada de cada uno de ellos.

Las próximas veces que se ejecute `composer install` en ese proyecto, leerá ese fichero e instalará aquellos paquetes que aparezcan en el fichero y no estén en el directorio `vendor`.

> Este comando hay que ejecutarlo desde el directorio raíz de la aplicación. (O desde un subdirectorio si queremos instalar únicamente las dependencias del fichero `composer.json` de ese directorio).

###Comando update.

El comando `composer update` lee **SIEMPRE** el fichero `composer.json` e instala las dependencias de ese fichero.

Después de instalar los paquetes, crea en el directorio donde se ha ejecutado el fichero `composer.lock` o **lo actualiza** si ya existe.

###Diferencias.

Por tanto, la diferencia fundamental es que `composer install` a excepción de la primera vez que se ejecuta, está pensado para que todos los usuarios y desarrolladores de ese paquete **compartan el mismo entorno** y las mismas versiones de cada paquete.

Mientras que `composer update` te permite actualizar todos los paquetes que utiliza tu aplicación.

###Resumen.

- Cuando clones una aplicación en la que vas a colaborar, si no existe el directorio `vendor`, ejecuta `composer install` para crear ese directorio y tener el mismo entorno que el resto de desarrolladores.

- Cuando clones una aplicación para utilizarla o probarla, puedes optar por cualquiera de las dos alternativas. Pero ten en cuenta que la aplicación ha sido testada con un entorno concreto. Si ejecutas `composer update` y la aplicación da errores. Sustituye el fichero `composer.lock` por la versión original del repositorio y ejecuta `composer install` para comprobar que no sea errores de compatibilidad con nuevas versiones de los paquetes.

- Cuando distribuyas tu aplicación, asegúrate de incluir el fichero `composer.lock` para que tus usuarios o miembros del equipo puedan usarla con el mismo entorno que tú tienes.

- Si al ejecutar una aplicación te aparece el siguiente error, comprueba si existe el directorio `vendor`. Y si es necesario ejecuta `composer install` o `composer update`.
 ```
PHP Fatal error:  require(): Failed opening required '(...)\bootstrap/../vendor/autoload.php' (include_path='.;C:\php\pear') in (...)\bootstrap\autoload.php on line 17
```

- No confundas esos comandos con `composer dump-autoload`.  `composer dump-autoload` **no descarga nada** y únicamente actualiza los ficheros de autocarga `autoload.php` de la aplicación. Se usa cuando hemos añadidos nuevas clases a nuestros proyectos y hemos actualizado el fichero `composer.json`.

###Más información:
Link a los ignores
[install command - Composer official docs](https://getcomposer.org/doc/03-cli.md#install)   
[update command - Composer official docs](https://getcomposer.org/doc/03-cli.md#update)  
[dump-autoload command - Composer official docs](https://getcomposer.org/doc/03-cli.md#dump-autoload)  
[Managing Dependencies with Composer - Beau Simensen, MidWest PHP Talks, March 15th, 2014](https://beau.io/talks/2014/03/15/managing-dependencies-with-composer-midwest-php-2014/)  
[xxxAutocarga de clases en Laravel (Autoload)](xxxx)