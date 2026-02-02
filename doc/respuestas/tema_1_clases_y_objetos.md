<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas de la programación orientada a objetos son: **encapsulación**, **herencia**, **polimorfismo** y **abstracción**. Estas propiedades permiten organizar el código de manera más modular y reutilizable que en la programación procedural tradicional.

La **encapsulación** consiste en agrupar datos (atributos) y las funciones que operan sobre ellos (métodos) dentro de una misma unidad llamada clase, ocultando los detalles internos de implementación. Similar a cómo en C se pueden usar estructuras con punteros a funciones, pero de forma más natural y segura. Esto permite controlar el acceso a los datos mediante modificadores de visibilidad, protegiendo la integridad de la información.

La **herencia** permite crear nuevas clases basadas en clases existentes, reutilizando y extendiendo su funcionalidad. Una clase hija (subclase) hereda los atributos y métodos de su clase padre (superclase), pudiendo añadir nuevos elementos o modificar los existentes. Esto facilita la reutilización de código y establece relaciones jerárquicas entre tipos de datos.

El **polimorfismo** permite que objetos de diferentes clases respondan al mismo mensaje (llamada a método) de formas distintas. Esto significa que una misma función puede comportarse de manera diferente según el tipo de objeto que la invoque. La **abstracción** consiste en modelar entidades del mundo real identificando sus características y comportamientos esenciales, ignorando los detalles irrelevantes para el problema a resolver. Permite trabajar con conceptos de alto nivel sin preocuparse por los detalles de implementación específicos.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Cuatro lenguajes populares que permiten la programación orientada a objetos son **Java**, **C++**, **Python** y **C#**. Estos lenguajes han sido ampliamente adoptados en la industria del software y soportan los principios fundamentales de la POO, aunque con diferentes enfoques y filosofías.

**Java** es un lenguaje puramente orientado a objetos donde todo (excepto los tipos primitivos) es un objeto, diseñado con portabilidad y seguridad como prioridades. **C++** extiende el lenguaje C añadiendo características de POO, permitiendo combinar programación procedural con orientada a objetos, lo que resulta familiar dado el conocimiento previo en C/C++. **Python** es un lenguaje interpretado que soporta múltiples paradigmas, incluyendo POO, con una sintaxis sencilla y flexible. **C#** es un lenguaje desarrollado por Microsoft, similar a Java en muchos aspectos, muy utilizado en el desarrollo de aplicaciones para el ecosistema Windows y multiplataforma con .NET.

Otros lenguajes populares con soporte para POO incluyen JavaScript, Ruby, PHP y Kotlin, cada uno con sus particularidades pero compartiendo los conceptos fundamentales de clases, objetos, herencia y polimorfismo.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** es un paradigma que organiza el código mediante estructuras de control bien definidas (secuencia, selección e iteración) y evita el uso de sentencias de salto incondicionales como `goto`. Este enfoque, popularizado en las décadas de 1960 y 1970, promueve escribir programas con un flujo de ejecución claro y predecible, dividiendo problemas complejos en funciones o procedimientos más pequeños. En C, este paradigma se manifiesta mediante el uso de `if-else`, `while`, `for` y `switch`, junto con funciones que realizan tareas específicas, facilitando la lectura y el mantenimiento del código.

La **programación modular** lleva la estructuración un paso más allá al organizar el código en módulos o unidades independientes, cada uno con una responsabilidad bien definida. Un módulo agrupa funciones y datos relacionados, exponiendo solo una interfaz pública mientras oculta los detalles de implementación. En C, esto se implementa típicamente mediante archivos de cabecera (`.h`) que declaran la interfaz y archivos de implementación (`.c`) que contienen el código, utilizando `static` para ocultar funciones y variables internas del módulo.

