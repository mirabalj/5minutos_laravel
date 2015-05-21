###Resumen sobre DDD. Domain Driven Design.

Resumen para introducirse en el desarrollo orientado a dominios. Esta es una interpretaci�n personal de la informaci�n que he le�do en varios lugares. Una de mis mayores fuentes de informaci�n ha sido el libro Domain Driven Design Quickly.

La mejor forma de entender los conceptos de este esquema de desarrollo, es olvidar por un momento todo lo que conoces sobre programaci�n y pensar en el mundo real. La OOP ha sido abstra�da del mundo real y, por tanto, es una representaci�n de este. Del mismo modo, el DDD es una abstracci�n de c�mo hemos estructurado el mundo real y buscar similitudes entre ambos te facilitar� la tarea de entenderlo. 

Dado que actualmente estoy desarrollando usando el Framework Laravel, he incluido algunos ejemplos relacionados con este Framework pero son perfectamente aplicables a cualquier otro framework/lenguaje de programaci�n.

DDD no define implementaciones, define conceptos y 'reglas de implementaci�n'. Por tanto es independiente de cualquier framework y lenguaje.

>**Nota:** Esta entrada contin�a en desarrollo.

####Lenguaje Ubicuo: Ubiquitous Language

El lenguaje ubicuo es un t�rmino que introdujo Eric Evans en su libro sobre DDD (__Domain Driven Design__) como propuesta para crear un lenguaje com�n entre los programadores y los usuarios.

La definici�n propone nombrar las variables, m�todos y clases con lenguaje del dominio de modo que sea 'autoexplicable'.

Con este tipo de nombres, el c�digo se documenta por s� mismo:
- `getRandomEntityFrom($modelName)` devuelve una entidad aleatoria a partir del nombre de un modelo.
- Comprueba si el modelo existe en la colecci�n antes de acceder a �l.
- `createMultipleEntities($numberOfEntities)` una cantidad de entidades determinada por el par�metro que recibe.
- Para ello usa un bucle en el que se crea cada vez una nueva entidad a partir del contador actual.

Habitualmente los nombres representan objetos y los verbos m�todos.

####Capas de la arquitectura

Se proponen cuatro capas conceptuales:

- Interface de usuario (User Interface)

 Responsable de presentar la informaci�n al usuario, interpretar sus acciones y enviarlas a la aplicaci�n.
 
- Aplicaci�n (Application)

 Responsable de coordinar todos los elementos de la aplicaci�n. No contiene l�gica de negocio ni mantiene el estado de los objetos de negocio. Es responsable de mantener el estado de la aplicaci�n y del flujo de esta.
 
