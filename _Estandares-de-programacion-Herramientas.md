##Estándares de programación - Utilidades.

Un pequeño tip sobre cómo adaptar tu código automáticamente a los estándares PSR-1 y PSR-2.

###PHP Coding Standards Fixer

Esta es una herramienta que formateará tu código para que siga los estándares de PSR-1 y PSR-2.

Es tan sencilla de utilizar como irte a su [Página Web](http://cs.sensiolabs.org/) descargar el fichero `php-cs-fixer.phar` e instalarlo en tu sistema como instalarías cualquier otro fichero `.phar`.

También lo puedes instalar con Composer.

Una vez lo tengas instalado, símplemente ejecuta `php php-cs-fixer.phar fix </ruta/de/tu/proyecto>` y tendrás todo tu código adaptado a esos estándares.

**Parámetros:**

- `--verbose`

 Muestra los cambios que va realizando en tu código.
 
- `--level`

 Te permite limitar qué correcciones quieres aplicar: `psr0`,`psr1`,`psr2`,`symfony`. Por defecto, se aplica el nivel PSR-2 y algunos niveles adicionales.
 
 ```php php-cs-fixer.phar fix </ruta/de/tu/proyecto> --level=symfony```

- `--fixers`

 Te permite especificar o excluir correcciones concretas. Separa varias correcciones con comas y precede las que quieras excluir con un guión. En su web tienes un listado de todas las correcciones disponibles.
 
 Por ejemplo, para aplicar correctamente las llaves y cambiar los tabs de indentación por espacios, usa:
 
 ```php php-cs-fixer.phar fix </ruta/de/tu/proyecto> --fixers=braces,indentation```

 Si quieres que te haga todas las correcciones excepto modificar las llaves, usar:
 
 ```php php-cs-fixer.phar fix </ruta/de/tu/proyecto> --fixers=-indentation```

-  `--dry-run` y `--diff`

 Si indicas estos dos parámetros. Los ficheros no se cambiarán y únicamente te mostrará un informe sobre los cambios que se realizarían.

> Para adaptar tu aplicación Laravel a los estándares, aplica esta herramienta únicamente en los directorios `/app`, `/database` y `/tests`.

**Plugins**

Hay plugins para algunos de los editores más conocidos:

* [Vim](https://github.com/stephpy/vim-php-cs-fixer)
* [Sublime Text](https://github.com/benmatselby/sublime-phpcs)
* [NetBeans](http://plugins.netbeans.org/plugin/49042/php-cs-fixer)
* [PhpStorm](http://sergigp.com/phpstorm-integrando-herramientas-de-calidad-de-codigo)
 
<br>  
###Fuentes y más información:

[PHP Coding Standards Fixer](http://cs.sensiolabs.org/)

[PHPSTORM: INTEGRANDO HERRAMIENTAS DE CALIDAD DE CÓDIGO](http://sergigp.com/phpstorm-integrando-herramientas-de-calidad-de-codigo)

[Estándares de programación en PHP (PSR-1 y PSR-4)](Est%C3%A1ndares-de-programaci%C3%B3n-PSR-1-y-PSR-4)   

[Estándares de programación en PHP (PSR-2)](Est%C3%A1ndares-de-programaci%C3%B3n-PSR-2)

[Estándares de programación en Laravel](Est%C3%A1ndares-de-programaci%C3%B3n-en-Laravel)

[Estándares de programación - Resumen](Est%C3%A1ndares-de-programaci%C3%B3n--Resumen)

[Guía de Contribución de Laravel - Estilo del Código](http://laravel.com/docs/5.0/contributions#coding-style)   

[Discusión acerca del Estilo de Código del Framework Laravel abierta por GrahamCampbell en GitHub](https://github.com/laravel/framework/issues/6836)   

[Estándar de código de Symfony](http://symfony.com/doc/current/contributing/code/standards.html)

[Estándar PSR-2](http://www.php-fig.org/psr/psr-2/)  

[Estándar PSR-4](http://www.php-fig.org/psr/psr-4/)  

###ToDo:

https://web.archive.org/web/20140914225937/http://arnolog.net/post/92715936483/use-fabpots-php-cs-fixer-tool-in-phpstorm-in-2-steps

https://plus.google.com/wm/4/+PedroLuz/posts/5sZVLv9y5Hu

https://www.snip2code.com/Snippet/429871/Run-php-cs-fixer-in-current-directory-fo

http://sergigp.com/phpstorm-integrando-herramientas-de-calidad-de-codigo

https://gist.github.com/mpalourdio/46f792347cf9d46b121c