Ambos paradigmas representan evoluciones importantes en la ingeniería del software, buscando mejorar la mantenibilidad y reducir la complejidad. La programación estructurada elimina el "código espagueti" causado por saltos arbitrarios, mientras que la modular promueve la separación de responsabilidades y la reutilización. La POO puede verse como una evolución natural de estos conceptos, donde los módulos se convierten en clases que encapsulan tanto datos como comportamiento, añadiendo además mecanismos como herencia y polimorfismo para mayor flexibilidad y expresividad.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Los tres elementos que definen a un objeto en programación orientada a objetos son: **identidad**, **estado** y **comportamiento**. Estos elementos permiten que un objeto sea una entidad completa y diferenciable dentro de un programa.

La **identidad** es lo que hace único a cada objeto, distinguiéndolo de otros objetos incluso si tienen el mismo estado. En términos prácticos, corresponde a la dirección de memoria donde reside el objeto, similar a cómo un puntero en C identifica una ubicación específica. Dos objetos pueden tener exactamente los mismos valores en sus atributos, pero siguen siendo entidades distintas con identidades diferentes.

El **estado** está determinado por los valores de los atributos o variables de instancia del objeto en un momento dado. Es análogo a los campos de una estructura (`struct`) en C, pero con la diferencia de que cada objeto tiene su propia copia de estos datos. Por ejemplo, si se tiene una clase `Persona`, el estado de un objeto particular podría incluir valores como nombre "Juan", edad 25 y altura 1.75.

El **comportamiento** se define mediante los métodos o funciones que el objeto puede ejecutar, determinando qué operaciones pueden realizarse sobre él o qué acciones puede realizar. Estos métodos pueden consultar o modificar el estado del objeto, e interactuar con otros objetos. A diferencia de las funciones en C que operan sobre estructuras pasadas como parámetros, los métodos están intrínsecamente asociados al objeto y tienen acceso directo a su estado interno.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla o molde que define la estructura y el comportamiento que tendrán los objetos creados a partir de ella. Define qué atributos (datos) y qué métodos (funciones) tendrán los objetos, pero no representa un objeto concreto en sí misma. Es análogo a cómo en C se define un `struct` que especifica los campos que tendrá una estructura, pero la definición del tipo no es lo mismo que una variable de ese tipo. Por ejemplo, una clase `Coche` definiría que todos los coches tienen marca, modelo y velocidad, y pueden acelerar o frenar.

Un **objeto** es una entidad concreta creada a partir de una clase, con valores específicos en sus atributos. No es lo mismo que una clase: la clase es el diseño abstracto, mientras que el objeto es una realización concreta de ese diseño. El término **instancia** es sinónimo de objeto; cuando se crea un objeto a partir de una clase, se dice que se "instancia" la clase. Si `Coche` es la clase, entonces `miCoche` con marca "Toyota", modelo "Corolla" y velocidad actual 60 km/h sería un objeto o instancia de esa clase. Pueden existir múltiples instancias de la misma clase, cada una con su propio estado independiente.

No todos los lenguajes orientados a objetos manejan el concepto de clase. Existen lenguajes basados en **prototipos** como JavaScript (en sus versiones originales) donde los objetos se crean clonando otros objetos existentes que sirven como prototipos, sin necesidad de definir clases explícitamente. Sin embargo, los lenguajes más populares como Java, C++, C# y Python utilizan el modelo basado en clases, donde la clase actúa como el mecanismo fundamental para crear objetos y organizar el código orientado a objetos.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

Los objetos se almacenan típicamente en la **memoria dinámica** o **heap**, una región de memoria destinada a datos cuyo tamaño o tiempo de vida no se conoce en tiempo de compilación. Esto es similar a cuando en C se utiliza `malloc()` o `calloc()` para reservar memoria dinámicamente. A diferencia de las variables locales que se almacenan en la pila (stack) y se destruyen automáticamente al salir de su ámbito, los objetos en el heap persisten hasta que se libera explícita o automáticamente su memoria.

La forma de gestionar esta memoria **no es igual en todos los lenguajes**. En C++, el programador tiene control total y debe liberar manualmente la memoria usando `delete` (similar al `free()` de C), lo que otorga eficiencia pero puede causar fugas de memoria o errores de acceso si no se maneja correctamente. En contraste, lenguajes como Java, C# y Python gestionan la memoria automáticamente mediante un recolector de basura, liberando al programador de esta responsabilidad pero introduciendo cierta sobrecarga en tiempo de ejecución.

