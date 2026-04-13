<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se suele expresar colocando una estructura dentro de otra. Si una línea está formada por dos puntos, se puede declarar una `struct Punto` con sus coordenadas y una `struct Linea` que contenga dos campos de tipo `Punto`. La idea sigue siendo la misma que en lenguaje natural: una línea tiene dos puntos.

La función que calcula la distancia entre dos puntos puede trabajar directamente con dos estructuras `Punto`. Después, la longitud de una línea no necesita repetir la fórmula, sino reutilizar la función de distancia sobre los dos puntos que contiene la propia línea. De ese modo, la composición de datos y la reutilización de funciones quedan separadas con claridad.

```c
#include <math.h>
#include <stdio.h>

struct Punto {
	double x;
	double y;
};

struct Linea {
	struct Punto inicio;
	struct Punto fin;
};

double distancia(struct Punto a, struct Punto b) {
	double dx = b.x - a.x;
	double dy = b.y - a.y;
	return sqrt(dx * dx + dy * dy);
}

double longitud(struct Linea linea) {
	return distancia(linea.inicio, linea.fin);
}

int main() {
	struct Linea linea = {{1.0, 2.0}, {4.0, 6.0}};
	printf("Longitud: %.2f\n", longitud(linea));
	return 0;
}
```

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

En Java, la misma idea se expresa con clases. La clase `Linea` no contiene coordenadas sueltas, sino dos objetos `Punto`. Ahí aparece la composición en orientación a objetos: un objeto más grande queda construido a partir de otros objetos más pequeños con significado propio.

La encapsulación permite mejorar el diseño respecto a C. Si los atributos se declaran `private final` y no se ofrecen métodos modificadores, ni los puntos ni la línea pueden cambiar después de construirse. Así se garantiza que una línea siempre unirá los mismos dos puntos durante toda su vida, y que ningún código externo podrá alterar sus coordenadas por accidente.

```java
public final class Punto {
	private final double x;
	private final double y;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double getX() {
		return x;
	}

	public double getY() {
		return y;
	}

	public double distancia(Punto otro) {
		double dx = otro.x - x;
		double dy = otro.y - y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

public final class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		if (inicio == null || fin == null) {
			throw new IllegalArgumentException("Los puntos no pueden ser null");
		}
		this.inicio = inicio;
		this.fin = fin;
	}

	public Punto getInicio() {
		return inicio;
	}

	public Punto getFin() {
		return fin;
	}

	public double longitud() {
		return inicio.distancia(fin);
	}
}
```

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad indica cuántos objetos de una clase pueden estar relacionados con un objeto de otra clase. No describe cómo se implementa el código, sino la cantidad mínima y máxima de participantes en la relación. En UML se suele expresar con valores como `1`, `0..1`, `0..*` o `1..*`.

En el ejemplo de `Linea` y `Punto`, de `Linea` hacia `Punto` la multiplicidad es `2`, porque cada línea está formada exactamente por dos puntos: el inicial y el final. En cambio, de `Punto` hacia `Linea` la multiplicidad, en un modelo general, sería `0..*`, porque un punto podría no pertenecer a ninguna línea o podría ser compartido por muchas líneas distintas.

Si se quisiera imponer otro significado, esa segunda multiplicidad podría cambiar. Por ejemplo, si se modelase un sistema donde cada punto solo pudiera pertenecer a una línea, entonces no sería `0..*`. La multiplicidad, por tanto, no sale del lenguaje Java por sí solo, sino de la interpretación que se haga del problema que se está modelando.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

Se habla de composición fuerte cuando el objeto contenido forma parte del objeto contenedor de manera tan estrecha que su ciclo de vida queda ligado al de este. En ese caso, el objeto parte suele nacer como parte del todo y deja de tener sentido independiente cuando el todo desaparece. Es la relación más intensa entre objetos.

Se habla de composición débil cuando un objeto contiene o referencia a otro, pero ese segundo objeto puede existir antes, durante y después del contenedor. El contenedor no es el dueño exclusivo de la vida del contenido, sino que simplemente mantiene una relación con él. Por eso, si el contenedor desaparece, los objetos relacionados pueden seguir existiendo sin problema.

Habitualmente, a la relación débil se la llama `asociación` o `agregación`, mientras que a la fuerte se la denomina `composición` en sentido estricto. La diferencia clave no es solo que un objeto tenga referencias a otros, sino quién controla realmente la existencia de esos otros objetos.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En esos casos se suele hablar de dependencia, no de composición. La idea es que una clase necesita conocer o utilizar a otra para realizar una operación concreta, pero no la incorpora como parte estable de su estado. Se trata de un uso puntual, normalmente limitado a un método, y no de una relación estructural permanente entre objetos.

