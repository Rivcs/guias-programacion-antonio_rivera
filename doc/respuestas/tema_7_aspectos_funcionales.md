<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a función es una variable que almacena la dirección de memoria de una función concreta. Igual que un puntero a un dato permite referenciar una posición de memoria, un puntero a función permite invocar una función sin escribir su nombre directamente, lo que da más flexibilidad para decidir en tiempo de ejecución qué comportamiento se quiere ejecutar.

En C, el tipo del puntero debe coincidir exactamente con la firma de la función. En el ejemplo siguiente, la función recibe una cadena modificable y la convierte a mayúsculas in situ. Después se declara la variable local `aMayusculas` como puntero a esa función y se invoca a través de ella.

```c
#include <stdio.h>
#include <ctype.h>

char* a_mayusculas(char* texto) {
	for (int i = 0; texto[i] != '\0'; i++) {
		texto[i] = (char)toupper((unsigned char)texto[i]);
	}
	return texto;
}

int main(void) {
	char texto[] = "Hola mundo";
	char* (*aMayusculas)(char*) = a_mayusculas;

	printf("%s\n", aMayusculas(texto));
	return 0;
}
```


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima que puede definirse de forma inline, normalmente donde se necesita, sin tener que declarar antes una función con nombre. Se usa mucho cuando se quiere pasar comportamiento como si fuera un dato, por ejemplo para transformar elementos, filtrar colecciones o reaccionar a eventos.

La gran diferencia con un puntero a función clásico es que la lambda suele integrarse con el sistema de tipos del lenguaje y puede capturar información del entorno donde se define. En JavaScript esto aparece de forma muy natural, y en Java se expresa a través de interfaces funcionales como `Function<String, String>`.

```javascript
const aMayusculas = texto => texto.toUpperCase();

console.log(aMayusculas("Hola mundo"));
```

```java
import java.util.function.Function;

public class Main {
	public static void main(String[] args) {
		Function<String, String> aMayusculas = texto -> texto.toUpperCase();
		System.out.println(aMayusculas.apply("Hola mundo"));
	}
}
```


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es una forma de programar en la que se construyen soluciones combinando funciones, evitando en la medida de lo posible el estado mutable y los efectos secundarios. En este enfoque interesa más describir qué transformación se quiere aplicar a los datos que detallar paso a paso cómo recorrerlos o modificarlos.

Se dice que un lenguaje es multi-paradigma cuando permite programar de varias formas distintas, por ejemplo orientada a objetos y funcional a la vez. Java 8 entra en esa categoría porque sigue siendo un lenguaje orientado a objetos, pero añade lambdas, referencias a métodos y funciones predefinidas para trabajar de manera funcional. Decir que las funciones son ciudadanos de primera clase significa que pueden almacenarse en variables, pasarse como parámetros y devolverse como resultado, igual que cualquier otro valor.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una lambda en Java sigue la forma `parámetros -> expresión` o `parámetros -> { bloque }`. Si la lambda tiene un único parámetro, los paréntesis pueden omitirse en muchos casos; si tiene varios, o si no tiene ninguno, los paréntesis son obligatorios. Cuando el cuerpo es una sola expresión, el valor de esa expresión se devuelve de forma implícita.

La lambda no tiene tipo propio por sí sola: su tipo lo determina la interfaz funcional a la que se asigna. Por eso, la misma lambda puede representar cosas distintas según el contexto, siempre que encaje con la firma esperada. Si el bloque contiene varias sentencias, entonces se usan llaves y se escribe `return` de forma explícita cuando proceda.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Pasar una función como parámetro permite separar la lógica de recorrido o gestión de datos de la lógica concreta de transformación. Así, un método como `transformar` puede encargarse de recibir el texto y aplicar la función que se le pase, sin saber si la transformación consiste en poner en mayúsculas, invertir la cadena o añadir un sufijo.

Este estilo evita repetir código y hace que el comportamiento sea más reutilizable. En Java se logra a través de una interfaz funcional, mientras que en JavaScript basta con pasar la función directamente como argumento.

```javascript
const aMayusculas = texto => texto.toUpperCase();

function transformar(texto, transformador) {
	return transformador(texto);
}

console.log(transformar("Hola mundo", aMayusculas));
```