La **recolección de basura** (garbage collection) es un mecanismo automático que identifica y libera la memoria ocupada por objetos que ya no son accesibles o utilizables por el programa. El recolector rastrea las referencias a objetos y cuando detecta que ninguna variable o estructura apunta a un objeto determinado, lo marca como candidato para eliminación y eventualmente libera su memoria. Esto elimina problemas comunes de C/C++ como las fugas de memoria (memory leaks) cuando se olvida liberar memoria, o los punteros colgantes (dangling pointers) cuando se accede a memoria ya liberada, aunque a cambio introduce pausas periódicas durante la ejecución del programa cuando el recolector se activa.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un **método** es una función que está asociada a una clase y define el comportamiento de los objetos de esa clase. A diferencia de las funciones tradicionales en C que operan sobre datos pasados como parámetros, un método tiene acceso implícito a los atributos del objeto sobre el cual se invoca. Por ejemplo, si existe un método `acelerar()` en una clase `Coche`, cuando se llama como `miCoche.acelerar()`, ese método opera específicamente sobre los datos de `miCoche` y puede modificar su velocidad. Los métodos son el mecanismo principal mediante el cual los objetos realizan acciones e interactúan con su estado interno y con otros objetos.

La **sobrecarga de métodos** (method overloading) es la capacidad de definir múltiples métodos con el mismo nombre dentro de una clase, siempre que tengan diferentes listas de parámetros. Los métodos se diferencian por el número, tipo u orden de sus parámetros, permitiendo que el compilador determine cuál versión ejecutar según los argumentos proporcionados en la llamada. Por ejemplo, una clase `Calculadora` podría tener varios métodos `sumar()`: uno que acepta dos enteros, otro que acepta tres enteros, y otro que acepta dos números decimales.

Esta característica no existe en C, donde cada función debe tener un nombre único, pero es común en lenguajes orientados a objetos como Java y C++. La sobrecarga mejora la legibilidad del código al permitir usar el mismo nombre conceptual para operaciones similares, en lugar de crear nombres distintos como `sumarDosEnteros()`, `sumarTresEnteros()`, etc. El compilador resuelve automáticamente qué versión del método invocar basándose en los tipos de los argumentos, un proceso conocido como resolución de sobrecarga en tiempo de compilación.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

A continuación se presenta un ejemplo mínimo de una clase `Punto` en Java con dos atributos y un método para calcular la distancia al origen:

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La clase `Punto` define dos atributos de tipo `double` llamados `x` e `y` que representan las coordenadas del punto en un plano cartesiano. El método `calculaDistanciaAOrigen()` utiliza el teorema de Pitágoras para calcular la distancia euclidiana desde el punto hasta el origen (0,0), accediendo directamente a los atributos `x` e `y` del objeto. La función `Math.sqrt()` es equivalente a la función `sqrt()` de la biblioteca matemática de C.

Para utilizar esta clase, es necesario crear una instancia (objeto) mediante el operador `new`, que reserva memoria en el heap y devuelve una referencia al objeto creado, similar a cómo `malloc()` devuelve un puntero en C. A continuación se muestra un ejemplo de uso:

```java
public class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;
        
        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}
```

En este ejemplo se crea un objeto `p` de tipo `Punto`, se asignan valores a sus atributos `x` e `y`, y luego se invoca el método `calculaDistanciaAOrigen()` sobre ese objeto específico. El resultado, que debería ser 5.0, se imprime por consola. La sintaxis de punto (`p.x`, `p.calculaDistanciaAOrigen()`) es similar a cómo se accede a los miembros de un `struct` en C, pero aquí se está trabajando con referencias a objetos en lugar de con estructuras directas o punteros.

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada en un programa Java es el método `main`, que debe tener la siguiente firma exacta: `public static void main(String[] args)`. Este método es el primero que se ejecuta cuando se lanza una aplicación Java, similar a la función `main()` en C/C++. La máquina virtual de Java (JVM) busca este método específico para iniciar la ejecución del programa, y debe estar declarado dentro de alguna clase pública que coincida con el nombre del archivo.

