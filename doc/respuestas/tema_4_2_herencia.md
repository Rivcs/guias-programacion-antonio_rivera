<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo de la orientación a objetos que permite definir una clase nueva a partir de otra ya existente, estableciendo una relación del tipo "A es-un B". A diferencia de la composición, que expresa "tiene-un", la herencia indica que el subtipo es una versión más concreta del supertipo. Por ejemplo, un `Artillero` es un `Soldado` y un `Zapador` también es un `Soldado`. En Java, esta relación se expresa con la palabra clave `extends`.

La primera implicación importante es la **compatibilidad de tipos**: si `Artillero` extiende a `Soldado`, entonces cualquier referencia de tipo `Soldado` puede apuntar a un objeto real de tipo `Artillero`. Esto permite, por ejemplo, crear un array de `Soldado` y almacenar en él objetos de cualquier subtipo. Al recorrer ese array se puede invocar `saludar()` sobre cada elemento sin necesidad de saber su tipo concreto.

La segunda implicación es la **herencia de estado y comportamiento**: la subclase recibe automáticamente los atributos y métodos de la superclase. El `Artillero` y el `Zapador` no necesitan declarar de nuevo el atributo `nombre` ni el método `saludar()`, porque los heredan de `Soldado`. Cada subclase solo necesita añadir lo que le es propio: el número de cohetes o el número de minas, respectivamente, junto con sus métodos específicos.

```java
public class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Soy el soldado " + nombre);
	}
}

public class Artillero extends Soldado {
	private int cohetes;

	public Artillero(String nombre, int cohetes) {
		super(nombre);
		this.cohetes = cohetes;
	}

	public int getCohetes() {
		return cohetes;
	}

	public void dispararCohete() {
		System.out.println("¡Cohete disparado!");
	}
}

public class Zapador extends Soldado {
	private int minas;

	public Zapador(String nombre, int minas) {
		super(nombre);
		this.minas = minas;
	}

	public int getMinas() {
		return minas;
	}

	public void ponerMina() {
		System.out.println("Mina colocada.");
	}
}

public class Main {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Carlos", 5),
			new Zapador("Elena", 3),
			new Artillero("Luis", 8),
			new Zapador("Marta", 2)
		};

		for (Soldado soldado : peloton) {
			soldado.saludar();
		}
	}
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre?

### Respuesta

Cuando se crea un objeto de una subclase, se ejecutan al menos dos constructores: primero el de la superclase y después el de la subclase. Si hubiera una cadena de herencia más larga (por ejemplo, `A` → `B` → `C`), se ejecutarían en orden desde la clase más alta hasta la más baja. Así, al hacer `new Artillero("Carlos", 5)`, primero se ejecuta el constructor de `Soldado` y después el de `Artillero`.

La llamada `super(...)` dentro de un constructor es la forma de invocar explícitamente al constructor de la superclase. En el ejemplo, `super(nombre)` le pasa el nombre al constructor de `Soldado`, que es el encargado de inicializar el atributo `nombre`. Esta llamada debe ser siempre la primera instrucción del constructor de la subclase; no se puede colocar después de otras sentencias.

Si la clase base no dispone de un constructor sin parámetros visible (porque se ha definido algún constructor con parámetros y no se ha escrito explícitamente el constructor vacío), entonces es obligatorio llamar a `super(...)` con los argumentos adecuados. En caso contrario, el compilador intentará insertar automáticamente `super()` sin argumentos y, al no encontrar un constructor sin parámetros en la superclase, producirá un error de compilación.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando se crea un objeto `Artillero`, ese objeto contiene en su interior tanto el atributo `nombre` heredado de `Soldado` como el atributo `cohetes` propio de `Artillero`. Ambos están presentes en el mismo bloque de memoria del objeto.

Sin embargo, que estén presentes en memoria no significa que sean accesibles desde el código de la subclase. El modificador `private` impide que cualquier clase distinta de la que declaró el atributo pueda referenciarlo directamente. Así, dentro del código de `Artillero` no se puede escribir `this.nombre` porque `nombre` es privado de `Soldado`. Para acceder a ese dato habría que hacerlo a través de métodos públicos o protegidos que `Soldado` ofrezca, como un getter.

Esta distinción es importante porque separa dos conceptos: la **estructura física** del objeto (qué datos ocupa en memoria) y la **visibilidad** del código (qué se puede referenciar desde cada clase). La herencia aporta estructura, pero la encapsulación sigue controlando el acceso. Gracias a esto, la superclase puede cambiar la forma en que gestiona sus atributos internos sin que las subclases se vean afectadas, siempre que mantenga su interfaz pública.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos hace que el código que trabaja con el supertipo sea extensible sin necesidad de modificación. Si un método o un bucle recorre un array de `Soldado` y llama a `saludar()`, ese código no necesita saber cuántos tipos concretos de soldado existen. Al añadir un nuevo subtipo, basta con crearlo y meterlo en el array; el bucle que recorre y saluda sigue funcionando exactamente igual.

Esto es muy relevante desde el punto de vista de la mantenibilidad del software. En C, si se quisiera añadir un nuevo tipo de soldado a una función que recorre un array, probablemente habría que modificar dicha función añadiendo nuevos `if` o `switch`. Con herencia y compatibilidad de tipos, el código existente permanece intacto, lo cual reduce el riesgo de introducir errores al extender el sistema.

A continuación se añade un nuevo tipo `Medico` que hereda de `Soldado`. El `main` anterior no necesita cambiar su bucle de saludo; solo se añade un nuevo elemento al array:

```java
public class Medico extends Soldado {
	private int botiquines;

