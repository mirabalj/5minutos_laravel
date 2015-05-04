###Modelos

Los modelos contienen las reglas de dominio?

Representan objetos del mundo real: Personas, Cuentas Bancarias, Productos, etc...

Suelen ser permanentes y almacenados fuera de la aplicación (Ficheros, BD, etc..)

En los modelos están:

- Las validaciones y reglas de negocio


Actúan como **controladores de acceso** y como **almacenadores de datos**.

###Vistas

La representación visual de un modelo en un contexto concreto.

Es el HTML que se envía al navegador.

Responsabilidades:

- Generar un interface de usuario.
- Solicitar datos al modelo.
- Formatear los datos recibidos del modelo.
- Mostrar los datos al usuario.
- 
- 
###Controladores

Los que establecen la comunicación entre los Modelos y las Vistas.

Responsabilidades:

- Procesar las entradas de datos.
- Enviar las entradas al modelo.
- Recibir la respuesta del modelo.
- Decidir la acción a realizar: Reenviar a otra url, Mostrar una vista, etc...

###Modelo de Datos

Es la Base de Datos o el sistema para almacenar esos datos.
Las relaciones definidas en la BD.
Los índices.
Los tipos de datos.

En Laravel, accedemos a ellos con Eloquent.

###Flujo en una aplicación Laravel:

1. El usuario interactua con el interface de la aplicación.  
2. El Navegador envía una petición. (Request)
3. El servidor web recibe la petición. 
4. El servidor web pasa la petición al sistema de Rutas de Laravel.
5. El sistema de Rutas de Laravel recibe la petición (url) y la procesa.  
6. El sistema de Rutas, redirige la petición al Controlador adecuado.  

 Caso sencillo:  
 
 6.1. El Controlador genera una Vista.
 
 Caso complejo:
 
 6.1. El Controlador solicita llama a un método de un Modelo.
 6.2. El Modelo, envía una petición a la Base de Datos. (SQL)
 6.2. La Base de Datos devuelve los datos al Modelo.
 6.3. El Modelo, envía una respuesta al Controlador. 
 6.4. El Controlador genera una vista.

7. El Controlado Envía el HTML al Navegador Web.
8. El Navegador Web muestra el HTML al Usuario.

###Diagrama del Flujo:

```
User -> ::(Interface):: -> Web Browser -> ::(Request):: -> Web Server ->  
-> Web Server -> ::(Pass Url):: -> Laravel Routes -> ::(Laravel Routing):: -> Controller ->  
-> Controller -> ::(Call Method):: -> Model -> ::(Exec SQL):: -> DataBase ->   
-> DataBase -> ::(Return Data):: -> Model -> ::(Method Return):: -> Controller ->   
-> Controller -> ::(Renders):: ->  View -> ::(Return HTML):: -> Controller ->   
 -> Controller -> ::(Return HTML):: -> Web Browser -> ::(Render HTML):: -> User


##Fuente: 

http://laravelbook.com/laravel-architecture/