```java
import java.util.function.Function;

public class Main {
	public static String transformar(String texto, Function<String, String> transformador) {
		return transformador.apply(texto);
	}

	public static void main(String[] args) {
		Function<String, String> aMayusculas = texto -> texto.toUpperCase();
		System.out.println(transformar("Hola mundo", aMayusculas));
	}
}
```


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Una de las ventajas de las lambdas es que no siempre hace falta guardar la función en una variable previa. Si solo se va a usar una vez, puede definirse justo en la llamada al método, lo que hace el código más compacto y deja claro que esa transformación no se reutiliza en otro sitio.

En este caso, la función de inversión se escribe directamente como argumento de `transformar`. La idea es la misma en JavaScript y en Java: la diferencia está en que Java necesita el tipo funcional esperado, mientras que JavaScript lo infiere sin declaración previa.

```javascript
function transformar(texto, transformador) {
	return transformador(texto);
}

console.log(transformar("Hola mundo", texto => texto.split("").reverse().join("")));
```

```java
import java.util.function.Function;

public class Main {
	public static String transformar(String texto, Function<String, String> transformador) {
		return transformador.apply(texto);
	}

	public static void main(String[] args) {
		System.out.println(transformar("Hola mundo", texto -> new StringBuilder(texto).reverse().toString()));
	}
}
```


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un closure es una función que conserva acceso al entorno donde fue creada, incluso cuando se ejecuta más tarde. Eso significa que la lambda no solo “sabe” qué hacer, sino también qué variables del contexto la rodeaban en el momento de definirse. Este mecanismo es muy útil para crear funciones parametrizadas sin necesidad de construir clases auxiliares.

En Java, una lambda puede leer variables locales del entorno siempre que sean final o efectivamente finales. En el ejemplo siguiente, la lambda concatena una cadena externa a la entrada, y esa variable queda capturada por la función.

```java
import java.util.function.Function;

public class Main {
	public static void main(String[] args) {
		String sufijo = "!!!";
		Function<String, String> anhadirSufijo = texto -> texto + sufijo;

		System.out.println(anhadirSufijo.apply("Hola mundo"));
	}
}
```

La idea de closure se ve mejor si se piensa en la función devuelta como algo que recuerda el valor capturado. Por eso resulta tan útil para fabricar transformaciones especializadas, ya que una misma plantilla de lambda puede adaptarse a distintas variables del contexto donde se define.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Un puntero a función en C solo referencia una función con una firma concreta; no transporta estado adicional y no captura variables del entorno. Si se quiere que una función use información externa, hay que pasarla por parámetros o recurrir a variables globales, lo que reduce bastante la encapsulación.

Una lambda, en cambio, puede cerrar sobre variables del contexto y comportarse como una pequeña unidad de comportamiento con memoria asociada. Además, en lenguajes como Java o JavaScript, la lambda se integra con tipos funcionales y puede escribirse inline de manera mucho más expresiva. Por eso una lambda no es solo “una dirección de función”, sino una construcción más rica que puede incluir contexto, tipo y comportamiento a la vez.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones permite construir comportamiento parametrizado. En este caso, `crearDescuento` recibe un porcentaje y devuelve una función que ya sabe aplicar ese porcentaje a cualquier cantidad que se le pase después. Eso evita recalcular o volver a pasar el mismo porcentaje cada vez.

El valor del porcentaje queda capturado por la lambda devuelta, así que la función resultante conserva ese dato aunque `crearDescuento` ya haya terminado. Esa es precisamente la closure: la función no solo ejecuta una fórmula, sino que recuerda el contexto con el que fue creada.

```java
import java.util.function.Function;

public class Main {
	public static Function<Double, Double> crearDescuento(double porcentaje) {
		return cantidad -> cantidad * (1.0 - porcentaje / 100.0);
	}

	public static void main(String[] args) {
		Function<Double, Double> descuento10 = crearDescuento(10);
		Function<Double, Double> descuento25 = crearDescuento(25);

		double precio = 100.0;
		System.out.println(descuento10.apply(precio));
		System.out.println(descuento25.apply(precio));
	}
}
```


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional es una interfaz que define un único método abstracto, y por tanto sirve como tipo objetivo para una lambda o una referencia a método. Cuando Java ve una lambda, necesita saber qué firma debe cumplir, y esa firma la aporta la interfaz funcional correspondiente.

