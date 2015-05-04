En PHP 5.4, se introdujo una nueva sint�xis para los arrays que se ha llamado Short Array Syntax.

Consiste en no utilizar la palabra `array` ni los par�ntesis `()` y utilizar �nicamente corchetes '[]' para definir los arrays.
Por supuesto, la anterior sint�xis a�n es v�lida. Pero esta nueva forma se ha convertido en un 'est�ndar de facto'.


```
// Modo habitual de declarar y utilizar par�ntesis
$arr = array(0 => "Pruebas", 1 => "M�s Pruebas");`
$result = miFunction(array(0 => "Pruebas", 1 => "M�s Pruebas"));

// Nueva posibilidad a partir de PHP 5.4
$arr = [0 => "Pruebas", 1 => "M�s Pruebas"];`
$result = miFunction([0 => "Pruebas", 1 => "M�s Pruebas"]);



[Short Array Syntax en la documentaci�n oficial de PHP 5.4]


ToDo: http://docs.typo3.org/flow/TYPO3FlowDocumentation/_downloads/TYPO3_Flow_Coding_Guidelines_on_one_page.pdf

http://www.slideshare.net/nperriault/30-symfony-best-practices
http://www.slideshare.net/nquocbao/symfony-2-best-practices?next_slideshow=1
http://framework.zend.com/manual/1.12/en/coding-standard.coding-style.html

[
dd
[dd