- Dominio (Domain)

 Contiene la informaci�n sobre el Dominio. Es el n�cleo de la parte de la aplicaci�n que contiene las reglas de negocio. Es responsable de mantener el estado de los objetos de negocio. (La persistencia de estos objetos se delega en la capa de infraestructura.
 
- Infraestructura (Infraestructure)

 Esta capa es la capa de soporte para el resto de capas. Provee la comunicaci�n entre las otras capas, implementa la persistencia de los objetos de negocio y las librer�as de soporte para las otras capas (Interface, Comunicaci�n, Almacenamiento, etc..)
 
Dado que son capas conceptuales, su implementaci�n puede ser muy variada y en una misma aplicaci�n, tendremos partes o componentes que formen parte de cada una de estas capas. Por ejemplo, en una aplicaci�n web desarrollada con Laravel, Las vistas formar�an parte de la capa de Interface, pero Sass o Less, por ejemplo, ser�an parte de la infraestructura. Algunos componentes del Framework formar�an parte de la infraestructura (Eloquent, Caches, etc...) y otros componentes formar�an parte de la aplicaci�n (Controladores, Comandos, Eventos, etc..). Los modelos, por ejemplo, formar�an parte de la capa de Dominio.  

####Entidades (Entities).

Cualquier objeto del dominio que mantiene un estado y comportamiento m�s all� de la ejecuci�n de la aplicaci�n y que necesita ser distinguido de otro que tenga las mismas propiedades y comportamientos, es una Entidad. A cada instancia, por tanto, se le debe asignar un identificador �nico. 

Por tanto, simplificando, podr�amos decir que podemos definir como identidad a todos los objetos de la aplicaci�n que tengan un identificador �nico.

Qu� se considere entidad depender� principalmente de la funci�n, objetivo y contexto de la aplicaci�n. Lo que para una aplicaci�n puede ser una entidad, para otra podr�a no serlo. Habitualmente las entidades ser�n objetos con identidad propia en el mundo real.

Por ejemplo, en el mundo real, dos personas con el mismo nombre, edad y caracter�sticas, son dos personas distintas y por ello se invent� el Documento de Identidad, o DNI, Pasaporte, etc...

Para m�, los �rboles no representan entidades, dado que no podr�a reconocer uno de otro. Pero para un jardinero encargado de catalogar todos los �rboles de un parque, si la tienen y por ello su primera funci�n ser� crear un m�todo de clave �nica para identificar cada �rbol.

Los coches tienen matr�cula y, por tanto se puede realizar seguimiento de por vida de cada coche. Incluso los motores tienen un n�mero �nico. Sin embargo, las alfombrillas de un coche o los espejos retrovisores no las tienen. Son meros 'objetos' que para m�, no llegan a convertirse en Entidades. Sin embargo, en una f�brica es posible que les hagan seguimiento individual y tengan asignado un n�mero de serie interno, convirti�ndolos en entidades.

Por tanto, Entidad es un t�rmino conceptual cuya concreci�n a la hora de definir en nuestra aplicaci�n qu� objetos o clases se tratar�n como tales, depender� del contexto y los objetivos de esta.

Durante el desarrollo de un aplicaci�n, al avanzar el conocimiento profundo del modelo de negocio, habr� objetos que puedan pasar a convertirse en entidades y, tambi�n ser�a posible lo contrario.

En una aplicaci�n habitual de gesti�n de usuarios, el Pa�s es una mera etiqueta, por lo que no llega apenas a ser un objeto. Sin embargo, si aplicamos gastos de env�o por pa�s, empieza a convertirse en un objeto y si queremos hacer un seguimiento de los pa�ses por los que pasa el env�o, se convertir�a en Entidad. En una aplicaci�n demogr�fica con encuestas globales, ocurre lo contrario, el Pa�s es una entidad y el usuario puede llegar a convertirse en un simple n�mero.

Pueden contener otras entidades y objetos de valor.

> Habitualmente, los nombres que aparezcan en las reglas de negocio se convertir�n en entidades o en objetos de valor.

####Objetos de valor (Value Objects).

Cuando un objeto no llega a ser entidad, se le considera un objeto de valor. Los objetos de valor dentro de una aplicaci�n son indistinguibles unos de otros. Consisten �nicamente en un conjunto de propiedades y comportamientos pero no mantienen identidad alguna. S� es posible que mantengan un estado, pero este se pierde al terminar la aplicaci�n.

Se pueden duplicar y destruir con facilidad. Y no deber�an ser modificados una vez han sido creados. 

Pueden proporcionarse a capas fuera del dominio, dado que su posible modificaci�n no afectar�a a la aplicaci�n.

Pueden contener a su vez otros objetos de valor.

En ocasiones es posible agrupar varios atributos en un objeto de valor.

Ejemplos: 

- Direcci�n de un usuario podr�a ser un objeto de valor que contenga: Calle, Ciudad, Estado, Pa�s. A su vez, la calle podr�a ser un objeto de valor que contenga: Nombre de la calle, n�mero del portal, piso, escalera, letra, etc..

- Un n�mero de tel�fono podr�a componerse de prefijo internacional y n�mero. (Y compa��a telef�nica)

####Servicios (Services).

La mayor�a de los verbos del lenguaje ubicuo se convertir�n en m�todos de los objetos de la capa de negocios. Pero hay verbos o comportamientos que no es f�cil concretar a cual de los objetos corresponden. Esos comportamientos suelen ser en realidad Servicios.

Volviendo al ejemplo del mundo real, la reparaci�n del autom�vil, �A qui�n corresponde?, �Al usuario/conductor o al veh�culo? Cuando env�o un paquete postal, �Qui�n hace el env�o? �El remitente o el destinatario? En realidad, son 'servicios' que contratamos. Por que no los 'implementamos' nosotros, simplemente los usamos. Y no forman parte de nuestra vida ni entorno personal. De hecho, es posible que en cada ocasi�n que tengamos que reparar el coche, utilicemos un taller distinto. O que elijamos el modo y la empresa para enviar el paquete postal en funci�n de las necesidades concretas de cada env�o.

Sin embargo, si soy el due�o de un taller de veh�culos o una empresa de mensajer�a, esos servicios son el n�cleo de mi aplicaci�n y de mi modelo de negocio. 

En el mundo de la programaci�n sucede exactamente lo mismo. Cuando encuentres comportamientos que no 'pertenecen' a ninguna entidad porque simplemente son 'utilizados' por estas, convi�rtelos en servicios.

Habitualmente los servicios actuar�n como interface o medio de relacionarse de varios objetos. Podemos tener servicios en cualquiera de las capas de la aplicaci�n.

En Laravel, los controladores son claros ejemplos de servicios. As� como la gesti�n de la cach�, de la configuraci�n de la aplicaci�n, el env�o de correos, etc..

Algunas claves para distinguirlos:

- Son comportamientos que no pertenecen a ninguna entidad.
- Pueden intercambiarse con facilidad por servicios de similares caracter�sticas.
- Van a ser usados por varias entidades en distintos puntos de la aplicaci�n.
- Nos interesa m�s el resultado del proceso que el proceso en s�.
- Desde nuestro punto de vista, no tienen un estado interno (o nos es desconocido y no nos interesa).

> Habitualmente, los verbos que aparezcan en las reglas de negocio se convertir�n en servicios o en m�todos de las entidades u objetos de valor.

####M�dulos (Modules).

Los m�dulos permiten seguir con facilidad el concepto de 'acoplamientos d�biles y alta cohesi�n'. Cuando una aplicaci�n comienza a ser compleja, es preferible dividirla en m�dulos.

En la vida real puedes ver esa divisi�n en las carpetas de tu disco duro. Empiezas con una carpeta llamada fotos y al cabo de un tiempo, organizas tus fotos por a�os, por eventos, o por ambos al mismo tiempo. Tambi�n puedes ver m�dulos en los departamentos de una empresa en comparaci�n con un freelance o una startup de cuatro o cinco empleados. O en la divisi�n de los espacios en una casa de 20 o 30 metros cuadrados y de un chalet de 400 metros cuadrados y cuatro plantas. (Como el m�o (en sue�os, claro), jajaja!)

En el mundo de la programaci�n, las bases de datos son m�dulos que contienen a su vez tablas relacionadas entre s� de alg�n modo concreto. Los paquetes que instalas en Laravel, son m�dulos independientes entre s� pero que pueden interactuar entre ellos. 'Componente' es un buen sin�nimo de 'M�dulo' y Laravel contiene muchos componentes: 'Pagination', 'FileSystem', 'Encryption', etc...

Organiza tus m�dulos agrupando clases o bien por que se comunican entre ellas o, preferiblemente, o porque est�n relacionadas a nivel funcional.

Dado que los m�dulos suelen contener varios objetos, es preferible implementar un interface para que los dem�s m�dulos no accedan directamente a los objetos.

Si un m�dulo comienza a volverse complejo durante el desarrollo, considera refactorizarlo y convertirlo a su vez en varios m�dulos m�s peque�os.

####Agregados (Aggregates).

Mientras los m�dulos suelen estar formados de clases relacionadas con los servicios o la funcionalidad de la aplicaci�n, los agregados son grupos de entidades relacionadas entre s� a nivel de negocio.

Cuando tenemos varias entidades que est�n relacionadas entre s� y son dependientes entre ellas, las agrupamos en Agregados. 

En un agregado, se define cual va a ser la entidad ra�z ('root') o principal y se dar� acceso p�blico �nicamente a esta. De modo que las entidades externas s�lo podr�n acceder a las entidades internas a trav�s de la entidad ra�z.

Por ejemplo, todos los objetos que tengo en casa son dependientes de m�, y nadie tiene acceso p�blico a ellos. Si alguien quiere usar mi ordenador, o que le preste un libro, tiene que ped�rmelo a m�. Formamos un agregado y yo soy la entidad ra�z. Sin embargo, en algunas bibliotecas, los libros son entidades p�blicas por s� mismas y en otras, el acceso es a trav�s del bibliotecario.

En el ejemplo del coche, no es l�gico que cualquiera pueda utilizar o modificar mi coche o el motor de este. Lo habitual es que accedan a �l a trav�s de m�.

En las aplicaciones desarrolladas en Laravel, no accedemos al fichero de configuraci�n de la aplicaci�n directamente, ni a los ficheros f�sicos de las plantillas, lo hacemos a trav�s de los m�todos correspondientes implementados en la clase Application y View. Estos son ejemplos claros de 'acceso controlado' en m�dulos o paquetes. A nivel de negocio, en una aplicaci�n que gestione una biblioteca, si tenemos un agregado con la clase Usuario y una colecci�n de los libros que el usuario tienen en pr�stamo, no ser�a l�gico que la clase Biblioteca acceda directamente a esos libros. Lo l�gico es que use el m�todo `$user->getBooksOnLoan()` en lugar de `$user->collectionBooksOnLoan`.

En el ejemplo anterior de la direcci�n del Usuario, tendr�amos un agregado que incluir�a la clase Usuario, la clase Direcci�n y la clase Portal. Si el interface permite que el usuario modifique su direcci�n, la implementaci�n correcta en este caso ser�a: `$user->updateAddress($newAddress)`.

####Factor�as (Factories).

Cuando la creaci�n de una entidad o un agregado se convierte en un proceso complejo, delegamos esa responsabilidad en una Factor�a. Las factor�as son clases encargadas de crear entidades o agregados, construyendo todas las entidades contenidas en ellos. Son �tiles especialmente cuando las reglas de creaci�n de estas entidades o agregados son complejas.

Las Factor�as permiten abstraer y separar la l�gica y reglas de creaci�n de una entidad y dejar en las entidades �nicamente las reglas de negocio que son inherentes a ellas.

De este modo, cumplimos con el Principio de Simple Responsabilidad delegando la creaci�n de la entidad fuera de esta y dej�ndole �nicamente aquellas responsabilidades que fon parte fundamental de sus reglas de negocio.

Es importante que el proceso de creaci�n sea at�mico y que se lance una excepci�n en el caso de que haya un error durante el proceso en lugar de devolver un objeto err�neo o incompleto.

En el mundo real, solemos ir a una panader�a a comprar el pan en lugar de hacerlo nosotros. Y, a veces, las propias panader�as son meras distribuidoras que compran el pan a un horno de pan. Si somos freelances especializados en el Backend y nos piden una p�gina web sencilla, podemos hacer nosotros mismos el html de la p�gina, pero si es compleja, lo solemos encargar a una 'Factor�a' autom�tica como 'WordPress' o a un dise�ador web. Es posible que el dise�ador web act�e como 'Base de datos' y sea el responsable de la persistencia y de 'recrear' el dise�o cada vez que sea necesario. En ese caso, se quedar� con el fichero de Photoshop y nos entregar� �nicamente los ficheros html y gr�ficos jpeg. De ese modo, cada vez que necesitemos una modificaci�n, �l actuar� de nuevo como Factor�a.

Hay varios patrones para implementar una Factor�a. Un ejemplo en Laravel, es `Illuminate\Foundation\Application` que act�a como Factor�a y Contenedor. Un modelo Eloquent tambi�n act�a como factor�a, abstrayendo la complejidad de crear un modelo a partir de tablas que est�n relacionadas entre s�.

Es posible que una Factor�a utilice a su vez varias factor�as para crear las entidades necesarias.

Por otro lado, hay que tener en cuenta que no es lo mismo crear un objeto desde cero que recuperarlo desde alg�n mecanismo de persistencia y reconstruirlo. En funci�n de la implementaci�n, este proceso puede realizarse un una �nica Factor�a o en una para la creaci�n y en otra para la reconstrucci�n y recuperaci�n desde el mecanismo de persistencia.

Tambi�n es importante tener en cuenta que no es igual la complejidad de una Factor�a de Objetos de Valor que de Entidades.

Si el proceso de creaci�n no es complejo, es preferible utilizar en su lugar un constructor:

- Si el proceso no es complejo.
- La creaci�n de un objeto no involucra la creaci�n de otros y todos los atributos necesarios se le facilitan en el constructor.
- El proceso de implementaci�n forma parte de las reglas de negocio y responsabilidades de la Entidad o del Cliente, ya que quieren elegir la Estrategia de implementaci�n. En este caso, usar�amos el patr�n Estrategia (Strategy) en lugar del patr�n Factor�a.
- La clase es sencilla y no hay herencia.

####Repositorios (Repository).

En una aplicaci�n desarrollada siguiendo los conceptos de la OOP, la aplicaci�n necesita mantener un puntero a cada objeto o entidad creadas. El Repositorio es el responsable de proporcionar a la aplicaci�n esos punteros. De ese modo, es el Repositorio quien conoce qu� mecanismo se est� utilizando para implementar la persistencia. En algunas implementaciones el repositorio actuar� tambi�n como 'Factor�a', mientras en otras har� de intermediario entre estas y la capa de negocio de la aplicaci�n.

A nivel global, el Repositorio act�a como la capa de persistencia de la aplicaci�n. Sin embargo, este a su vez, puede utilizar otros servicios que implementen ese tipo de persistencia y utilizar uno u otro en funci�n de una estrategia. Por ejemplo, utilizar una Cach� o acceder a la Base de Datos si el objeto solicitado no est� en la Cach�.

Implementa repositorios para acceder �nicamente a las Entidades Ra�z de los Agregados. Un Repositorio puede contener interfaces con m�todos que permitan solicitar un puntero o instancia de una Entidad, de varias, utilizando filtros si es necesario y proporcionando a los m�todos habituales de persistencia tales como inserci�n, modificaci�n y borrado. Pero su implementaci�n deber�a ser sencilla y actuar como intermediario de otros Servicios o Factor�as que implementen la complejidad de esos procesos.

Habitualmente, el interface de un Repositorio ser� similar al del sistema de persistencia utilizado. Por eso, en ocasiones, un Repositorio estar� 'fuertemente acoplado' a los sistemas de persistencia y almacenamiento utilizados. Permitiendo de ese modo que la capa de negocio y la aplicaci�n, sin embargo, est�n 'd�bilmente acopladas' a esos sistemas.

Aunque un Repositorio puede ser visto como una Factor�a, cuando se le solicite una entidad que no exista en el sistema de persistencia, deber�a delegar la responsabilidad de crear la nueva entidad a una Factor�a y devolver la instancia o el objeto que reciba de esta.


####Fuentes:

[Libro gratuito: Domain Driven Design Quickly](http://www.infoq.com/minibooks/domain-driven-design-quickly)