La palabra clave `static` indica que un método o atributo pertenece a la **clase en sí** y no a instancias particulares de esa clase. Esto significa que se puede acceder a elementos estáticos sin necesidad de crear un objeto, llamándolos directamente a través del nombre de la clase. Es análogo al concepto de variables y funciones globales en C, pero organizadas dentro del contexto de una clase. Por ejemplo, `Math.sqrt()` es un método estático porque no tiene sentido crear un objeto `Math` para calcular una raíz cuadrada; la operación no depende del estado de ningún objeto particular. El método `main` debe ser estático precisamente porque la JVM lo invoca sin crear ninguna instancia de la clase que lo contiene.

El uso de `static` **no se limita al método `main`**; puede aplicarse a métodos y atributos en cualquier clase. Los métodos estáticos son útiles para operaciones utilitarias que no requieren acceso al estado de un objeto, como funciones matemáticas o conversiones. Los atributos estáticos (también llamados variables de clase) son compartidos por todas las instancias de una clase, existiendo una única copia en memoria independientemente de cuántos objetos se creen. Por ejemplo, un contador de instancias creadas podría ser un atributo estático.

La combinación `static final` se utiliza para declarar **constantes** a nivel de clase. `final` indica que el valor no puede modificarse tras su inicialización, similar a `const` en C/C++. Cuando se combinan, se crea una constante global accesible desde cualquier lugar mediante el nombre de la clase, como `Math.PI` que es `public static final double PI = 3.141592653589793`. Esta combinación es muy común para definir valores configurables o constantes matemáticas que deben ser inmutables y compartidas por todo el programa.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Voy a crear un programa Java básico y mostrarte cómo compilarlo y ejecutarlo desde línea de comandos.

```java
// Archivo: HolaMundo.java
public class HolaMundo {
    public static void main(String[] args) {
        System.out.println("Hola desde Java!");
    }
}
```

Para compilar y ejecutar este programa, se utilizan dos comandos separados. Primero, `javac HolaMundo.java` compila el código fuente y genera un archivo `HolaMundo.class`. Luego, `java HolaMundo` (sin la extensión .class) ejecuta el programa. Es importante que el nombre de la clase pública coincida exactamente con el nombre del archivo, incluyendo mayúsculas y minúsculas.

Java es un lenguaje **híbrido entre compilado e interpretado**. A diferencia de C/C++ que compilan directamente a código máquina específico del procesador, Java compila a un formato intermedio llamado **bytecode**. Este bytecode es un conjunto de instrucciones independientes de la plataforma que se almacena en los archivos `.class`. El bytecode no es directamente ejecutable por el procesador, sino que requiere una capa adicional de traducción.

La **máquina virtual de Java (JVM)** es el software que ejecuta el bytecode, actuando como un intermediario entre el código compilado y el hardware real. La JVM interpreta o compila dinámicamente (JIT - Just In Time compilation) el bytecode a código máquina nativo durante la ejecución. Este diseño permite el lema "write once, run anywhere": el mismo archivo `.class` puede ejecutarse en Windows, Linux o cualquier sistema que tenga una JVM compatible, sin necesidad de recompilar. En contraste, un ejecutable compilado en C para Windows no funcionaría directamente en Linux.

El proceso completo es: código fuente `.java` → compilador `javac` → bytecode en `.class` → JVM interpreta/compila → ejecución en el procesador. Esto añade una capa de abstracción que sacrifica algo de rendimiento respecto a lenguajes compilados nativamente, pero gana portabilidad, seguridad (la JVM puede verificar el bytecode antes de ejecutarlo) y características como la recolección automática de basura.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

