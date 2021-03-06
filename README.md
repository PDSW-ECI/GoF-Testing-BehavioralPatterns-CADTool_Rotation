### Escuela Colombiana de Ingenieria
### Procesos de desarrollo de Software - PDSW


__Taller - diseño de pruebas: clases de equivalencia, condiciones de frontera, análisis de transiciones.__

__Importante: Las instrucciones de este taller asumen el uso de Linux__

Entregables: 

* Parte I, en clase.
* Parte II (clases de equivalenci y diseño). El martes clase.
* Parrte II (implementado). Antes del próximo laboratorio.

### Parte I

0. Revise las [especificaciones del ejercicio básico de diseño de pruebas](https://docs.google.com/document/d/1ThdKgqam1lhJmiNrOlszQg3uEr_65Yw4suOeW2vFDrw).

1. Descargue e importe el proyecto de base del primer punto.

	```bash
	git clone https://github.com/PDSW-ECI/EquivalenceClassesPropertyTestingExcercise
	```

2. Revise cómo es la estructura de Maven para separar las pruebas de los demás artefactos del proyecto: src/main, src/test.
3. Ejecute la fase de pruebas del proyecto con el comando, y analice cómo son presentados los resultados.

	```
	mvn test
	```
	
4. Implemente un caso de prueba para cada clase de equivalencia identificado, siguiendo el esquema PBT (property-based testing) a través de JUnit y QuickTheories. Recuerde usar la anotacion @Test y seguir la conveción de nombres: 

	```java
	/**
	* Pruebas clase de equivalencia XX: [Aquí la especificación de la clase de equivalencia]
	**/
	@Test
	public void testClaseEquivalenciaXX(){
		...
	}
	```

5. Ejecute las pruebas. A partir de los resultados obtenidos, identifique los posibles problemas que tenga la implementación.


### Parte II.
#### Parte A.


En este ejercicio, va a agregar un par de requerimientos funcionales a una herramienta de dibujo tipo CAD, siguiendo un esquema TDD (Test-Driven development). En la evaluación se revisará que efectivamente se hayan hecho las pruebas antes de la implementación.

1. Clone los fuentes de ESTE repositorio con git, NO lo descargue directamente de la página!).

2. Importe el proyecto y revise su funcionalidad. Como observará, la opción 'rotar figura seleccionada' no está implementada aún. Esta función permite rotar la figura seleccionada actualmente en la lista de figuras, usando como eje de rotación la esquina inferior izquierda del rectángulo que enmarque dicha figura (por ahora sólo se pueden dibujar líneas y rectángulos).

Por ejemplo, si la siguiente fuera la figura seleccionada:
 
![](img/rot1.png)

Al seleccionar la opción de rotación se haría:
	
![](img/rot2.png)

Quedando al final:

![](img/rot3.png)

Revise la especificación del método 'rotateSelectedShape' y a partir del mismo haga lo siguiente:
	
* Defina las clases de equivalencia para las posibles entradas de este método. Ponga el detalle de estas clases, a manera de comentarios en la clase de pruebas ControllerTest, incluyendo:
	* La descripción del conjunto, usando lenguaje natural o lenguaje matemático.
	* El tipo de prueba 	
	* El resultado esperado para dicha clase de equivalencia.
	
	Ejemplo:
	
	| #CE	| Método        | Clase de equivalencia         | Resultado |
	| ---	| ------------- |:-------------| --- |
	| 1| Controller.rotateSelectedShape      | Segmento Horizontal: La instancia del controlador tiene un segmento de recta r tal que r.punto1.y==r.punto2.y ^ r.punto1.x != r.punto1.x |  ????? |
	| 2| Controller.rotateSelectedShape      | ????  | ???? |
	|3	| Controller.rotateSelectedShape      |????  | ????| 
	|....	| .....      |..... |  ...... | .....| 	
	|N	| Controller.rotateSelectedShape      |????  | ????| 

	
	

