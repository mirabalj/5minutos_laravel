La mayor�a de los paquetes, tendr�n un ServiceProvider que ser� el encargado de inicializar y configurar el paquete: Registrar los objetos que contiene el paquete, establecer las opciones de configuraci�n necesarias y realizar todo aquello que se necesite para utilizar el paquete.

Una vez instalamos el paquete, hay que configurar su ServiceProvider en el fichero `config/app.php` en el array `providers` y si contiene alguna `Facade` que queramos utilizar, podemos asignarle un alias en el array `aliases`.

