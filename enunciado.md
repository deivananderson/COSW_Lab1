##Escuela Colombiana de Ingenier�a ###Construcci�n de Software - COSW ###Taller - Prototipado de aplicaciones SPA

####Trabajo individual o en parejas

Parte I.

Cree un proyecto base de Sprint Boot basado en Maven, que incluya dentro de sus dependencias los elementos Web (SpringMVC, Servidor Web embebido, etc). Para esto, abra el sitio de Spring Intializer, y cree un nuevo proyecto agregando el 'starter' Web:

alt text

Genere el proyecto, desc�rguelo y descompr�malo.

Para permitir que la actualizaci�n autom�tica del contenido Web, se va a cambiar el servidor Web incluido por defecto en spring-boot Web starter (Tomcat) por Jetty. Para hacer esto, cabie esta dependencia:

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>

	por:

	```xml	
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>
En el proyecto reci�n generado, la ruta 'src/main/resources/static' corresponde al espacio donde se publicar� el contenido Web (es decir, donde residir� la SPA). Para generar un 'adamiaje' de base, desde el directorio

src/main/resources

	Clone el proyecto Angular-seed, de la siguiente manera:

	```
git clone --depth=1 --branch=master https://github.com/angular/angular-seed.git static
Rectifique que quede una estructura est�ndar de Angular.js en la ruta src/main/resources/static
Modifique el archivo bower.json para especificar las dependencias de javascript requeridas para la SPA. En este caso, s�lo se usar� angular, angular-route, y las hojas de estilo de BootStrap:

{ "name": "angular-seed", "description": "A starter project for AngularJS", "version": "0.0.0", "homepage": "https://github.com/angular/angular-seed", "license": "MIT", "private": true, "dependencies": { "angular": "^1.5.8", "angular-route": "^1.5.8", "bootstrap-css-only": "^3.3.6" } }

6. Rectifique que el archivo de configraci�n de Bower (.bowerrc) tenga definido que las dependencias de javascript se descargar�n en la ruta 'app/bower_components'.

7. En la ruta src/main/resources/static ejecute el comando de instalaci�n de dependencias de bower

	```
bower install
y rectifique que las dependencias antes configuradas se hayan descargado en app/bower_components. Igualmente, verifique que los elementos de la SPA sean accesibles desde el IDE:

![alt text](img/NetbeansSetup.png)
Rectifique que la aplicaci�n 'andamio' funcione. Para esto, ejecute:

mvn spring-boot:run


	y abra en un navegador la ruta: http://localhost:8080/app/index.html


## Parte II.

Va a adaptar el 'andamiaje' de angular-seed para crear una nueva aplicaci�n (una aplicaci�n de tareas pendientes en l�nea), la cual tendr� tres vistas parciales:

* Una vista est�tica que s�lo tendr� una descripci�n textual de la aplicaci�n (view1).
* Una vista en la cual el usuario ingresar� nuevas tareas pendientes (view2).
* Una vista en la cual el usuario consultar� las tareas pedientes registradas hasta el momento (una nueva vista parcial).

1. Para crear la tercera vista parcial, haga una copia una de las carpetas de vistas existentes (view1 o view2) y p�ngale otro nombre (tanto a la carpeta como a su contenido).
2. En el archivo javascript de la vista (el cual contiene el m�dulo que configura las reglas de enrutamiento de la aplicaci�n y su controlador) cambie el nombre del m�dulo y del controlador. Igualmente modifique la regla de enrutamiento para que coincida con el nuevo nombre y ruta del archivo html de la vista.
3. Para agregar el m�dulo antes definido a la aplicaci�n, edite el archivo /app/index.html. Dado que en dicho archivo se defienen los m�dulos javascript a ser importados, agrege una una entrada a las existentes, incluyendo el m�dulo creado anteriormente:

	```xml
  <script src="bower_components/angular/angular.js"></script>
  <script src="bower_components/angular-route/angular-route.js"></script>
  <script src="app.js"></script>
  <script src="view1/view1.js"></script>
  <script src="view2/view2.js"></script>
  <script src="components/version/version.js"></script>
  <script src="components/version/version-directive.js"></script>
  <script src="components/version/interpolate-filter.js"></script>
En el mimso archivo html, en el men� de la SPA, agregue un enlace a la nueva vista. Recuerde que en lugar de incluir una ruta absoluta, debe incluir el nombre indicado en la configuraci�n de enrutamiento de la vista (.when('/rutaxx') ... ).