Por ejemplo, si un método recibe un `Punto` como parámetro para hacer un cálculo, la clase depende de `Punto`, pero no por ello pasa a estar compuesta por puntos. Lo mismo ocurre si crea un objeto con `new` dentro de un método y lo usa solo localmente, o si devuelve un objeto de otra clase como resultado. La composición aparece cuando esa referencia pasa a ser parte de los atributos del objeto y, por tanto, de su estructura interna.

Dicho de otro modo, la dependencia responde a "usa a" y la composición responde a "tiene un" o "tiene varios". Ambas relaciones pueden coexistir en una misma clase, pero conviene no confundir un uso temporal con una relación estable de pertenencia.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para expresar una composición fuerte en Java conviene hacer que `Linea` cree internamente sus puntos. De ese modo, los puntos nacen como parte de la línea y desde fuera no se necesita disponer de ellos antes. Conceptualmente, la línea es dueña de esos puntos concretos, aunque Java no muestre la destrucción de forma explícita.

Para expresar una composición débil, en cambio, la línea recibe dos objetos `Punto` ya existentes. Ahí no se está creando la vida de los puntos dentro de la línea, sino que simplemente se mantiene una referencia a ellos. Eso permite, por ejemplo, reutilizar un mismo punto en varias líneas distintas o seguir usando ese punto aunque la línea deje de existir.

```java
public final class Punto {
	private final double x;
	private final double y;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double distancia(Punto otro) {
		double dx = otro.x - x;
		double dy = otro.y - y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

public final class LineaFuerte {
	private final Punto inicio;
	private final Punto fin;

	public LineaFuerte(double x1, double y1, double x2, double y2) {
		this.inicio = new Punto(x1, y1);
		this.fin = new Punto(x2, y2);
	}

	public double longitud() {
		return inicio.distancia(fin);
	}
}

public final class LineaDebil {
	private final Punto inicio;
	private final Punto fin;

	public LineaDebil(Punto inicio, Punto fin) {
		if (inicio == null || fin == null) {
			throw new IllegalArgumentException("Los puntos no pueden ser null");
		}
		this.inicio = inicio;
		this.fin = fin;
	}

	public double longitud() {
		return inicio.distancia(fin);
	}
}
```

En ambos códigos el aspecto externo puede parecer parecido, pero el significado del modelo no es el mismo. En `LineaFuerte` los puntos pertenecen a la línea que los crea; en `LineaDebil` los puntos existen al margen y la línea solo se asocia con ellos.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java no se destruyen objetos manualmente como podría intentarse en C++ con destructores o con liberación explícita de memoria. Cuando un objeto deja de ser accesible desde el programa, queda candidato a ser retirado por el recolector de basura o garbage collector. Por eso no se ve un método de `Linea` que destruya sus puntos de forma directa.

En una composición fuerte, lo que se quiere expresar es que, si desaparece el contenedor, normalmente también deja de haber referencias útiles a los objetos contenidos. Si una `Linea` creó internamente sus `Punto` y nadie más los conoce, al perderse la referencia a la `Linea` también se pierden las referencias a esos puntos. A partir de ese momento, el recolector podrá liberar su memoria cuando lo considere oportuno.

Así pues, en Java la destrucción no se programa explícitamente, sino que se infiere de la alcanzabilidad de los objetos. La composición fuerte sigue teniendo sentido conceptual, pero su efecto sobre el ciclo de vida se observa mediante referencias y recolección de basura, no mediante llamadas manuales de destrucción.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Aquí conviene modelar una composición débil porque los objetos `Profesor` pueden existir por sí mismos, independientemente de un `Departamento`. El departamento mantiene dos relaciones a la vez: una con el conjunto de profesores y otra con el profesor que actúa como director. La invariante importante es que siempre debe existir director y que ese director debe estar incluido en la colección de profesores del departamento.

Como se pide ocultar la implementación interna, no se debe devolver el array ni dejar que se modifique desde fuera. En su lugar, se ofrece un método para conocer cuántos profesores hay y otro para consultar uno por posición. También conviene comprobar situaciones erróneas: director nulo, array lleno, posiciones inválidas, profesores repetidos y eliminación del profesor que actualmente es director.

En este diseño, la composición es débil porque `Departamento` no crea necesariamente a los profesores ni controla toda su vida. Simplemente los agrupa y establece restricciones coherentes sobre la relación interna entre la lista y el director. Las excepciones sirven para impedir que el objeto quede en un estado inválido.

