###Tipos de Tests:

Cuando damos nuestros primeros pasos en el mundo de los tests y de las pruebas del código y de las aplicaciones desarrolladas, a veces podemos sentirnos confundidos por la gran cantidad de categorías y distinciones que llegamos a encontrar. Los tipos de tests definidos y qué tipos de pruebas abarcan cada uno de esos tipos dependen de 'la escuela' o el grupo de desarrollo que los haya definido, del lenguaje y a veces incluso del framework.

En este posts voy a definir los tipos de tests más habituales para que te sean familiares. Por supuesto, no están todos los que existen, ni existen únicamente los que están.

**Tests Unitarios (Unit Tests)**

Estos tests suelen ser los primeros en escribirse, ya que la unidad básica de cualquier aplicación es un trozo de código, función o método. Podríamos decir que son las pruebas a más bajo nivel.

Un test unitario es un trozo de código (habitualmente una función o un método) desarrollado con el único fin de verificar que una rutina o función de nuestro código está funcionando como esperamos.

Estos tests, prueban la funcionalidad de cada método o función contemplando únicamente la lógica interna y sin tener en cuenta el resto de clases o sistemas.

La plataforma más conocida para este tipo de pruebas en PHP es PHPUnit.

Responsabilidad: Probar el código y detectar errores en los datos, lógica, flujos y algoritmos.

Ventajas:
 * Facilitan la integración. Ya que tenemos un alto porcentaje de probabilidades de que el código se esté ejecutando correctamente.

??Por eso, cuando nuestro código interactúa con otras unidades, se recomienda desarrollar una 'versión de Tests' para cada una de esas dependencias. Esas versiones se llaman mock, stub, dummy, etc.. Implementaríamos por tanto dos versiones de cada unidad: La que ejecuta la lógica real de la aplicación y la que ejecutará una 'simulación' para nuestras pruebas unitarias.??

En esta fase probamos que el código responda a como nosotros esperamos, independientemente de las especificaciones del cliente.

 
**Pruebas Funcionales**  
Pruebas para encontrar discrepancias entre las funcionalidades del programa y la especificación funcional. Permiten detectar errores en la implementación de los requerimientos.

Son similares a las de Aceptación excepto que son técnicas e incluyen las especificaciones funcionales. Por eso, a veces se les llama "Pruebas de Aceptación Funcionales".

Suelen ejecutarse en entornos controlados. Por ejemplo, una base de datos de pruebas, para que los datos no cambien durante las pruebas.

**Pruebas de Integración**
Una vez se aprueban las pruebas unitarias, se prueban todos los elementos que componen un proceso. Permiten detectar errores de interfaces y relaciones entre los componentes o módulos de un sistema.

Estas pruebas son similares a las Funcionales, pero se ejecutan sobre el entorno real e interaccionando realmente con el resto de sistemas. Suelen ejecutarse para asegurarse que la aplicación funciona después de pasarla a Producción.

**Pruebas de Validación**
Pruebas para comprobar que la aplicación se corresponde con lo que el cliente quería. Prueban cómo se comporta la aplicación desde el punto de vista del usuario.

**Pruebas de Sistema**
Pruebas globales de la aplicación.

**Pruebas de Aceptación**
Pruebas que realizan personas que no son técnicas como testers, usuarios o clientes para comprobar que la aplicación funciona correctamente. Permiten detectar fallos en la implementación del sistema.

La plataforma más conocida es Codeception. Y los navegadores especiales: Selenium y WebDriver

Exiten un tipo de pruebas de aceptación que se llama: Pruebas Automatizadas. En ese caso, las pruebas deben recibir siempre los mismos datos y generar siempre los mismos resultados. Para eso se desarrollan clases Mock y Stub tal como hacíamos en las pruebas unitarias.

A veces se les llama también "Pruebas de Aceptación No Funcionales"

**Pruebas de Regresión**
Después de hacer un cambio en la aplicación. Si se producen errores o comportamientos no inesperados que antes no existían, son las pruebas que se realizan para descubrir las causas.

Pueden ser de Aceptación o Funcionales. Suelen ser pruebas de funcionamiento básico y son especialmente útiles en aplicaciones de las que no tenemos documentación ni pruebas.

**Pruebas de Stress**
Son las pruebas que se realizan para observar el comportamiento de la aplicación bajo una gran cantidad de peticiones. Permiten detectar cuellos de botella en la aplicación.

Una plataforma para realizar este tipo de pruebas es [JMeter](http://jmeter.apache.org/) de Apache.

**Pruebas de Prestaciones**
Pruebas que simulan el uso normal de la aplicación para detectar posibles problemas de rendimiento y exceso de uso de recursos y facilitar su corrección.

**Pruebas de la Calidad del Código**
Ayudan a garantizar que la calidad del código es óptima y minimizar las posibilidades de encontrar errores o bugs.

XDebug incluye esta funcionalidad: http://xdebug.org/docs/code_coverage
[SonarQube](http://www.sonarqube.org/) es una plataforma Open Source que soporta [PHP] (http://docs.sonarqube.org/display/SONAR/PHP+Plugin) entre otros lenguajes. Aquí hay una prueba en directo de Sonar: http://nemo.sonarqube.org/dashboard/index/674819  
PhpStorm incluye [análisis del código(https://www.jetbrains.com/phpstorm/help/using-php-code-sniffer-tool.html) y admite además [PHP Code Sniffer](https://www.jetbrains.com/phpstorm/help/using-php-code-sniffer-tool.html) y [PHP Mess Detector](https://confluence.jetbrains.com/display/PhpStorm/PHP+Mess+Detector+in+PhpStorm)  

###Tests de Aceptación (Acceptance tests)

Son los que describen 'casos de uso' o escenarios desde el punto de vista del usuario.
Se ejecutan como si el usuario estuviera visitando la página desde un navegador:
 * Usan un emulador interno: PHPBrowser
 * O un navegador especial: SeleniumHQ
 
Ventajas:
 * Ayudan a definir con claridad y tener un esquema de todos los casos de uso de la aplicación.
 * Pueden ser ejecutados por personas que no sean técnicas. (Usuarios, clientes, etc..)

Más info: http://codeception.com/docs/04-AcceptanceTests
  
###Tests Funcionales (Functional tests)

No necesitan un navegador. Básicamente simulan peticiones usando las variables $_REQUEST, $_GET y $POST.

Ventajas:
 * Dan una información más detallada sobre los fallos.
 * Se ejecutan más rápido.
 
Más info: http://codeception.com/docs/05-FunctionalTests