* Seleccione un caso por cada clase de equivalencia e implemente las respectivas pruebas en ControllerTest, siguiendo el esquema PBT y QuickTheories.

* Cuando haya hecho lo anterior, ejecute:
	
```bash		
git add .			
git commit -m "primera versión de las pruebas"
			
```		
		
* Haga la implementación del método 'rotateSelectedShape', y apoyese en las pruebas (mvn test), para verificar la funcionalidad del mismo.
* Una vez tenga la funcionalidad deseada, realice las pruebas de cubrimiento para rectificar que las pruebas están contemplando todos los caminos/condiciones del método implementado.
* Una vez se tenga la funcionalidad implementada, haga un nuevo commit:
	
```bash		
git add .			
git commit -m "funcionalidad de rotación implementada"
			
```		

#### Parte B.

Otra funcionalidad faltante es la opción de 'deshacer' / 'rehacer'. Para esto, aplique el patrón Comando (ver referencia dada en la programación de lecturas).

Tenga en cuenta que para lograr esta funcionalidad se requiere:

1. Encapsular en 'Comandos' las funcionalidades de:

	* Dibujar una figura
	* Duplicar
	* Rotar la figura seleccionada

2. Embeber en dichos comandos las respectivas operaciones inversas. Por ejemplo, el inverso de dibujar una figura es eliminarla, mientras que el inverso de duplicar, será borrar todas las figuras adicionales creadas.

3. Mantener, con un esquema de pilas, tanto la secuencia de comandos ejecutada, como la secuencia de comandos 'deshecha', de manera que las operaciones de 'deshacer' y 'rehacer' se hagan en un orden lógico.


Nota: Para comprimir el avance en un archivo .zip, use el comando (dentro del directorio que va a comprimir, sin olvidar el punto):


```bash	
	zip -r NOMBRE.PROYECTO.zip .	
```			

<!-- ### Criterios de evaluación

Parte I.

* Se modificó el código hecho pro el 'programador poco confiable', corrigiendo el error existente en el mismo.

Parte II.

* Proceso:
	* Se especificaron clases de equivalencia y condiciones de frontera para las pruebas del método 'rotar', a partir de la especificación (dicha especificación debe estar como comentarios en la clase que implementa las pruebas).
	* Las clases de equivalencia deben describir conjuntos NO UNITARIOS, es decir NO deben hacer referencia a conjuntos de valores concretos (estos serían casos de prueba, no clases de equivalencia).
	* Se describen condiciones de frontera (igualmente, sin hacer referencia a valores concretos).
	* Se implementaron casos de prueba acordes con las clases de equivalencia.
	* Al revisar los LOGs de GIT, se evidencia que primero se hicieron las pruebas, y luego la implementación del código.
	
* Diseño:
	* Se creó un conjunto de comandos que encapsulan la funcionalidad de dibujar, duplicar y rotar. Cada comando incluye un método con la operación inversa, de manera que con ésta se puedan deshacer las operaciones.
	* La aplicación lleva el historial de los comandos ejecutados, de manera que se puedan realizar las operaciones de deshacer/rehacer consistentemente. Nota: el incluír pilas dentro de los comandos (y hacer el apilar/desapilar dentro de la ejecución de los mismos), se evaluará como R, pues esto acopla dichos comandos a la funcionalidad de deshacer/rehacer.
	* En el esquema de deshacer/rehacer se debe tener en cuenta que si después de deshacer una acción, se crea una nueva acción (un nuevo comando), el 'rehacer' debe quedar invalidado.		

* Funcionalidad:
	* La aplicación permite dibujar, duplicar y hacer espejo de manera consistente.
	* Las operaciones se pueden deshacer/rehacer.-->


<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />Este contenido hace parte del curso Procesos de Desarrollo de Software del programa de Ingeniería de Sistemas de la Escuela Colombiana de Ingeniería, y está licenciado como <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.
