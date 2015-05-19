###Pruebas Unitarias (Unit tests)

Se ejecutan a través de PHPUnit.

Ventajas:
 * Cualquier PHPUnit test puede ser ejecutado por Codeception.
 * Si ya tienes experiencia con PHPUnit, puedes escribir los tests como siempre.
 * Codeception ha añadido algunas funciones para facilitar la creación de tests con PHPUnit
 
Más info: http://codeception.com/docs/06-UnitTests
	
###Instalar Codeception:

Descargar de: http://codeception.com/install

 * Opción fichero Phar.
	Descargar el fichero codecept.phar

###Generar los ficheros iniciales:
`codecept bootstrap`

> Si lo has instalado con composer, usa: `./vendor/bin/codecept bootstrap`

```
Initializing Codeception in <path_del_proyecto>

File codeception.yml created       <- global configuration
tests/unit created                 <- unit tests
tests/unit.suite.yml written       <- unit tests suite configuration
tests/functional created           <- functional tests
tests/functional.suite.yml written <- functional tests suite configuration
tests/acceptance created           <- acceptance tests
tests/acceptance.suite.yml written <- acceptance tests suite configuration
tests/_output was added to .gitignore
tests/_bootstrap.php written <- global bootstrap file
Building initial Tester classes
Building Actor classes for suites: acceptance, functional, unit
\AcceptanceTester includes modules: PhpBrowser, AcceptanceHelper
AcceptanceTester.php generated successfully. 51 methods added
\FunctionalTester includes modules: Filesystem, FunctionalHelper
FunctionalTester.php generated successfully. 13 methods added
\UnitTester includes modules: Asserts, UnitHelper
UnitTester.php generated successfully. 19 methods added

Bootstrap is done. Check out <path_del_proyecto>/tests directory
```

###Escenarios

Los nombres de los ficheros de PHP que representen un escenario deben acabar en Cept.

Para crear un escenario:
`php codecept.phar generate:cept <tipo> <nombre>`

Por ejemplo, con `php codecept.phar generate:cept acceptance Home` se crearía el fichero `tests/acceptance/HomeCept.php`

	
###Instalar un módulo para las Pruebas Funcionales

Configurar en el fichero `tests\functional.suite.yml` el Framework que estamos utilizando:

```
class_name: FunctionalTester
modules:
    enabled: [Filesystem, FunctionalHelper, Laravel5]
```

Ejecutar `codecept build` para instalar el nuevo módulo.

```
cc build

Building Actor classes for suites: acceptance, functional, unit
\AcceptanceTester includes modules: PhpBrowser, AcceptanceHelper
AcceptanceTester.php generated successfully. 51 methods added
\FunctionalTester includes modules: Filesystem, FunctionalHelper, Laravel5
FunctionalTester.php generated successfully. 78 methods added
\UnitTester includes modules: Asserts, UnitHelper
UnitTester.php generated successfully. 19 methods added
```

###Configurar
`<app_path>/codeception.yml`

Para tener colores en windows:

```
settings:
    bootstrap: _bootstrap.php
    colors: true
```

> Necesitas instalar ansicon: `https://github.com/adoxa/ansicon`

[Documentación oficial y módulo para Laravel 5](http://codeception.com/docs/modules/Laravel5)
[Poryecto Demo de Laravel 5](https://github.com/janhenkgerritsen/codeception-laravel5-sample)

	
http://codeception.com/10-30-2012/pro-tips-1.html

http://laravelista.com/api-testing-using-codeception-and-laravel/
http://themsaid.github.io/2014/12/28/smooth-database-testing-in-laravel/

TestDummy:
http://www.neontsunami.com/posts/laravel-controller-testing-with-testdummy-2
https://infinit.io/link/Jeffrey_Jordan_Way/9KniZRd.png
https://github.com/laracasts/TestDummy