view1
view2
... <-- Enlace a la nueva vista
```
Ahora, para que el m�dulo sea incluido en la aplicaci�n a nivel l�gico, edite el m�dulo de la aplicaci�n (en la ruta app/app.js) y agregue el nombre del m�dulo creado (recuerde, el nombre l�gico, no el nombre del archivo donde se cre�!).

angular.module('myApp', [ 'ngRoute', 'myApp.view1', 'myApp.view2', 'myApp.version' ]).


6. Rectifique que la aplicaici�n ya tenga las tres vistas.

##Parte III.

La funcionalidad de la vista del 'listado de tareas pendientes' es simplemente mostrar una lista de tareas que tiene las siguientes propiedades:

* Descripci�n de la tarea.
* Prioridad de 1 a 10.

1. Edite el controlador de la vista de 'tareas pendientes', e inyecte en �ste el objeto $scope:

	```javascript	
	.controller('ControladorListado', ['$scope', function($scope){         
Agregue al controlador un atributo en el que se contendr�n las tareas que se deben mostrar en la vista. Para probar el dise�o de la vista, haga que -por ahora- dicho atributo tenga un listado est�tico de tareas (en este ejemplo, cada '{}' debe reempalzarse por un objeto javascript):

	$scope.listado=[
		{},
		{},
		{}
	];


3. En la vista de 'tareas pendientes' agregue una tabla usando la directiva ng-repeat (donde todo.propiedad1 y todo.propiedad2 deben reemplazarse por los nombres de las propiedades de los objetos contenidos en $scope.listado:

	```html
    <table class="table table-striped table-hover">
      <tbody>
        <tr ng-repeat="todo in listado">
          <td>{{todo.propiedad2}}</td>
          <td>{{todo.propiedad2}}</td>
        </tr>
      </tbody>
    </table>
Verifique el funcionamiento en el navegador. Si el estilo 'table table-striped table-hover' no funciona (se deber�a mostrar la tabla en colores intercalados), significa que no se ha importado la hoja de estilos de bootstrap. Para hacerlo, revise el encabezado de la p�gina principal de la SPA (app/index.html), y rectifique que se est� usando:

``` 5. Para este ejercicio se requiere que desde la vista2, se capture la informaci�n (la tarea pendiente) que se muestra en la vista 'listado'. Para hacer esto, se va a implementar un servicio de tipo 'f�brica' que ser� inyectado a los controladores de las dos vistas. Dentro de la estructura planteada, se crear� un directorio 'services' al mismo nivel de las vistas:
```
	app\
		index.html 
		app.js 
		view1\
			controlador.js
			vista.html
		services\
		... <- definici�n de servicios comunes.

6. En la ruta antes indicada cree un archivo javascript que defina un m�dulo, y dentro de �ste, un servicio de tipo 'f�brica' (cambie los nombres NOMBREMODULO/NOMBREFABRICA a algo m�s adecuado):

	```javascript
'use strict';
angular.module('services.listFactory', ['ngRoute'])

        .factory('NOMBREFABRICA', function () {
            var data = {
                listado: []
            };
            return {
                getListado: function () {
                    return data.listado;
                },
                addTodo: function (todo) {
                    data.listado.push(todo);
                }};
        });
Para inclu�r el m�dulo anterior de la aplicaci�n, repita los pasos anteriormente descritos:

Incluir el archivo javascript en app/index.html
Incluir el m�dulo (nombrado anteriormente como 'services.XXXXXX') en el m�dulo princopal definido en app/app.js.
En el controlador de la vista2 inyecte el servicio antes creado (haga el cambio de nombres donde aplique):

.controller('View2Ctrl', ['$scope', 'NOMBREFABRICA', function ($scope, NOMBREFABRICA) { ...

9. En el mismo controlador cree:
	* Las propiedades requeridas para capturar los detalles de una 'tarea pendiente'.
	* Una funci�n que, al ser invocada, tome los valores de las dos propiedades definidas anteriormente, cree un objeto de tipo tarea pendiente, y lo agregue a la f�brica.

10. En la vista2 agregue los campos necesarios para capturar los detalles de una tarea pendiente. As�cielos con las propiedades correspondientes de su controlador con la directiva 'ng-model'.
11. Agregue un bot�n, y as�ciele la funci�n antes creada con la directiva 'ng-click'.
12. Verifique el funcionamiento de la vista2.
13. Modifique el controlador de la vista 'listado' para que en lugar de inicializarse con el conjunto de valores est�ticos, sea inicializado con el contenido de la f�brica:

	```javascript
$scope.listado=NOMBREFABRICA.getListado();             
Verifique el funcionamiento global de la aplicaci�n. Lo agregado en la vista dos, debe verse reflejado en la vista de 'listado'.
##Opcional

Revise la documentaci�n del filtro OrderBy, y modifique la vista del listado para que el cliente pueda decidir con qu� orden quiere visualizar las tareas pendientes.