El operador `new` es el mecanismo en Java para crear objetos, reservando memoria en el heap para una nueva instancia de una clase. Es análogo a la función `malloc()` en C, pero más seguro y automatizado: `new` no solo reserva la memoria necesaria, sino que también inicializa el objeto llamando a su constructor y devuelve una referencia (similar a un puntero) al objeto creado. Sin `new`, no se pueden crear objetos en Java, solo se pueden declarar referencias que inicialmente valen `null`.

Un **constructor** es un método especial que se ejecuta automáticamente cuando se crea un objeto con `new`, y su propósito es inicializar el estado del objeto recién creado. Los constructores tienen el mismo nombre que la clase, no especifican tipo de retorno (ni siquiera `void`), y pueden recibir parámetros para configurar el objeto durante su creación. Si no se define ningún constructor explícitamente, Java proporciona automáticamente un constructor sin parámetros que inicializa los atributos a valores por defecto (0 para números, `null` para referencias).

A continuación se presenta un ejemplo de la clase `Empleado` con un constructor que inicializa sus tres atributos:

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;
    
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Para usar esta clase, se crearía un objeto proporcionando los valores requeridos: `Empleado emp = new Empleado("12345678A", "Juan", "García López")`. La palabra clave `this` se utiliza dentro del constructor para diferenciar entre los parámetros y los atributos del objeto cuando tienen el mismo nombre, siendo `this.dni` el atributo del objeto y `dni` el parámetro recibido. Esto es necesario porque los parámetros "ocultan" temporalmente los atributos en el ámbito del constructor, similar al concepto de shadowing en C.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia `this` es una palabra clave que dentro de un método o constructor representa una referencia al objeto actual sobre el cual se está ejecutando ese método. Es similar a un puntero implícito al propio objeto, permitiendo acceder explícitamente a sus atributos y métodos. `this` resulta especialmente útil cuando existe ambigüedad entre nombres de parámetros y atributos, o cuando se necesita pasar el objeto actual como argumento a otro método.

La palabra clave `this` **no se llama igual en todos los lenguajes** orientados a objetos. En Python se utiliza convencionalmente `self` (aunque técnicamente puede tener cualquier nombre), en C++ también se usa `this` pero es un puntero explícito, en JavaScript se emplea `this` aunque con semántica diferente según el contexto, y en algunos lenguajes como Ruby se usa `@` como prefijo para las variables de instancia. La idea conceptual es la misma en todos: referenciar al objeto actual.

A continuación se muestra un ejemplo del uso de `this` en la clase `Punto`, tanto en el constructor como en un método:

```java
class Punto {
    double x;
    double y;
    
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
    
    double calculaDistanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En el constructor, `this.x` y `this.y` se refieren a los atributos del objeto, mientras que `x` e `y` son los parámetros recibidos. En el método `calculaDistanciaA()`, `this.x` representa la coordenada x del punto sobre el cual se invoca el método, diferenciándolo de `otro.x` que es la coordenada del punto pasado como parámetro. Aunque en `calculaDistanciaAOrigen()` el uso de `this` es opcional (podría escribirse simplemente `x` e `y`), utilizarlo explícitamente mejora la claridad del código al dejar claro que se están usando atributos del objeto.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

A continuación se presenta la clase `Punto` completa con el nuevo método `distanciaA` que calcula la distancia entre dos puntos:

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

El método `distanciaA` recibe como parámetro otro objeto de tipo `Punto` y calcula la distancia euclidiana entre el punto actual (`this`) y el punto proporcionado (`otro`). La fórmula utiliza el teorema de Pitágoras calculando primero las diferencias en cada coordenada: `dx` representa la distancia horizontal y `dy` la distancia vertical entre ambos puntos.

En este método, `this.x` y `this.y` se refieren a las coordenadas del punto sobre el cual se invoca el método, mientras que `otro.x` y `otro.y` son las coordenadas del punto pasado como argumento. Por ejemplo, si se tiene `p1.distanciaA(p2)`, dentro del método `this` representa a `p1` y `otro` representa a `p2`. Un ejemplo de uso sería:

```java
Punto p1 = new Punto();
p1.x = 0.0;
p1.y = 0.0;

Punto p2 = new Punto();
p2.x = 3.0;
p2.y = 4.0;

