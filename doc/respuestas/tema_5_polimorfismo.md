<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es la capacidad de que una misma operación se comporte de forma distinta según el tipo real del objeto sobre el que se invoca. En programación orientada a objetos, esto significa que si se tiene una referencia de un supertipo (por ejemplo, `Soldado`) y se llama a un método, el comportamiento que se ejecuta es el que corresponde al tipo real del objeto (un `Zapador`, un `Artillero`, etc.), no al tipo de la referencia. Esto permite escribir código genérico que trabaja con el supertipo y que funciona correctamente con cualquier subtipo presente o futuro.

La **sobreescritura** (_overriding_) de métodos es el mecanismo que hace posible el polimorfismo. Consiste en que una subclase redefina un método que ya existía en la superclase, proporcionando una implementación propia que sustituye a la original. Cuando se invoca ese método a través de una referencia del supertipo, se ejecuta la versión de la subclase si el objeto real es de ese subtipo. Sin sobreescritura, todos los objetos se comportarían exactamente igual al invocar un método heredado, y el polimorfismo no tendría utilidad práctica.

El polimorfismo es especialmente valioso para la extensibilidad del código. En C, para conseguir algo similar habría que usar punteros a funciones dentro de estructuras y gestionar manualmente la selección de la función adecuada. En Java, el lenguaje se encarga de esa selección automáticamente, lo que simplifica enormemente el diseño y reduce la posibilidad de errores.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La **ligadura dinámica** (o enlace tardío, _late binding_) consiste en que la decisión sobre qué versión de un método se debe ejecutar se toma en tiempo de ejecución, no en tiempo de compilación. Cuando se tiene una referencia de tipo `Soldado` que apunta a un objeto real de tipo `Zapador` y se llama a `saludar()`, el programa determina en el momento de la llamada que debe ejecutar la versión de `Zapador`, no la de `Soldado`. Esta decisión se basa en el tipo real del objeto, no en el tipo declarado de la referencia. Es el mecanismo que hace funcionar el polimorfismo.

En **C++**, por defecto los métodos usan ligadura estática (enlace temprano): se ejecuta la versión del método correspondiente al tipo de la referencia o puntero, no al tipo real del objeto. Para activar la ligadura dinámica es necesario declarar el método como `virtual` en la clase base. Solo los métodos marcados como `virtual` participan en el polimorfismo. En **Java**, en cambio, todos los métodos de instancia usan ligadura dinámica de forma automática, sin necesidad de ninguna palabra clave especial. No existe un equivalente a `virtual`; el comportamiento polimórfico está activo por defecto.

En **Python**, todos los métodos también usan ligadura dinámica de forma automática, ya que es un lenguaje de tipado dinámico. De hecho, Python va más allá: ni siquiera es necesario que exista una relación de herencia entre las clases para que el polimorfismo funcione, ya que se basa en el llamado _duck typing_ ("si camina como un pato y suena como un pato, es un pato"). Basta con que un objeto tenga el método esperado para que la llamada funcione correctamente, independientemente de su tipo.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

En este ejemplo se define la clase `Soldado` con un método `saludar()` que muestra un saludo genérico. La subclase `Zapador` sobreescribe completamente ese método con su propia versión, mientras que `Artillero` no lo sobreescribe y por tanto hereda el comportamiento original de `Soldado`.

Al crear un array de tipo `Soldado[]` con objetos de ambos subtipos y recorrerlo con un bucle, se observa el polimorfismo en acción: cuando el objeto real es un `Zapador`, se ejecuta su versión sobreescrita de `saludar()`; cuando es un `Artillero`, se ejecuta la versión heredada de `Soldado`. Todo ello sin que el bucle necesite conocer el tipo concreto de cada elemento.

```java
public class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}

	public void saludar() {
		System.out.println("Soy el soldado " + nombre + ". ¡A sus órdenes!");
	}
}

public class Zapador extends Soldado {
	public Zapador(String nombre) {
		super(nombre);
	}

	@Override
	public void saludar() {
		System.out.println("Soy " + getNombre() + ", zapador especialista en demoliciones.");
	}
}

public class Artillero extends Soldado {
	public Artillero(String nombre) {
		super(nombre);
	}
}

public class Main {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Zapador("Elena"),
			new Artillero("Carlos"),
			new Zapador("Marta"),
			new Artillero("Luis")
		};

		for (Soldado soldado : peloton) {
			soldado.saludar(); // Se ejecuta la versión correspondiente al tipo real
		}
	}
}
```