	public Medico(String nombre, int botiquines) {
		super(nombre);
		this.botiquines = botiquines;
	}

	public int getBotiquines() {
		return botiquines;
	}

	public void curar() {
		if (botiquines > 0) {
			botiquines--;
			System.out.println("Soldado curado.");
		} else {
			System.out.println("No quedan botiquines.");
		}
	}
}

public class Main {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Carlos", 5),
			new Zapador("Elena", 3),
			new Medico("Ana", 10)
		};

		// Este bucle no ha cambiado respecto al ejemplo anterior
		for (Soldado soldado : peloton) {
			soldado.saludar();
		}
	}
}
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, se puede tener una referencia del supertipo que apunte a un objeto real de un subtipo. Escribir `Soldado s = new Artillero("Carlos", 5)` es perfectamente válido. Sin embargo, a través de esa referencia de tipo `Soldado` solo se pueden invocar los métodos definidos en `Soldado`; no se puede llamar directamente a `getCohetes()` ni a `dispararCohete()`, porque el compilador solo conoce el tipo declarado de la referencia, no el tipo real del objeto.

El **upcasting** es la conversión de una referencia de subtipo a una referencia de supertipo. Es automática e implícita en Java: al asignar un `Artillero` a una variable de tipo `Soldado` se está haciendo upcasting sin necesidad de ningún operador especial. El **downcasting** es la operación inversa: convertir una referencia de supertipo a una de subtipo. Esta conversión debe hacerse de forma explícita con un cast, por ejemplo `(Artillero) soldado`. Si el objeto real no es del tipo indicado, se lanzará una `ClassCastException` en tiempo de ejecución.

Para evitar ese riesgo, Java proporciona el operador `instanceof`, que permite comprobar en tiempo de ejecución si un objeto es de un tipo concreto antes de hacer el downcasting. De este modo se puede recorrer un array de `Soldado` con seguridad, preguntando a cada elemento si realmente es un `Artillero` antes de intentar acceder a sus métodos específicos.

```java
public class Main {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Carlos", 5),
			new Zapador("Elena", 3),
			new Artillero("Luis", 8)
		};

		for (Soldado soldado : peloton) {
			soldado.saludar();

			if (soldado instanceof Artillero artillero) { // Versión moderna de downcasting (Java +16)
				System.out.println("  Cohetes: " + artillero.getCohetes());
			}
		}
	}
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido es un nivel de visibilidad intermedio entre `private` y `public`. Un miembro marcado como `protected` es accesible desde la propia clase, desde las subclases (aunque estén en otro paquete) y también desde cualquier clase del mismo paquete. En Java se implementa colocando la palabra clave `protected` delante del atributo o método.

Este modificador resulta útil en herencia cuando se desea que las subclases puedan acceder directamente a ciertos miembros de la superclase sin necesidad de pasar por getters o setters públicos, pero sin exponerlos al resto del código exterior. Es un compromiso: se gana comodidad en las subclases a cambio de relajar ligeramente la encapsulación, ya que las subclases pasan a depender de un detalle interno de la superclase.

En el siguiente ejemplo, se declara `nombre` como `protected` en `Soldado`. Gracias a ello, el `Zapador` puede usar `nombre` directamente dentro de su método `ponerMina()` sin necesidad de invocar ningún getter:

```java
public class Soldado {
	protected String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Soy el soldado " + nombre);
	}
}