double dist = p1.distanciaA(p2);  // Resultado: 5.0
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, el paso de parámetros siempre es **por valor**, pero es crucial entender qué se está copiando. Para objetos como `Punto`, lo que se pasa es una **copia de la referencia** (similar a copiar un puntero en C), no una copia del objeto completo. Esto significa que si dentro del método se modifican los atributos del objeto referenciado mediante esa referencia, **los cambios sí afectan al objeto original** fuera del método, porque tanto el parámetro como la variable externa apuntan al mismo objeto en memoria.

```java
void modificaPunto(Punto p) {
    p.x = 100.0;  // Esto SÍ modifica el objeto original
    p.y = 200.0;  // Esto SÍ modifica el objeto original
}

// Uso:
Punto miPunto = new Punto();
miPunto.x = 5.0;
miPunto.y = 10.0;
modificaPunto(miPunto);
// Ahora miPunto.x vale 100.0 y miPunto.y vale 200.0
```

Sin embargo, si se intenta reasignar completamente el parámetro a un nuevo objeto con `p = new Punto()`, este cambio **no afecta** a la variable original porque solo se está modificando la copia local de la referencia, no la referencia externa. Es equivalente a modificar una copia de un puntero en C: se puede seguir el puntero y modificar lo que apunta, pero cambiar el puntero mismo no afecta al puntero original.

Para tipos primitivos como `int`, el comportamiento es diferente. Los tipos primitivos se pasan **por valor puro**: se copia el valor numérico completo. Cualquier modificación del parámetro dentro del método **no afecta** a la variable original fuera del método:

```java
void modificaEntero(int n) {
    n = 999;  // Esto NO afecta al valor original
}

// Uso:
int numero = 42;
modificaEntero(numero);
// numero sigue valiendo 42
```

En resumen: para objetos, se puede modificar su contenido interno a través del parámetro (porque se comparte la misma dirección de memoria), pero para tipos primitivos, las modificaciones son completamente locales al método. Este comportamiento es consistente con C cuando se pasan estructuras por puntero (se puede modificar el contenido) versus pasar valores escalares directamente (no se puede modificar el original).

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método `toString()` es un método especial en Java que devuelve una representación en forma de cadena de texto (`String`) de un objeto. Este método se invoca automáticamente cuando se intenta imprimir un objeto con `System.out.println()`, cuando se concatena un objeto con una cadena usando el operador `+`, o cuando se necesita convertir explícitamente un objeto a texto. Si no se define un `toString()` personalizado, Java utiliza una implementación por defecto que muestra el nombre de la clase seguido de un código hash en formato hexadecimal, algo poco informativo como `Punto@15db9742`.

Este concepto **existe en otros lenguajes orientados a objetos** aunque con nombres diferentes. En Python se llama `__str__()` o `__repr__()`, en C# se llama `ToString()` (con mayúscula inicial), en JavaScript es `toString()`, y en Ruby es `to_s`. La idea subyacente es la misma: proporcionar una representación textual legible del objeto que sea útil para depuración, logging o presentación al usuario.

