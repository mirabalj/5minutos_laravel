###Estructura switch

rimero tienes que definir lo que cambia de lo que no cambia. Que parte espec�fica de tu c�digo est�s duplicando? Luego analizar las opciones. Ese c�digo est� en un controlador y claramente son muchas lineas. Puedes extraer la l�gica que se duplica y ponerla en un m�todo del modelo, puedes extraerla a un evento, puedes extraerla a un servicio aparte (que es b�sicamente otra clase que vas a llamar, servicio es un nombre m�s bonito).
Lo otro es el switch que tienes ah�. Si lo ocupas s�lo en este controlador no es tan problem�tico, pero si lo est�s utilizando en m�s partes es peligroso ya que si quieres a�adir un nuevo condicional tendr�s que agregarlo en todos los switch esparcidos en tu aplicaci�n y hay probabilidades altas de introducir un bug. Para refactorizar un switch generalmente hay 2 opciones, usar un diccionario, o usar polimorfismo. La primera es m�s r�pida y la segunda es m�s elegante y m�s orientada a objetos. Por ejemplo con un diccionario (array asociativo en php) podr�as crear una variable que sea algo as�:
$perfiles = ['Coordinador Proyectoovspop' => 'todos', 'Tematicoovspop' => 'uno', ....] //Todos los cases del switch van aqui
Y luego en vez de un switch s�lo haces
$submenuObservatorios = $perfiles[$perfilObservatorio]
Como vez es simple pero puede ser inflexible en muchos casos.
Por otro lado si usas polimorfismo, tendr�as que crear clases para cada uno de los cases, crear un m�todo en com�n que devuelva el valor que quieres. Por ejemplo: creas las siguientes clases:
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
El segundo enfoque quiz�s te parezca abrumador por que habr�a que crear una clase para cada opci�n del switch, pero crear clases es barato y no es una mala pr�ctica en OO tener MUCHAS clases. Si tienes alguna duda pregunta nomas.
Saludos!

--------------------
http://refactoring.com/catalog/extractMethod.html