```java
public final class Profesor {
	private final String nombre;

	public Profesor(String nombre) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre del profesor es obligatorio");
		}
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}
}

public final class Departamento {
	private static final int MAX_PROFESORES = 50;

	private final String nombre;
	private final Profesor[] profesores;
	private int numProfesores;
	private Profesor director;

	public Departamento(String nombre, Profesor directorInicial) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre del departamento es obligatorio");
		}
		if (directorInicial == null) {
			throw new IllegalArgumentException("Debe existir un director inicial");
		}

		this.nombre = nombre;
		this.profesores = new Profesor[MAX_PROFESORES];
		this.profesores[0] = directorInicial;
		this.numProfesores = 1;
		this.director = directorInicial;
	}

	public String getNombre() {
		return nombre;
	}

	public int getNumProfesores() {
		return numProfesores;
	}

	public Profesor getProfesor(int pos) {
		comprobarPosicion(pos);
		return profesores[pos];
	}

	public Profesor getDirector() {
		return director;
	}

	public void addProfesor(Profesor profesor) {
		if (profesor == null) {
			throw new IllegalArgumentException("No se puede anadir un profesor null");
		}
		if (numProfesores == MAX_PROFESORES) {
			throw new IllegalStateException("No caben mas profesores en el departamento");
		}
		if (contiene(profesor)) {
			throw new IllegalArgumentException("El profesor ya pertenece al departamento");
		}

		profesores[numProfesores] = profesor;
		numProfesores++;
	}

	public void setDirector(Profesor nuevoDirector) {
		if (nuevoDirector == null) {
			throw new IllegalArgumentException("El director no puede ser null");
		}
		if (!contiene(nuevoDirector)) {
			throw new IllegalArgumentException("El director debe pertenecer al departamento");
		}

		director = nuevoDirector;
	}

	public void eliminarProfesor(int pos) {
		comprobarPosicion(pos);

		Profesor profesorAEliminar = profesores[pos];
		if (profesorAEliminar == director) {
			throw new IllegalStateException("No se puede eliminar al director actual");
		}

		for (int i = pos; i < numProfesores - 1; i++) {
			profesores[i] = profesores[i + 1];
		}
		profesores[numProfesores - 1] = null;
		numProfesores--;
	}

	private void comprobarPosicion(int pos) {
		if (pos < 0 || pos >= numProfesores) {
			throw new IndexOutOfBoundsException("Posicion no valida: " + pos);
		}
	}

	private boolean contiene(Profesor profesor) {
		for (int i = 0; i < numProfesores; i++) {
			if (profesores[i] == profesor) {
				return true;
			}
		}
		return false;
	}
}
```

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Con `List` se mantiene la misma idea del modelo, pero desaparece bastante código mecánico. Ya no hace falta desplazar manualmente elementos al borrar, ni mantener un array de tamaño fijo, ni comprobar si queda hueco antes de insertar. El código se centra más en las invariantes del dominio y menos en la gestión manual de la estructura de almacenamiento.

Si existiese un método que devolviera todos los profesores de golpe, sería peligroso entregar directamente la lista interna, porque desde fuera se podría modificar con `add`, `remove` o `clear` y se rompería la encapsulación. Por ejemplo, podría eliminarse al director sin pasar por las comprobaciones de `Departamento`. Para evitarlo, conviene devolver una copia inmodificable, o al menos una vista no modificable, de la colección interna.

```java
import java.util.ArrayList;
import java.util.List;

public final class Departamento {
	private final String nombre;
	private final List<Profesor> profesores;
	private Profesor director;

	public Departamento(String nombre, Profesor directorInicial) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre del departamento es obligatorio");
		}
		if (directorInicial == null) {
			throw new IllegalArgumentException("Debe existir un director inicial");
		}

		this.nombre = nombre;
		this.profesores = new ArrayList<>();
		this.profesores.add(directorInicial);
		this.director = directorInicial;
	}

	public int getNumProfesores() {
		return profesores.size();
	}

	public Profesor getProfesor(int pos) {
		return profesores.get(pos);
	}

	public List<Profesor> getProfesores() {
		return List.copyOf(profesores);
	}

	public Profesor getDirector() {
		return director;
	}

	public void addProfesor(Profesor profesor) {
		if (profesor == null) {
			throw new IllegalArgumentException("No se puede anadir un profesor null");
		}
		if (profesores.contains(profesor)) {
			throw new IllegalArgumentException("El profesor ya pertenece al departamento");
		}
		profesores.add(profesor);
	}

	public void setDirector(Profesor nuevoDirector) {
		if (nuevoDirector == null || !profesores.contains(nuevoDirector)) {
			throw new IllegalArgumentException("El director debe pertenecer al departamento");
		}
		director = nuevoDirector;
	}

	public void eliminarProfesor(int pos) {
		Profesor profesor = profesores.get(pos);
		if (profesor == director) {
			throw new IllegalStateException("No se puede eliminar al director actual");
		}
		profesores.remove(pos);
	}
}
```