Además del único método abstracto, una interfaz funcional puede tener métodos `default`, métodos `static` e incluso heredar métodos de `Object` sin dejar de ser funcional. Lo habitual es anotarla con `@FunctionalInterface`, no porque sea obligatorio, sino porque ayuda al compilador a comprobar que realmente cumple el requisito principal y evita errores si en el futuro se añade por accidente un segundo método abstracto.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una interfaz funcional propia permite dar nombre semántico al comportamiento que se espera. En este caso, `Transformador` describe de forma muy clara que se trata de algo capaz de convertir una cadena en otra, sin tener que hablar directamente de `Function<String, String>`.

Aunque por debajo el mecanismo es el mismo, usar una interfaz con nombre mejora la legibilidad cuando el código crece o cuando el comportamiento tiene un significado de dominio más concreto. Esa interfaz puede usarse después con lambdas exactamente igual que cualquier otra interfaz funcional.

```java
@FunctionalInterface
public interface Transformador {
	String transformar(String texto);
}

public class Main {
	public static String aplicar(String texto, Transformador transformador) {
		return transformador.transformar(texto);
	}

	public static void main(String[] args) {
		Transformador aMayusculas = texto -> texto.toUpperCase();
		System.out.println(aplicar("Hola mundo", aMayusculas));
	}
}
```


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Si la interfaz se vuelve genérica, deja de estar atada a cadenas y puede reutilizarse para cualquier transformación entre tipos. Eso la aproxima mucho a la interfaz `Function<T, R>` de la librería estándar, pero sigue siendo útil si se quiere expresar una intención más concreta en el dominio del problema.

En el ejemplo siguiente, el transformador recibe un `Double` y devuelve un `Integer` redondeado. La firma genérica permite declarar claramente el tipo de entrada y el tipo de salida, sin necesidad de recurrir a casts ni perder seguridad de tipos.

```java
@FunctionalInterface
public interface Transformador<T, R> {
	R transformar(T valor);
}

public class Main {
	public static void main(String[] args) {
		Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);
		System.out.println(redondear.transformar(3.6));
	}
}
```


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java incluye un paquete completo de interfaces funcionales predefinidas en `java.util.function`. La más general para transformar valores es `Function<T, R>`, pero hay muchas otras pensadas para casos frecuentes, como consumir valores, producirlos, comprobar predicados o combinar funciones binarias.

Entre las más usadas están `Function<T, R>`, `Consumer<T>`, `Supplier<T>`, `Predicate<T>`, `UnaryOperator<T>`, `BinaryOperator<T>`, `BiFunction<T, U, R>`, `BiConsumer<T, U>` y `BiPredicate<T, U>`. También existen variantes especializadas para tipos primitivos, como `IntPredicate`, `IntConsumer`, `IntFunction<R>`, `ToIntFunction<T>`, `IntUnaryOperator` o `DoubleBinaryOperator`, que evitan el coste de los boxing y unboxing innecesarios.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

`forEach` permite recorrer una colección indicando qué se quiere hacer con cada elemento, en lugar de escribir explícitamente el índice o el mecanismo de iteración. Eso encaja bien con la programación funcional porque la atención se centra en la acción aplicada a cada valor, no en el control del bucle.

En el ejemplo siguiente, la lambda recibe cada entero y muestra un mensaje solo si es positivo. El recorrido completo sigue estando a cargo de `forEach`, mientras que la lógica concreta de filtrado y salida queda en la lambda.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
	public static void main(String[] args) {
		List<Integer> numeros = Arrays.asList(-3, 0, 5, 8, -1);

		numeros.forEach(n -> {
			if (n > 0) {
				System.out.println(n + " es positivo");
			}
		});
	}
}
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

Se usa `Consumer<? super T>` porque el consumidor no necesita ser exactamente de `T`, sino que puede aceptar cualquier supertipo de `T`. Por ejemplo, si una lista contiene `String`, un `Consumer<Object>` también puede consumir esos `String`, así que restringirlo a `Consumer<T>` sería innecesariamente rígido y haría perder flexibilidad.

