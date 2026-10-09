# Trabajo-clases actividad SRP
[Actividades_ relaciones a partir del enunciado.pdf](https://github.com/user-attachments/files/33228886/Actividades_.relaciones.a.partir.del.enunciado.pdf)


Por si falla el link dejo las respuestas:

Actividad 1: relaciones a partir del enunciado “biblioteca”

1. ¿Qué relaciones deberían incorporarse al diagrama? Indique las 
clases involucradas, el tipo de relación y justifique su elección.

Préstamos () necesita a un Socio + Libro, tipo asociación 

Préstamo usa temporalmente a impresora osea tenemos una dependencia de uso

Al Devolver libro, préstamo “Marcado como devuelto”----> si libro no es devuelto Multa Multa = 500 x cada dia atrasado —--> Sin atraso Multa=0

Préstamos (no guarda impresora) depende de la información brindad por Préstamo, no creo que imprima un Préstamo vacío si no hay socio ni libro registrado 

2. Para cada asociación propuesta, indique sus multiplicidades y explique qué significan en el caso.

Para este caso en particular Préstamo depende de Libro y socio, como mencione anteriormente, ya que para validar un préstamo se necesita tener un libro y el libro es solicitado por el socio, otra asociación sería la de Préstamos con la Impresora esta dependencia sería para la impresora esta misma no puede imprimir nada si no recibe la info que necesita (suponiendo que imprime el socio + libro devuelto o no, multa si o no) 
Donde Préstamo y socio con una multiplicidad de 1 a 1 (recibe-guarda un socio)
Préstamo + libro  con una multiplicidad de 1 a 1 (recibe un libro determinado)



3. ¿Bastaría representar todas las relaciones como dependencias para describir el caso? Justifique con el enunciado.

No es conveniente utilizar al 100% la dependencia temporal, esto se debe a que nos están diciendo en el enunciado que “Al devolver el libro, el socio y el libro siguen registrados. El préstamo se conserva en el historial.” estos nos lleva a requerir una asociación entre Préstamos <-socio-libro, requieren ser información fija o referencia establecidas para crear préstamos no solo viajar como parámetros, sino que necesitan “vivir dentro del objeto” ya sea para un historial o para la dependencia de la impresora 

4. ¿Qué objetos debería guardar Préstamo como atributos? Justifique
Un socio, libró como private, los cuales serán “modificados con setter” según cada caso, al tener definido los atributos e incluso un mismo constructor con socio + libró, con esto tenemos el molde ideal para generar los préstamos adecuados a cada caso de libros solicitados 


5. ¿Qué parámetros y tipos propondría para devolver? Explique para qué sirve cada uno.

Refiriéndose a la acción de devolver el libro (ya nos dicen que guardo al socio y al libro), requerimos saber si tenemos o no multa con tipo int como “diasAtraso”, para poder calcular según los días de este, por otro lado necesitamos el uso de “Impresora” podemos pasarlo igual como un parametro para ejecutar su respectiva acción sabiendo que ahora tenemos diasAtraso + libro + socio,


Actividad 2:relaciones en el código


1. ¿Dónde se reflejan las asociaciones y dependencias en el código? Señale los atributos o métodos, las clases involucradas y justifique cada relación.

Código 1: Clase Prestamo
Las asociaciones parte de los atributos “private Socio socio; private Libro libro” ya que se está definiendo fijamente las clases Socio y Libro en el atributo siendo una instancia de estas dos clases

Dependencias apreciadas en los parámetros requeridos por “public void devolver(int diasAtraso, Impresora impresora)” osea está solicitando información para poder ejecutar sus tareas en el instante no se mantiene activa.

Código 2: Clases Socio, Libro e Impresora
Para la class Socio, No presenta asociación, dependencia puede ser pero desde otra clase que requiera usar temporalmente algún nombre para efectuar alguna tarea o registro, osea que seria una clase “independiente”

En la clase de Libro, lo mismo que la class Socio 

Por último la clase impresora en su método está esperando un parámetro pero este no lo contaría ya que es directamente un dato String que no es una clase de este caso

2. ¿Cuántas asociaciones y dependencias hay entre las cuatro clases?
Entonces tenemos un total de 3 relaciones (2 asociaciones y 1 dependencia

3. ¿Existe una relación directa entre Socio y Libro? Justifique con el código.
No ya que si observamos el codigo, la clase Socio solo contiene el atributo nombre y la clase Libro solo contiene el atributo título. Ninguna de las dos tiene atributos o métodos que hagan referencia a la otra. Su única conexión es indirecta, ya que ambas son llamadas por la clase Prestamo. 

4. ¿La clase Prestamo aplica SRP? Justifique según sus razones para cambiar y proponga una separación si la considera necesaria.

No se está aplicando el principio de única responsabilidad en esta clase Préstamo el método “public void devolver” si bien no devuelve nada está ejecutando más lógica de la que debería manejar la class Préstamo, el uso de la multa perfectamente puede ser llevado a una clase propia ya que si a futuro se necesita realizar un cambio con el precio x dia,  cobrar multa extra si el libro llega dañado, poner un máximo de atrasos antes de cobrar el libro completo o bloquear al usuario (podría ser otro método o clase que al sobrepasar x días se bloquee al usuario hasta que pague la multa y devuelva el libro), para este caso es más efectivo tener una clase que funcione netamente como “cobrador de multas”