Lo que se ha ahorrado es, sobre todo, la lógica de bajo nivel asociada al array: el contador separado del tamaño máximo, el corrimiento de elementos al borrar y el acceso a una capacidad fija. La parte importante del código pasa a ser casi exclusivamente la protección de la invariante del director.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Se habla de composición recursiva cuando una clase contiene una referencia a otro objeto de su misma clase. En este caso, una `Persona` puede tener una madre que también es una `Persona`. La estructura puede repetirse tantas veces como generaciones se quieran representar. Si además la clase es inmutable, una vez creada una persona no se podrá cambiar ni su nombre ni su enlace hacia la madre.

Este tipo de composición resulta natural para modelar árboles genealógicos, nodos de árboles, directorios que contienen subdirectorios o comentarios que contienen respuestas. También aparece en las excepciones encadenadas, donde una excepción puede contener otra como causa. La idea común es siempre la misma: un objeto está formado, en parte, por otro objeto del mismo tipo.

```java
public final class Persona {
	private final String nombre;
	private final Persona madre;

	public Persona(String nombre, Persona madre) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre es obligatorio");
		}
		this.nombre = nombre;
		this.madre = madre;
	}

	public String getNombre() {
		return nombre;
	}

	public Persona getMadre() {
		return madre;
	}
}

public class Main {
	public static void main(String[] args) {
		Persona abuela = new Persona("Ana", null);
		Persona madre = new Persona("Beatriz", abuela);
		Persona nieto = new Persona("Carlos", madre);

		System.out.println("Nieto: " + nieto.getNombre());
		System.out.println("Madre: " + nieto.getMadre().getNombre());
		System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
	}
}
```

En el ejemplo anterior se observa cómo la referencia se encadena de forma recursiva desde el nieto hasta la abuela. El caso base aparece cuando una persona no tiene madre conocida en el modelo y se representa con `null`.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación bidireccional es aquella en la que ambos extremos conocen la relación. No solo `Departamento` sabría qué profesores contiene, sino que además cada `Profesor` sabría a qué `Departamento` pertenece. Así, la navegación podría hacerse en los dos sentidos: del departamento a sus profesores y del profesor a su departamento.

Para implementar eso en el ejemplo anterior, habría que añadir en `Profesor` un atributo de tipo `Departamento` y mantenerlo sincronizado con la lista interna del departamento. Esa sincronización es la parte delicada: cuando se añade un profesor al departamento, también debe actualizarse la referencia del profesor; cuando se elimina, debe limpiarse o cambiarse esa referencia. Si no se hace en ambos lados a la vez, se producirían inconsistencias.

Por eso, en relaciones bidireccionales conviene centralizar las operaciones en métodos que actualicen los dos extremos de forma coordinada, y restringir las modificaciones directas desde fuera. La dificultad no está en añadir una referencia más, sino en garantizar que ambas vistas de la relación digan siempre lo mismo.

```java
import java.util.ArrayList;
import java.util.List;

public final class Profesor {
	private final String nombre;
	private Departamento departamento; // Nueva referencia hacia el contenedor

	public Profesor(String nombre) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre es obligatorio");
		}
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}

	public Departamento getDepartamento() {
		return departamento;
	}

	// Mediante visibilidad de paquete limitamos que solo el propio
	// Departamento (en la misma carpeta) pueda actualizar este enlace
	void setDepartamento(Departamento nuevoDepartamento) {
		this.departamento = nuevoDepartamento;
	}
}

public final class Departamento {
	private final String nombre;
	private final List<Profesor> profesores;

	public Departamento(String nombre) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre es obligatorio");
		}
		this.nombre = nombre;
		this.profesores = new ArrayList<>();
	}

	public String getNombre() {
		return nombre;
	}

	public void addProfesor(Profesor profesor) {
		if (profesor == null) {
			throw new IllegalArgumentException("El profesor no puede ser null");
		}

		if (!profesores.contains(profesor)) {
			profesores.add(profesor);
			// Sincroniza al profesor de manera coordinada
			profesor.setDepartamento(this);
		}
	}

	public void eliminarProfesor(Profesor profesor) {
		if (profesores.remove(profesor)) {
			// Rompe el enlace del profesor con el departamento
			profesor.setDepartamento(null);
		}
	}
}
```
