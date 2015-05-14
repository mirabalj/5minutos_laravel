###Estructura switch

rimero tienes que definir lo que cambia de lo que no cambia. Que parte específica de tu código estás duplicando? Luego analizar las opciones. Ese código está en un controlador y claramente son muchas lineas. Puedes extraer la lógica que se duplica y ponerla en un método del modelo, puedes extraerla a un evento, puedes extraerla a un servicio aparte (que es básicamente otra clase que vas a llamar, servicio es un nombre más bonito).
Lo otro es el switch que tienes ahí. Si lo ocupas sólo en este controlador no es tan problemático, pero si lo estás utilizando en más partes es peligroso ya que si quieres añadir un nuevo condicional tendrás que agregarlo en todos los switch esparcidos en tu aplicación y hay probabilidades altas de introducir un bug. Para refactorizar un switch generalmente hay 2 opciones, usar un diccionario, o usar polimorfismo. La primera es más rápida y la segunda es más elegante y más orientada a objetos. Por ejemplo con un diccionario (array asociativo en php) podrías crear una variable que sea algo así:
$perfiles = ['Coordinador Proyectoovspop' => 'todos', 'Tematicoovspop' => 'uno', ....] //Todos los cases del switch van aqui
Y luego en vez de un switch sólo haces
$submenuObservatorios = $perfiles[$perfilObservatorio]
Como vez es simple pero puede ser inflexible en muchos casos.
Por otro lado si usas polimorfismo, tendrías que crear clases para cada uno de los cases, crear un método en común que devuelva el valor que quieres. Por ejemplo: creas las siguientes clases:
class CoordinadorProyectoovspop{
  public function getSubMenuObservatorios(){
  return 'todos';
  }
}

class Tematicoovspop{
  public function getSubMenuObservatorios(){
    return 'uno';
  }
}
Luego en vez del switch usas:
$perfilObservatorio = $perfil.$observatorio->alias;
$perfilClass = new $perfilObservatorio; //te creara una de las clases que declaraste arriba

$submenuObservatorios = $perfilClass->getSubMenuObservatorios(); 
El segundo enfoque quizás te parezca abrumador por que habría que crear una clase para cada opción del switch, pero crear clases es barato y no es una mala práctica en OO tener MUCHAS clases. Si tienes alguna duda pregunta nomas.
Saludos!

--------------------
http://refactoring.com/catalog/extractMethod.html