Esto se resume con la regla PECS: “Producer Extends, Consumer Super”. Si algo produce valores, conviene usar `extends`; si algo los consume, conviene usar `super`. Aplicado a `transformar`, si el método recibe un valor de tipo `T` y una función que lo transforma en `R`, una firma más flexible sería `Function<? super T, ? extends R>`, porque la entrada se consume y la salida se produce.

```java
import java.util.function.Function;

public class Main {
	public static <T, R> R transformar(T valor, Function<? super T, ? extends R> transformador) {
		return transformador.apply(valor);
	}
}
```

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una referencia a método es una forma abreviada de pasar una función existente sin escribir una lambda equivalente. En JavaScript suele ser necesario cuidar el valor de `this`, porque al extraer un método puede perderse el contexto del objeto. En Java, el enlace entre la instancia y el método queda más claramente expresado en la propia referencia.

La idea en ambos lenguajes es la misma: se crea una variable que apunta al comportamiento `saludar` y luego se invoca desde esa variable. En JavaScript puede usarse `bind` para fijar el contexto, mientras que en Java la referencia `persona::saludar` ya conserva la instancia concreta.

```javascript
class Persona {
	constructor(nombre) {
		this.nombre = nombre;
	}

	saludar() {
		console.log(`Hola, soy ${this.nombre}`);
	}
}

const persona = new Persona("Ana");
const saludar = persona.saludar.bind(persona);
saludar();
```

```java
public class Persona {
	private final String nombre;

	public Persona(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Hola, soy " + nombre);
	}
}

public class Main {
	public static void main(String[] args) {
		Persona persona = new Persona("Ana");
		Runnable saludar = persona::saludar;
		saludar.run();
	}
}
```


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java se distinguen cuatro formas habituales de referencia a método. La primera es la referencia a un método estático, como `Math::abs`. La segunda es la referencia a un constructor, como `ArrayList::new`. La tercera es la referencia a un método de instancia de una instancia concreta, como `persona::saludar`. La cuarta es la referencia a un método de instancia sobre cualquier instancia de un tipo, como `String::toUpperCase`.

Estas formas son útiles porque reducen ruido y dejan más clara la intención del código. En lugar de escribir lambdas largas para llamar a un método existente, se puede expresar directamente qué método se reutiliza y en qué contexto se aplicará.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;
import java.util.function.Supplier;

public class Main {
	public static void main(String[] args) {
		Function<Integer, Integer> abs = Math::abs;
		Supplier<List<String>> listaVacia = ArrayList::new;

		Persona persona = new Persona("Ana");
		Runnable saludar = persona::saludar;

		Function<String, String> aMayusculas = String::toUpperCase;

		System.out.println(abs.apply(-7));
		System.out.println(listaVacia.get());
		saludar.run();
		System.out.println(aMayusculas.apply("hola"));
	}
}
```


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Para ordenar por varios criterios, una opción es escribir la comparación manualmente y decidir primero por edad y luego por nombre. Esa versión muestra con claridad la lógica exacta de ordenación, aunque el código resulta más largo y menos reutilizable.

La segunda versión usa `Comparator`, que ya encapsula este tipo de composición de criterios. Con `comparingInt` y `thenComparing` se expresa de forma más declarativa que se quiere ordenar primero por edad y, si hay empate, por nombre alfabéticamente.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Persona {
	private final String nombre;
	private final int edad;

	public Persona(String nombre, int edad) {
		this.nombre = nombre;
		this.edad = edad;
	}

	public String getNombre() {
		return nombre;
	}

	public int getEdad() {
		return edad;
	}

	@Override
	public String toString() {
		return nombre + " (" + edad + ")";
	}
}

public class Main {
	public static void main(String[] args) {
		List<Persona> personas = new ArrayList<>();
		personas.add(new Persona("Luis", 20));
		personas.add(new Persona("Ana", 20));
		personas.add(new Persona("Marta", 18));

		Collections.sort(personas, (p1, p2) -> {
			int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
			if (comparacionEdad != 0) {
				return comparacionEdad;
			}
			return p1.getNombre().compareTo(p2.getNombre());
		});

		System.out.println(personas);

		Collections.sort(personas,
				Comparator.comparingInt(Persona::getEdad)
						  .thenComparing(Persona::getNombre));

		System.out.println(personas);
	}
}
```