public class Zapador extends Soldado {
	private int minas;

	public Zapador(String nombre, int minas) {
		super(nombre);
		this.minas = minas;
	}

	public int getMinas() {
		return minas;
	}

	public void ponerMina() {
		System.out.println(nombre + " ha colocado una mina. Quedan " + (minas - 1));
		minas--;
	}
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En muchos lenguajes orientados a objetos existe una clase raíz de la que heredan, directa o indirectamente, todas las demás clases. Sin embargo, no es una regla universal. Algunos lenguajes como C++ no imponen una clase base común obligatoria: se puede diseñar una jerarquía completamente independiente sin que herede de ninguna clase predefinida.

En Java sí existe esa clase base universal y se llama `Object`. Toda clase que no declare explícitamente `extends` de otra clase hereda automáticamente de `Object`. Esto significa que cualquier objeto en Java, sin excepción, es compatible con el tipo `Object`. Por ejemplo, se puede declarar un array de tipo `Object[]` y almacenar en él cualquier tipo de objeto.

`Object` proporciona un conjunto de métodos que todas las clases heredan, como `toString()` (que devuelve una representación en texto del objeto), `equals(Object)` (que permite comparar dos objetos) y `hashCode()` (que devuelve un valor hash). Estos métodos tienen implementaciones por defecto que pueden ser suficientes o que las subclases pueden redefinir para adaptarlos a sus necesidades concretas. Gracias a esta clase raíz, Java garantiza un contrato mínimo común para todos los objetos del sistema.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple consiste en que una clase pueda heredar de dos o más superclases a la vez. Por ejemplo, si existiera una clase `Estudiante` y una clase `Trabajador`, la herencia múltiple permitiría crear una clase `EstudianteTrabajador` que heredase de ambas simultáneamente. Algunos lenguajes, como C++, permiten esta posibilidad.

El problema principal de la herencia múltiple es el llamado "problema del diamante": si dos superclases heredan de una misma clase base y ambas modifican un mismo método, la subclase que hereda de las dos no sabe cuál de las dos versiones debe utilizar. Esto genera ambigüedades difíciles de resolver y hace que el diseño del lenguaje y del compilador se complique considerablemente.

En Java no existe herencia múltiple de clases. Una clase solo puede extender a una única superclase mediante `extends`. Sin embargo, Java permite que una clase implemente múltiples interfaces (con `implements`), lo cual ofrece una forma limitada de herencia múltiple. Las interfaces definen contratos (métodos que la clase debe implementar) pero tradicionalmente no aportan estado ni implementación completa, lo que evita los problemas del diamante. A partir de Java 8, las interfaces pueden incluir métodos con implementación por defecto (`default`), pero el lenguaje establece reglas claras de resolución de conflictos si dos interfaces ofrecen el mismo método.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea _no controlada_ y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente.

### Respuesta

Las excepciones en Java son objetos y, como tales, pueden usar la herencia para crear tipos de excepción personalizados. Para que una excepción sea no controlada (es decir, que no obligue a quien la llama a capturarla o declararla con `throws`), debe heredar de `RuntimeException`. Esto se hace simplemente con `extends RuntimeException`.

Además de heredar el comportamiento estándar de las excepciones, se puede componer la excepción personalizada con otros objetos. En este caso, `UsuarioNoEncontradoException` contiene una referencia al `Usuario` que causó el problema. De ese modo, quien capture la excepción puede consultar directamente qué usuario no fue encontrado. La sobrecarga de constructores permite crear la excepción con o sin una causa subyacente (`Throwable`), útil cuando el error se produce como consecuencia de otra excepción.

Este ejemplo ilustra cómo herencia y composición se combinan dentro de una misma clase: se hereda de `RuntimeException` para obtener el comportamiento de excepción no controlada y se compone con `Usuario` para transportar información adicional sobre el contexto del error.

```java
public class Usuario {
	private final String nombre;

	public Usuario(String nombre) {
		if (nombre == null || nombre.isBlank()) {
			throw new IllegalArgumentException("El nombre del usuario es obligatorio");
		}
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}
}

public class UsuarioNoEncontradoException extends RuntimeException {
	private final Usuario usuario;

	public UsuarioNoEncontradoException(Usuario usuario) {
		super("Usuario no encontrado: " + usuario.getNombre());
		this.usuario = usuario;
	}

	public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
		super("Usuario no encontrado: " + usuario.getNombre(), causa);
		this.usuario = usuario;
	}

	public Usuario getUsuario() {
		return usuario;
	}
}

public class Main {
	public static void main(String[] args) {
		Usuario u = new Usuario("Pepe");

		try {
			throw new UsuarioNoEncontradoException(u);
		} catch (UsuarioNoEncontradoException e) {
			System.out.println(e.getMessage());
			System.out.println("Usuario afectado: " + e.getUsuario().getNombre());
		}
	}
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia no es un mecanismo de reutilización de código de propósito general, sino un mecanismo para modelar relaciones "es-un" entre tipos. Si se hereda de una clase solo porque contiene métodos útiles que se quieren reutilizar, pero no existe una relación conceptual real entre las dos clases, se está creando un acoplamiento artificial. La subclase pasa a depender de toda la interfaz pública de la superclase, incluyendo métodos que quizá no tengan sentido para ella.

Por ejemplo, si se tuviera una clase `Utilidades` con varios métodos de cálculo y se quisieran usar desde una clase `Factura`, no tendría sentido que `Factura` extienda a `Utilidades` solo para acceder a esos métodos. Decir que "una factura es una utilidad" no tiene significado real en el dominio del problema. Además, `Factura` heredaría toda la interfaz pública de `Utilidades`, exponiendo métodos que no le corresponden.

La composición es más adecuada para reutilizar código sin establecer una relación de tipos: basta con tener una referencia a un objeto de la clase que ofrece la funcionalidad y delegar en él. De este modo, se reutiliza el comportamiento sin asumir la identidad del otro tipo ni exponerse a los cambios internos de una superclase que no representa lo que la clase realmente es.

## 11. Herencia vs. Composición. Se dice que se debe _"favorecer la composición frente a la herencia"_, ¿por qué?

### Respuesta

Se suele recomendar favorecer la composición frente a la herencia porque la composición establece una relación más flexible y menos acoplada entre clases. Con composición, un objeto contiene una referencia a otro y delega en él. Si en el futuro se necesita cambiar el comportamiento, basta con sustituir el objeto interno por otro diferente, incluso en tiempo de ejecución. Con herencia, la relación queda fijada en la propia definición de la clase y no se puede cambiar una vez compilada.

La herencia produce un acoplamiento fuerte entre la subclase y la superclase. Cualquier cambio interno en la superclase puede afectar a todas las subclases, incluso si no se modifica su interfaz pública. Esto hace que las jerarquías de herencia profundas sean frágiles: un pequeño cambio en una clase alta de la jerarquía puede tener repercusiones inesperadas en clases que están varios niveles por debajo.

Esto no significa que la herencia sea mala ni que deba evitarse siempre. La herencia tiene su lugar cuando la relación "es-un" es genuina y se desea la compatibilidad de tipos. Lo que se busca al favorecer la composición es que, ante la duda, se opte por la alternativa que genera menos acoplamiento y ofrece mayor flexibilidad, recurriendo a la herencia solo cuando la relación entre los tipos lo justifique de forma clara.

## 12. Herencia vs. Composición. Se dice que la _"herencia rompe la encapsulación"_, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que la herencia rompe la encapsulación, se hace referencia a que la subclase puede quedar expuesta a los detalles internos de la superclase. Aunque los atributos privados no sean directamente accesibles, la subclase depende de cómo la superclase implementa sus métodos. Si la superclase cambia la lógica interna de un método que la subclase utiliza o sobreescribe, el comportamiento de la subclase puede verse alterado sin que se haya tocado su propio código.

Por ejemplo, si una superclase tiene un método `agregar()` que internamente llama a otro método `validar()`, y la subclase sobreescribe `validar()`, el comportamiento resultante depende de cómo estén conectados internamente esos dos métodos en la superclase. Si la superclase cambia y deja de llamar a `validar()` desde `agregar()`, la subclase se rompe sin explicación aparente. La subclase necesitaba conocer un detalle de implementación de la superclase para funcionar correctamente.

Con composición, este problema no se da de la misma forma. El objeto que se contiene se usa a través de su interfaz pública, y su implementación interna queda oculta. No hay dependencia de cómo están conectados internamente los métodos del objeto contenido, porque simplemente se llama a sus métodos públicos y se trabaja con sus resultados. Por eso se dice que la composición respeta mejor la encapsulación que la herencia.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

La alternativa por herencia consiste en crear una superclase `Persona` con el DNI y el nombre, y hacer que `Estudiante` y `Trabajador` hereden de ella. Así, ambos son una `Persona` y heredan esos atributos comunes. Esta solución es sencilla y permite la compatibilidad de tipos: se puede tratar a un `Estudiante` o a un `Trabajador` como si fuera una `Persona`.

La alternativa por composición consiste en crear una clase `DatosPersonales` que encapsule el DNI y el nombre, y que tanto `Estudiante` como `Trabajador` reciban una instancia de `DatosPersonales` en su constructor. En este caso no hay relación "es-un" entre las clases, sino "tiene-un": un estudiante tiene unos datos personales. La ventaja es que los datos personales podrían reutilizarse en cualquier otro contexto sin imponer una jerarquía de tipos.

Ambas soluciones son válidas. La herencia es más adecuada si se necesita tratar a estudiantes y trabajadores como personas a nivel de tipos. La composición es preferible si la relación no es realmente de identidad, o si se quiere mayor flexibilidad, como que una misma instancia de `DatosPersonales` pueda ser compartida o cambiada sin depender de una jerarquía de clases.

```java
// ──── Alternativa 1: Herencia ────

public class Persona {
	private final String dni;
	private final String nombre;