La salida del programa sería:

```
Soy Elena, zapador especialista en demoliciones.
Soy el soldado Carlos. ¡A sus órdenes!
Soy Marta, zapador especialista en demoliciones.
Soy el soldado Luis. ¡A sus órdenes!
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al sobreescribir un método es posible invocar la versión original de la superclase desde la subclase utilizando la palabra clave `super`. En lugar de reemplazar completamente el comportamiento, se puede llamar a `super.saludar()` para ejecutar primero el saludo base del `Soldado` y después añadir el comportamiento propio del `Zapador`. De esta forma se reutiliza el código de la superclase y se extiende su comportamiento sin duplicarlo.

La palabra clave `super` permite referenciar la implementación de la clase padre desde la clase hija. Es el mismo mecanismo que se usa con `super(...)` en los constructores para invocar al constructor de la superclase, pero aplicado a métodos ordinarios. En C, algo similar requeriría llamar explícitamente a una función distinta; en Java, `super` resuelve esto de forma directa y segura.

```java
public class Zapador extends Soldado {
	public Zapador(String nombre) {
		super(nombre);
	}

	@Override
	public void saludar() {
		super.saludar(); // Ejecuta el saludar() de Soldado
		System.out.println("ZAPADOR A SUS ORDENES");
	}
}
```

Con esta implementación, al llamar a `saludar()` sobre un objeto `Zapador`, primero se imprime el saludo genérico del soldado y a continuación se añade la línea propia del zapador.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (_overriding_) y sobrecarga (_overloading_)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java, los tipos de los parámetros deben ser exactamente los mismos que los del método original de la superclase. Si se cambia el tipo o el número de parámetros, el compilador interpreta que se está definiendo un método nuevo (sobrecarga), no sobreescribiendo el existente. En cuanto al tipo de retorno, debe ser el mismo o un subtipo del tipo de retorno original (lo que se conoce como _tipo de retorno covariante_). Además, la visibilidad del método sobreescrito no puede ser más restrictiva que la del original: si el método base es `public`, la sobreescritura no puede ser `protected` ni `private`.

La **sobreescritura** (_overriding_) y la **sobrecarga** (_overloading_) son conceptos distintos. La sobreescritura consiste en que una subclase redefina un método heredado con la misma firma (mismo nombre, mismos parámetros), proporcionando un comportamiento diferente. La sobrecarga consiste en definir varios métodos con el mismo nombre pero distinta lista de parámetros dentro de la misma clase o entre una clase y su superclase. La sobreescritura está ligada al polimorfismo y se resuelve en tiempo de ejecución; la sobrecarga se resuelve en tiempo de compilación según los tipos de los argumentos.

La anotación `@Override` se coloca antes del método para indicar explícitamente que se pretende sobreescribir un método de la superclase. Si el método anotado no coincide exactamente con ningún método heredado (por ejemplo, por un error tipográfico en el nombre o una diferencia en los parámetros), el compilador genera un error. Sin esta anotación, ese mismo error pasaría desapercibido: el compilador simplemente crearía un método nuevo por sobrecarga y el polimorfismo no funcionaría como se espera. Por eso es altamente recomendable usar `@Override` siempre que se pretenda sobreescribir un método.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, efectivamente. Desde el momento en que se sobreescribe `toString()` o `equals()` en una clase, ya se está usando polimorfismo, aunque no se sea consciente de ello. Ambos métodos están definidos en la clase `Object`, que es la raíz de toda la jerarquía de clases en Java. Al proporcionar una implementación propia en una clase concreta, se está sobreescribiendo un método heredado, que es exactamente lo que permite el polimorfismo.

El caso de `toString()` es muy ilustrativo. Cuando se pasa un objeto a `System.out.println()`, internamente se invoca `toString()` sobre ese objeto. Gracias al polimorfismo, se ejecuta la versión de `toString()` que corresponde al tipo real del objeto, no la versión genérica de `Object` (que solo devolvería el nombre de la clase y un código hash). Lo mismo ocurre con `equals()`: al comparar objetos, se puede redefinir el criterio de igualdad para que se base en los atributos relevantes de la clase en lugar de comparar referencias.

Este es uno de los aspectos más elegantes del diseño de Java: el polimorfismo está integrado de forma natural en el lenguaje desde los cimientos. No es un concepto que se añada a posteriori, sino que se utiliza desde las operaciones más básicas. Cualquier clase que sobreescriba métodos de `Object` está participando en el mecanismo polimórfico, lo que demuestra que la herencia y el polimorfismo no son conceptos teóricos aislados, sino herramientas que se aplican de forma constante en la práctica diaria.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una **clase abstracta** es una clase que no puede ser instanciada directamente. Representa un concepto incompleto o general que solo tiene sentido cuando se concreta mediante subclases. Se declara añadiendo la palabra clave `abstract` antes de `class`. Si se intenta hacer `new` sobre una clase abstracta, el compilador produce un error. Su propósito es servir de base común para las subclases, definiendo una estructura y un comportamiento parcial que estas deben completar.

Un **método abstracto** es un método que se declara sin cuerpo (sin llaves ni implementación), precedido por la palabra clave `abstract`. Este método define una obligación: toda subclase no abstracta debe proporcionar una implementación concreta. Si una clase contiene al menos un método abstracto, la clase entera debe declararse como abstracta. Es una forma de establecer un contrato: se garantiza que todos los subtipos tendrán ese método, pero cada uno lo implementará a su manera.

En el ejemplo, `Soldado` se convierte en clase abstracta porque tiene el método `atacar()` abstracto. Cada subtipo (`Zapador` y `Artillero`) debe implementar `atacar()` con su propio comportamiento. El método `saludar()`, al tener una implementación concreta, se hereda normalmente. La palabra `abstract` debe colocarse en la declaración de la clase `Soldado` y en la firma del método `atacar()`.

```java
public abstract class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}

	public void saludar() {
		System.out.println("Soy el soldado " + nombre + ". ¡A sus órdenes!");
	}

	public abstract void atacar(); // Sin cuerpo: las subclases deben implementarlo
}