A continuación se muestra un ejemplo de `toString()` implementado en la clase `Punto`:

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}
```

Con este método definido, cuando se ejecuta `System.out.println(miPunto)` donde `miPunto` tiene coordenadas (3.0, 4.0), la salida será `Punto(3.0, 4.0)` en lugar del código hash críptico. El método debe declararse como `public` y devolver un tipo `String`. La anotación `@Override` (opcional pero recomendada) indica que se está sobrescribiendo un método heredado de la clase base `Object`, concepto que se verá al estudiar herencia.

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta

Una clase en Java tiene similitudes con un `struct` en C en el sentido de que ambos permiten agrupar datos relacionados bajo un mismo tipo definido por el programador. Ambos definen una plantilla que especifica qué campos o atributos contendrá cada variable de ese tipo. Sin embargo, un `struct` en C es fundamentalmente una estructura de datos pasiva que solo agrupa variables, mientras que una clase es una entidad más completa que combina datos y comportamiento en una unidad cohesiva.

A un `struct` de C le faltan varios elementos fundamentales para ser equivalente a una clase. Primero, **no tiene métodos asociados**: las funciones que operan sobre un `struct` se definen externamente y reciben la estructura como parámetro explícito, en lugar de estar intrínsecamente vinculadas al tipo. Segundo, **carece de constructores**: no existe un mecanismo automático de inicialización cuando se declara una variable del tipo `struct`, siendo responsabilidad del programador inicializar manualmente cada campo o crear funciones de inicialización por convención. Tercero, **no tiene control de visibilidad**: todos los campos son públicos y accesibles desde cualquier parte del código, imposibilitando la encapsulación que permite ocultar detalles de implementación.

Además, los `struct` de C no soportan mecanismos de POO como **herencia** (extender una estructura existente añadiendo nuevos campos y comportamientos), **polimorfismo** (diferentes tipos respondiendo de formas distintas a la misma interfaz), ni **destructores automáticos** que se ejecuten al terminar el ciclo de vida de la variable. Tampoco gestionan automáticamente la memoria dinámica ni proporcionan recolección de basura. En C se trabaja directamente con valores de estructura en la pila o manualmente con punteros a memoria dinámica, mientras que en Java las variables de tipo clase son automáticamente referencias a objetos gestionados en el heap.

C++ evolucionó precisamente añadiendo estas capacidades a los `struct`: en C++ pueden tener métodos, constructores, destructores, herencia y modificadores de acceso, siendo funcionalmente equivalentes a las `class` (con la única diferencia de que los miembros son públicos por defecto en `struct` y privados en `class`). Esto demuestra cómo la POO extiende el concepto de agrupación de datos hacia una integración completa de datos y comportamiento con mecanismos adicionales de abstracción y reutilización.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para emular la clase `Punto` en C usando estructuras, es necesario separar los datos del comportamiento. La estructura contendrá solo los atributos, mientras que las funciones operarán sobre esa estructura recibiéndola como parámetro explícito. A continuación se muestra cómo se implementaría:

```c
#include <math.h>
#include <stdio.h>

struct Punto {
    double x;
    double y;
};

double calculaDistanciaAOrigen(struct Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

double distanciaA(struct Punto* p1, struct Punto* p2) {
    double dx = p1->x - p2->x;
    double dy = p1->y - p2->y;
    return sqrt(dx * dx + dy * dy);
}

int main() {
    struct Punto miPunto;
    miPunto.x = 3.0;
    miPunto.y = 4.0;
    
    double dist = calculaDistanciaAOrigen(&miPunto);
    printf("Distancia al origen: %f\n", dist);
    
    return 0;
}
```

La diferencia fundamental es que en C las funciones están **desacopladas** de la estructura: no hay una asociación sintáctica entre `struct Punto` y las funciones que operan sobre ella. Las funciones reciben explícitamente un puntero a la estructura como primer parámetro, que debe pasarse manualmente en cada llamada. En Java, la sintaxis `miPunto.calculaDistanciaAOrigen()` oculta este detalle, pero internamente es equivalente a pasar el objeto como parámetro implícito.

**¿Qué ha pasado con `this`?** En la versión de C, `this` se ha convertido en el parámetro explícito `p` (o `p1` en el caso de `distanciaA`). Mientras que en Java `this` es una referencia implícita automáticamente disponible dentro de cualquier método, en C ese "puntero al objeto actual" debe declararse explícitamente como parámetro y pasarse en cada llamada. Por ejemplo, `p1.distanciaA(p2)` en Java se traduce a `distanciaA(&p1, &p2)` en C, donde `&p1` se convierte en lo que sería `this` dentro de la función.

Esta comparación revela que la POO no introduce magia incomprensible, sino que proporciona azúcar sintáctico y abstracciones que hacen el código más intuitivo y mantenible. Lo que en Java es implícito y automático (`this`, la asociación método-clase, la gestión de memoria), en C debe gestionarse manualmente, pero el concepto subyacente es el mismo: funciones que operan sobre datos estructurados.