	public Persona(String dni, String nombre) {
		this.dni = dni;
		this.nombre = nombre;
	}

	public String getDni() {
		return dni;
	}

	public String getNombre() {
		return nombre;
	}
}

public class Estudiante extends Persona {
	private final String grado;

	public Estudiante(String dni, String nombre, String grado) {
		super(dni, nombre);
		this.grado = grado;
	}

	public String getGrado() {
		return grado;
	}
}

public class Trabajador extends Persona {
	private final String empresa;

	public Trabajador(String dni, String nombre, String empresa) {
		super(dni, nombre);
		this.empresa = empresa;
	}

	public String getEmpresa() {
		return empresa;
	}
}


// ──── Alternativa 2: Composición ────

public class DatosPersonales {
	private final String dni;
	private final String nombre;

	public DatosPersonales(String dni, String nombre) {
		this.dni = dni;
		this.nombre = nombre;
	}

	public String getDni() {
		return dni;
	}

	public String getNombre() {
		return nombre;
	}
}

public class Estudiante {
	private final DatosPersonales datos;
	private final String grado;

	public Estudiante(DatosPersonales datos, String grado) {
		if (datos == null) {
			throw new IllegalArgumentException("Los datos personales son obligatorios");
		}
		this.datos = datos;
		this.grado = grado;
	}

	public DatosPersonales getDatos() {
		return datos;
	}

	public String getGrado() {
		return grado;
	}
}

public class Trabajador {
	private final DatosPersonales datos;
	private final String empresa;

	public Trabajador(DatosPersonales datos, String empresa) {
		if (datos == null) {
			throw new IllegalArgumentException("Los datos personales son obligatorios");
		}
		this.datos = datos;
		this.empresa = empresa;
	}

	public DatosPersonales getDatos() {
		return datos;
	}

	public String getEmpresa() {
		return empresa;
	}
}
```