public class Zapador extends Soldado {
	public Zapador(String nombre) {
		super(nombre);
	}

	@Override
	public void atacar() {
		System.out.println(getNombre() + " coloca una carga explosiva. ¡BOOM!");
	}
}

public class Artillero extends Soldado {
	public Artillero(String nombre) {
		super(nombre);
	}

	@Override
	public void atacar() {
		System.out.println(getNombre() + " dispara un cohete. ¡FUEGO!");
	}
}

public class Main {
	public static void main(String[] args) {
		// Soldado s = new Soldado("Pepe"); // ERROR: no se puede instanciar una clase abstracta

		Soldado[] peloton = {
			new Zapador("Elena"),
			new Artillero("Carlos")
		};

		for (Soldado soldado : peloton) {
			soldado.saludar();
			soldado.atacar(); // Polimorfismo: cada soldado ataca a su manera
		}
	}
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave `final` aplicada a un **método** impide que las subclases lo sobreescriban. Esto significa que el comportamiento de ese método queda fijado de forma definitiva en la clase que lo define; ninguna subclase podrá proporcionar una versión propia. Aplicada a una **clase**, `final` impide que se pueda heredar de ella en absoluto: no podrá tener subclases. En ambos casos, `final` actúa como un candado que limita la extensibilidad.

Su relación con el polimorfismo es directa: al bloquear la sobreescritura de un método (o impedir la herencia de una clase entera), se está restringiendo o eliminando la posibilidad de polimorfismo sobre esos elementos. Un método `final` siempre se ejecutará de la misma forma, independientemente del tipo real del objeto. Esto puede ser útil cuando se quiere garantizar que un determinado comportamiento no sea alterado por ninguna subclase, por razones de seguridad, corrección o rendimiento (el compilador puede optimizar las llamadas a métodos `final` al saber que no serán sobreescritos).

Un ejemplo muy conocido de clase `final` en la API estándar de Java es `String`. La clase `String` está declarada como `final`, lo que impide que se pueda crear una subclase de ella. Esta decisión de diseño se tomó por razones de seguridad e inmutabilidad: si se pudiera heredar de `String` y sobreescribir sus métodos, se podría romper el contrato de inmutabilidad que muchas partes del lenguaje y las bibliotecas asumen. Otras clases `final` conocidas son las clases envolventes como `Integer`, `Double` y `Boolean`.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Una **interfaz** en Java es un tipo que define un contrato: un conjunto de métodos que cualquier clase que la implemente debe proporcionar. Se declara con la palabra clave `interface` en lugar de `class`, y los métodos se definen sin cuerpo (de forma similar a los métodos abstractos). Una clase indica que cumple con una interfaz usando la palabra clave `implements`. A partir de ese momento, el compilador obliga a que la clase proporcione una implementación concreta de todos los métodos de la interfaz.

Aunque se parecen a las clases abstractas en que definen métodos sin implementación, hay diferencias importantes. Una clase abstracta puede contener atributos de instancia, constructores y métodos con implementación completa, además de métodos abstractos. Una interfaz, tradicionalmente, solo podía contener constantes (`public static final`) y métodos abstractos. A partir de Java 8, las interfaces pueden incluir métodos con implementación por defecto (`default`) y métodos estáticos, pero siguen sin poder tener atributos de instancia ni constructores. La diferencia conceptual también es clara: una clase abstracta modela una relación "es-un" parcial, mientras que una interfaz modela una capacidad o un contrato ("es capaz de", "cumple con").

Una ventaja fundamental de las interfaces es que una clase puede implementar **más de una interfaz**, a diferencia de la herencia, donde solo se puede extender una única clase. Esto permite que un objeto cumpla con múltiples contratos simultáneamente. Por ejemplo, una clase `Documento` podría implementar `Imprimible` y `Serializable` a la vez, adquiriendo ambas capacidades sin necesidad de herencia múltiple de clases. Esto resuelve de forma elegante la limitación de herencia simple de Java.

```java
public interface Imprimible {
	void imprimir();
}

public interface Serializable {
	String serializar();
}

public class Documento implements Imprimible, Serializable {
	private String contenido;

	public Documento(String contenido) {
		this.contenido = contenido;
	}

	@Override
	public void imprimir() {
		System.out.println("Imprimiendo: " + contenido);
	}

	@Override
	public String serializar() {
		return "DOC:" + contenido;
	}
}
```

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y _downcasting_ para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

En este diseño, `Punto` es una clase abstracta con un método abstracto `calcularDistanciaA(Punto otro)`. Las subclases `Punto2D` y `Punto3D` implementan ese método cada una con su fórmula correspondiente. Dentro de la implementación, se usa `instanceof` para verificar que el punto recibido es del mismo subtipo y se realiza un _downcasting_ para acceder a las coordenadas específicas. Si el punto recibido no es compatible, se lanza una excepción.

La clase `Linea` demuestra el poder del polimorfismo: recibe dos objetos de tipo `Punto` (sin saber si son 2D o 3D) y calcula su longitud delegando en `calcularDistanciaA()`. Gracias a la ligadura dinámica, se ejecuta automáticamente la implementación correcta según el tipo real de los puntos. La clase `Linea` no necesita conocer los subtipos; trabaja exclusivamente con la abstracción `Punto`.

Este patrón es muy habitual en diseño orientado a objetos: se programa contra la abstracción (el supertipo) y se deja que el polimorfismo seleccione el comportamiento concreto. Si en el futuro se añadiera un `Punto4D`, la clase `Linea` seguiría funcionando sin ninguna modificación, siempre que `Punto4D` implemente `calcularDistanciaA()` correctamente.

```java
public abstract class Punto {
	public abstract double calcularDistanciaA(Punto otro);
}

public class Punto2D extends Punto {
	private final double x;
	private final double y;

	public Punto2D(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double getX() {
		return x;
	}

	public double getY() {
		return y;
	}

	@Override
	public double calcularDistanciaA(Punto otro) {
		if (!(otro instanceof Punto2D)) {
			throw new IllegalArgumentException("Se esperaba un Punto2D");
		}
		Punto2D otro2D = (Punto2D) otro;
		double dx = this.x - otro2D.x;
		double dy = this.y - otro2D.y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

public class Punto3D extends Punto {
	private final double x;
	private final double y;
	private final double z;

	public Punto3D(double x, double y, double z) {
		this.x = x;
		this.y = y;
		this.z = z;
	}

	public double getX() {
		return x;
	}

	public double getY() {
		return y;
	}

	public double getZ() {
		return z;
	}

	@Override
	public double calcularDistanciaA(Punto otro) {
		if (!(otro instanceof Punto3D)) {
			throw new IllegalArgumentException("Se esperaba un Punto3D");
		}
		Punto3D otro3D = (Punto3D) otro;
		double dx = this.x - otro3D.x;
		double dy = this.y - otro3D.y;
		double dz = this.z - otro3D.z;
		return Math.sqrt(dx * dx + dy * dy + dz * dz);
	}
}

public class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		if (inicio == null || fin == null) {
			throw new IllegalArgumentException("Los puntos no pueden ser nulos");
		}
		this.inicio = inicio;
		this.fin = fin;
	}

	public double longitud() {
		return inicio.calcularDistanciaA(fin); // Polimorfismo: se ejecuta la versión correcta
	}
}

public class Main {
	public static void main(String[] args) {
		Linea linea2D = new Linea(new Punto2D(0, 0), new Punto2D(3, 4));
		System.out.println("Longitud 2D: " + linea2D.longitud()); // 5.0

		Linea linea3D = new Linea(new Punto3D(0, 0, 0), new Punto3D(1, 2, 2));
		System.out.println("Longitud 3D: " + linea3D.longitud()); // 3.0
	}
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La **herencia de interfaces** consiste en que una interfaz puede extender a otra interfaz usando la palabra clave `extends`, de forma análoga a como una clase extiende a otra clase. La interfaz hija hereda todos los métodos de la interfaz padre y puede añadir métodos nuevos. Cualquier clase que implemente la interfaz hija deberá proporcionar implementación para todos los métodos: tanto los propios como los heredados de la interfaz padre.

En Java sí existe la **herencia múltiple de interfaces**: una interfaz puede extender varias interfaces a la vez, separándolas por comas después de `extends`. Esto es algo que no está permitido para las clases (que solo pueden extender una única clase), pero las interfaces no tienen esta restricción porque no aportan estado de instancia, lo que evita los conflictos típicos de la herencia múltiple. De la misma forma, una clase puede implementar múltiples interfaces con `implements`.

En el siguiente ejemplo, `Fichero` define la capacidad de leer contenido. `FicheroEscribible` extiende `Fichero` y añade las capacidades de escribir y eliminar. Una clase que implemente `FicheroEscribible` debe proporcionar los tres métodos: el heredado de `Fichero` y los dos propios de `FicheroEscribible`. De este modo se construye una jerarquía de contratos progresivamente más ricos.

```java
public interface Fichero {
	String leer();
}

public interface FicheroEscribible extends Fichero {
	void escribir(String contenido);
	void eliminar();
}

public class FicheroTexto implements FicheroEscribible {
	private String contenido;
	private boolean eliminado;

	public FicheroTexto(String contenidoInicial) {
		this.contenido = contenidoInicial;
		this.eliminado = false;
	}

	@Override
	public String leer() {
		if (eliminado) {
			throw new IllegalStateException("El fichero ha sido eliminado");
		}
		return contenido;
	}

	@Override
	public void escribir(String contenido) {
		if (eliminado) {
			throw new IllegalStateException("El fichero ha sido eliminado");
		}
		this.contenido = contenido;
	}

	@Override
	public void eliminar() {
		this.contenido = null;
		this.eliminado = true;
	}
}

public class Main {
	public static void main(String[] args) {
		// Se puede usar la referencia del tipo más general
		Fichero soloLectura = new FicheroTexto("Hola mundo");
		System.out.println(soloLectura.leer()); // "Hola mundo"

		// O la referencia del tipo más específico
		FicheroEscribible editable = new FicheroTexto("Contenido inicial");
		System.out.println(editable.leer());
		editable.escribir("Contenido modificado");
		System.out.println(editable.leer());
		editable.eliminar();
	}
}
```
