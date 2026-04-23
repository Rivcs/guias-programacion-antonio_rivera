<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En C, el puntero `void*` permite apuntar a cualquier tipo de dato sin comprometerse con un tipo concreto. Se puede construir una estructura que contenga un array de `void*` y un contador de elementos, de modo que sea posible almacenar punteros a enteros, cadenas o cualquier otro tipo en las distintas posiciones del array. El problema es que, al extraer un elemento, es responsabilidad del programador hacer el cast correcto al tipo original, ya que el compilador no realiza ninguna comprobación.

En Java, la clase `Object` cumple un papel análogo: al ser la raíz de la jerarquía de clases, cualquier objeto es compatible con una referencia de tipo `Object`. Se puede crear una clase que internamente gestione un array de `Object` y ofrezca métodos para añadir y obtener elementos. Gracias al autoboxing, incluso valores primitivos como `int` o `double` se envuelven automáticamente en sus clases wrapper (`Integer`, `Double`) al almacenarse. Al recuperar un elemento, es necesario hacer un downcasting explícito al tipo esperado.

```c
// Versión en C con void*
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    void** datos;
    int tamanho;
    int capacidad;
} Contenedor;

Contenedor* crear(int capacidad) {
    Contenedor* c = malloc(sizeof(Contenedor));
    c->datos = malloc(sizeof(void*) * capacidad);
    c->tamanho = 0;
    c->capacidad = capacidad;
    return c;
}

void anhadir(Contenedor* c, void* elem) {
    if (c->tamanho < c->capacidad) {
        c->datos[c->tamanho++] = elem;
    }
}

void* obtener(Contenedor* c, int indice) {
    return c->datos[indice];
}
```

```java
// Versión en Java con Object
public class Contenedor {
    private Object[] datos;
    private int tamanho;

    public Contenedor(int capacidad) {
        datos = new Object[capacidad];
        tamanho = 0;
    }

    public void anhadir(Object elem) {
        if (tamanho < datos.length) {
            datos[tamanho++] = elem;
        }
    }

    public Object obtener(int indice) {
        return datos[indice];
    }
}

public class Main {
    public static void main(String[] args) {
        Contenedor c = new Contenedor(10);
        c.anhadir("Hola");
        c.anhadir(42);
        c.anhadir(3.14);

        String texto = (String) c.obtener(0);   // downcasting necesario
        Integer numero = (Integer) c.obtener(1);
        System.out.println(texto + " " + numero);
    }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta

La programación genérica consiste en escribir código que no está ligado a un tipo de dato concreto, sino que puede funcionar con distintos tipos. El objetivo es crear estructuras de datos y algoritmos reutilizables que operen sobre cualquier tipo, evitando tener que duplicar el código para cada tipo específico. En lugar de escribir un contenedor para enteros, otro para cadenas y otro para cada clase nueva, se escribe uno solo que sirva para todos.

El ejemplo anterior sí constituye una forma básica y primitiva de programación genérica. Tanto el `void*` de C como el `Object` de Java permiten almacenar cualquier tipo de dato en la misma estructura. Sin embargo, se trata de una genericidad muy rudimentaria porque se pierde la información del tipo: al extraer un elemento, el programador debe recordar qué tipo almacenó y realizar un cast manual. No existe ningún mecanismo del compilador que ayude a verificar que los tipos son correctos, lo que puede provocar errores difíciles de detectar. Los mecanismos modernos de genericidad (como los generics de Java o los templates de C++) solucionan precisamente estas limitaciones.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta

El primer problema es la **ausencia de chequeo de tipos en tiempo de compilación**. Cuando se usa `void*` u `Object`, el compilador no puede verificar que los tipos que se insertan y los que se extraen sean coherentes. Por ejemplo, se podría almacenar un `String` y luego intentar recuperarlo como un `Integer`; el compilador no detectaría ningún error y el fallo solo se manifestaría en tiempo de ejecución con una `ClassCastException` en Java o un comportamiento indefinido en C.

El segundo problema es la **necesidad de downcasting explícito**. Cada vez que se extrae un elemento, es necesario hacer un cast al tipo concreto que se espera. Esto hace el código más verboso, propenso a errores y difícil de mantener. Además, se pierde la posibilidad de que el IDE o el compilador proporcionen autocompletado o detección temprana de errores, ya que todo es tratado como `Object`.

El tercer problema es la **imposibilidad de restringir el tipo de elementos**. Si se desea que un contenedor aloje exclusivamente cadenas de texto, no hay forma de expresarlo en la propia estructura: cualquier `Object` puede insertarse. Esto elimina la garantía de homogeneidad dentro de la colección y traslada al programador toda la responsabilidad de mantener la consistencia de tipos, algo que idealmente debería hacer el compilador.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta

Los parámetros de tipo son identificadores que se colocan entre ángulos (`< >`) al declarar una clase, interfaz o método, y que actúan como "variables de tipo". En lugar de fijar un tipo concreto como `Object` o `String`, se utiliza un nombre genérico —por convención una letra mayúscula como `T`, `E` o `K`— que será sustituido por un tipo real cuando se utilice la clase o el método. De este modo, se puede escribir código que opera sobre un tipo aún desconocido, pero que queda fijado en el momento de la instanciación.

Por ejemplo, al declarar `class Contenedor<T>`, se indica que `Contenedor` trabaja con un tipo genérico `T`. Cuando se crea una instancia escribiendo `Contenedor<String>`, el compilador sabe que ese contenedor concreto solo admite `String` y devolverá `String` en sus métodos. Esto resuelve los problemas anteriores: el compilador verifica los tipos en tiempo de compilación, desaparece la necesidad de hacer downcasting manual y se garantiza la homogeneidad del contenido.

Los parámetros de tipo son, por tanto, el mecanismo que permite pasar de la genericidad rudimentaria (basada en `void*` u `Object`) a una genericidad real con seguridad de tipos. Se obtiene la flexibilidad de no duplicar código para cada tipo concreto, pero sin sacrificar las comprobaciones del compilador.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

Tanto en Java como en C++ se dispone de mecanismos para crear colecciones genéricas parametrizadas por tipo. En Java se utilizan los generics con la sintaxis `<String>`, y en C++ se usan los templates con la misma notación. En ambos casos, al especificar el tipo concreto, el compilador garantiza que solo se pueden insertar elementos de ese tipo y que, al extraerlos, ya se obtienen con el tipo correcto sin necesidad de cast.

La diferencia visual es mínima: en Java se usa `ArrayList<String>` y en C++ `vector<string>`. Pero internamente, como se verá en la siguiente pregunta, el mecanismo de funcionamiento es muy diferente. Lo importante en ambos casos es que el recorrido devuelve directamente elementos de tipo `String` (o `string`): se pueden invocar sus métodos específicos con total seguridad, sin necesidad de preguntar por el tipo ni hacer conversiones.

```java
// Java con Generics
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> nombres = new ArrayList<>();
        nombres.add("Ana");
        nombres.add("Luis");
        nombres.add("Marta");

        // nombres.add(42);  // Error de compilación: no es String

        for (String nombre : nombres) {
            // 'nombre' ya es de tipo String, sin cast
            System.out.println(nombre.toUpperCase());
        }
    }
}
```

```cpp
// C++ con Templates
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

int main() {
    std::vector<std::string> nombres;
    nombres.push_back("Ana");
    nombres.push_back("Luis");
    nombres.push_back("Marta");

    // nombres.push_back(42);  // Error de compilación: no es string

    for (const std::string& nombre : nombres) {
        // 'nombre' ya es de tipo string, sin cast
        std::cout << nombre << std::endl;
    }

    return 0;
}
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

No, C++ y Java gestionan los parámetros de tipo de formas muy distintas a nivel de compilación. Aunque la sintaxis y el uso se parecen en la superficie, el mecanismo subyacente es radicalmente diferente en cada lenguaje. Esta diferencia tiene implicaciones importantes sobre lo que se puede o no hacer con los tipos genéricos.

En C++, se emplea la **instanciación de plantillas** (_template instantiation_). Cuando el compilador encuentra un uso como `vector<int>` y otro como `vector<string>`, genera internamente una versión completa y separada del código de la clase para cada tipo concreto. Es como si se duplicara el código fuente reemplazando el parámetro de tipo por `int` en una copia y por `string` en otra. Cada instanciación es una clase independiente con su propio código máquina. Esto significa que el tipo concreto está presente en tiempo de ejecución y permite optimizaciones específicas, pero puede aumentar el tamaño del binario si se usan muchos tipos diferentes.

En Java, se utiliza el **_type erasure_** (borrado de tipos). El compilador verifica que los tipos son correctos en tiempo de compilación, pero después **elimina** toda la información genérica y la reemplaza por `Object` (o por el tipo del _bound_ si existe una restricción). Así, `ArrayList<String>` y `ArrayList<Integer>` generan exactamente el mismo bytecode: internamente ambos son un `ArrayList` de `Object`. El compilador inserta automáticamente los casts necesarios al extraer elementos. Esto tiene consecuencias prácticas: no se puede hacer `new T()`, ni `new T[]`, ni usar `instanceof` con un tipo genérico, porque en tiempo de ejecución no existe información sobre `T`.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`.

### Respuesta

La clase `Par` es un ejemplo clásico de tipo genérico con dos parámetros de tipo, habitualmente llamados `A` y `B`. Cada instancia almacena dos valores que pueden ser de tipos distintos entre sí. Esto es útil cuando se necesita devolver dos valores desde un método sin crear una clase específica para cada combinación de tipos. En C no existe un mecanismo equivalente directo; habría que crear un `struct` nuevo para cada caso o recurrir a punteros de salida.

Al parametrizar `Par` con dos tipos, se garantiza la seguridad de tipos sin necesidad de casting. Cuando se crea un `Par<Double, Double>`, los getters devuelven directamente `Double`, no `Object`. Esto permite usar el resultado de forma inmediata y segura. En el ejemplo, se emplea `Par` para devolver simultáneamente la media y la desviación típica de un array de `double`, evitando la necesidad de crear una clase ad hoc como `ResultadoEstadistico`.

```java
public class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}

public class Estadistica {
    public static Par<Double, Double> mediaYDesviacion(double[] valores) {
        double suma = 0;
        for (double v : valores) {
            suma += v;
        }
        double media = suma / valores.length;

        double sumaCuadrados = 0;
        for (double v : valores) {
            sumaCuadrados += Math.pow(v - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / valores.length);

        return new Par<>(media, desviacion);
    }
}

public class Main {
    public static void main(String[] args) {
        double[] datos = {4.0, 7.0, 13.0, 2.0, 8.0};

        Par<Double, Double> resultado = Estadistica.mediaYDesviacion(datos);

        // getPrimero() y getSegundo() devuelven Double directamente, sin cast
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo.

### Respuesta

Los métodos genéricos permiten declarar parámetros de tipo propios, independientes de los de la clase. La sintaxis consiste en colocar el parámetro de tipo entre ángulos justo antes del tipo de retorno: `public static <T> T seleccionaUno(T a, T b)`. Esto indica que `T` se inferirá a partir de los argumentos que se pasen en la llamada concreta.

Si se define el método con `Object`, ambos problemas quedan sin resolver. En primer lugar, el método devuelve `Object`, con lo que el código que lo invoca necesita hacer downcasting para obtener el tipo concreto. En segundo lugar, nada impide pasar un `String` y un `Integer` como argumentos, ya que ambos son `Object`; no existe restricción de que los dos sean del mismo tipo.

Con la versión genérica, el compilador infiere `T` a partir de los argumentos. Si se pasan dos `String`, `T` es `String` y el retorno es `String` directamente, sin cast. Si se intenta pasar un `String` y un `Integer`, el compilador no puede unificar `T` y produce un error de compilación. Así, el método genérico ofrece las dos ventajas: evitar el downcasting y forzar que ambos objetos sean del mismo tipo.

```java
import java.util.Random;

public class Selector {
    private static final Random rng = new Random();

    // Versión con Object: no hay seguridad de tipos
    public static Object seleccionaUnoObject(Object a, Object b) {
        return rng.nextBoolean() ? a : b;
    }

    // Versión genérica: seguridad de tipos garantizada
    public static <T> T seleccionaUno(T a, T b) {
        return rng.nextBoolean() ? a : b;
    }
}

public class Main {
    public static void main(String[] args) {
        // --- Con Object ---
        Object resultado1 = Selector.seleccionaUnoObject("Hola", "Mundo");
        String texto1 = (String) resultado1;  // downcasting necesario

        // Esto compila sin error, aunque los tipos son distintos:
        Object mezcla = Selector.seleccionaUnoObject("Hola", 42);

        // --- Con genéricos ---
        String resultado2 = Selector.seleccionaUno("Hola", "Mundo");
        // No hace falta downcasting: resultado2 ya es String

        // Esto NO compila: T no puede ser String e Integer a la vez
        // String error = Selector.seleccionaUno("Hola", 42);
    }
}
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, se pueden establecer restricciones en los parámetros de tipo. En Java se utiliza la palabra clave `extends` dentro de la declaración del parámetro de tipo, con la sintaxis `<T extends Number>`. Esto se denomina _bounded type parameter_ (parámetro de tipo acotado). De este modo, `T` puede ser `Integer`, `Double`, `Float` o cualquier otra subclase de `Number`, pero no puede ser `String` ni ningún tipo que no extienda de `Number`. Dentro de la clase, se puede tratar `T` como si fuera al menos un `Number` e invocar sus métodos, como `doubleValue()`.

Respecto al _type erasure_ en la versión con generics: cuando se declara `<T extends Number>`, tras la compilación el compilador no sustituye `T` por `Object` (como haría sin restricción), sino por `Number`, que es el _bound_. Es decir, el tipo final tras la compilación de `T` es `Number`. La diferencia con la versión sin generics es que el compilador ha verificado previamente la consistencia de los tipos, de forma que el bytecode resultante es equivalente, pero con la garantía de que en tiempo de compilación se detectaron posibles errores.

```java
// ──── Solución 1: sin generics, con Number ────

public class Punto {
    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// ──── Solución 2: con generics y bounded type ────

public class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

La solución sin generics permite crear un punto con coordenadas de tipos distintos sin que el compilador lo impida. Por ejemplo, `new Punto(3, 2.5)` es perfectamente válido, ya que tanto `Integer` como `Double` son subtipos de `Number`. No hay ningún mecanismo que fuerce que ambas coordenadas sean del mismo tipo de número. Esto puede ser o no deseable: si se quiere un punto estrictamente de enteros, nada impide meter accidentalmente un `Double` en una de sus coordenadas.

La solución con generics, al declarar `Punto<T extends Number>`, obliga a que ambas coordenadas sean del mismo tipo `T`. Si se escribe `new Punto<Integer>(3, 2.5)`, el compilador producirá un error porque `2.5` no es compatible con `Integer`. Esto refuerza la consistencia interna del punto: si se decide que es un punto de enteros, ambas coordenadas deben serlo.

Respecto al tipo de retorno de `getX`: en la solución sin generics, `getX()` devuelve `Number`. Esto obliga a hacer un downcasting si se quiere trabajar con el tipo concreto, por ejemplo `Integer x = (Integer) punto.getX()`. En la solución con generics, `getX()` devuelve `T`, que se resuelve al tipo concreto especificado. Si el punto es `Punto<Integer>`, entonces `getX()` devuelve directamente `Integer`, sin necesidad de ningún cast. Esta es la ventaja fundamental de los generics: preservar la información del tipo concreto a través de toda la API de la clase.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

```java
public interface Punto {
    public double distanciaA(Punto p);
}

public class Punto2D implements Punto {
     private final double x, y;
     public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    public double distanciaA(Punto p) {
        if (p instanceof Punto2D) {
            Punto2D p2d = (Punto2D) p;
            return Math.sqrt(Math.pow(x - p2d.x, 2)
                    + Math.pow(y - p2d.y, 2));
        } else {
            throw new RuntimeException("p debe ser Punto 2D");
        }
    }
}
public class Punto3D implements Punto {
    // Igual que Punto2D, pero con tres coordenadas
    ...
}
```

### Respuesta

El problema del código original es que el método `distanciaA` recibe un `Punto` genérico, con lo que el compilador no puede garantizar que se pase un punto del mismo tipo. Un `Punto2D` podría recibir un `Punto3D` y solo se detectaría el error en tiempo de ejecución mediante el `instanceof`. La solución es parametrizar la interfaz `Punto` con un tipo genérico que represente "el propio tipo", de modo que cada implementación fije ese parámetro a sí misma.

Al declarar `Punto<T extends Punto<T>>`, se establece lo que se conoce como un _self-referencing generic type_: cada implementación concreta indica que trabaja con puntos de su mismo tipo. `Punto2D` implementa `Punto<Punto2D>`, de modo que su método `distanciaA` recibe directamente un `Punto2D`. Ya no es necesario comprobar con `instanceof` ni hacer downcasting, porque el compilador garantiza que el argumento es del tipo correcto.

Esta técnica aprovecha los generics para convertir un error de tiempo de ejecución en un error de tiempo de compilación. Si alguien intenta calcular la distancia entre un `Punto2D` y un `Punto3D`, el compilador lo rechazará inmediatamente. El código queda más limpio, más seguro y más fácil de mantener.

```java
public interface Punto<T extends Punto<T>> {
    public double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2)
                + Math.pow(z - p.z, 2));
    }
}

public class Main {
    public static void main(String[] args) {
        Punto2D a = new Punto2D(0, 0);
        Punto2D b = new Punto2D(3, 4);
        System.out.println(a.distanciaA(b));  // 5.0

        Punto3D c = new Punto3D(0, 0, 0);
        Punto3D d = new Punto3D(1, 2, 2);
        System.out.println(c.distanciaA(d));  // 3.0

        // a.distanciaA(c);  // Error de compilación: se espera Punto2D, no Punto3D
    }
}
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque `String` es subtipo de `Object`, `List<String>` **no** es subtipo de `List<Object>`. Esto puede resultar sorprendente, pero tiene una razón de seguridad fundamental. Si `List<String>` fuera subtipo de `List<Object>`, se podría asignar una lista de cadenas a una referencia de tipo `List<Object>` y luego insertar un `Integer` a través de esa referencia, corrompiendo la lista original que solo debería contener cadenas. Para evitar este problema, los tipos genéricos en Java son **invariantes** respecto a su parámetro de tipo: `List<String>` y `List<Object>` son tipos completamente independientes, sin relación de subtipado.

En cambio, con los arrays sí ocurre: `String[]` **es** subtipo de `Object[]`. Los arrays en Java son **covariantes**: si `A` es subtipo de `B`, entonces `A[]` es subtipo de `B[]`. Esto permite escribir `Object[] arr = new String[3]`, lo cual compila sin problemas. Sin embargo, si después se ejecuta `arr[0] = 42`, el compilador no detecta ningún error (ya que `arr` es de tipo `Object[]`), pero en tiempo de ejecución se lanza una `ArrayStoreException` porque el array real es de `String[]` y no puede almacenar un `Integer`. Este es precisamente el problema que los generics evitan al ser invariantes.

A partir de estos conceptos, se definen tres variantes: un tipo genérico es **covariante** si respeta la dirección del subtipado (`A` subtipo de `B` implica `G<A>` subtipo de `G<B>`); es **contravariante** si la invierte (`A` subtipo de `B` implica `G<B>` subtipo de `G<A>`); y es **invariante** si no existe ninguna relación de subtipado entre `G<A>` y `G<B>`, independientemente de la relación entre `A` y `B`. Los generics de Java son invariantes por defecto, lo cual protege frente a errores en tiempo de ejecución como el que se produce con los arrays covariantes.

```java
import java.util.List;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        // --- Arrays: covariantes (peligroso) ---
        String[] arrStr = {"Hola", "Mundo"};
        Object[] arrObj = arrStr;       // Compila: String[] es subtipo de Object[]
        // arrObj[0] = 42;              // Compila, pero lanza ArrayStoreException en ejecución

        // --- Generics: invariantes (seguro) ---
        List<String> listaStr = new ArrayList<>();
        // List<Object> listaObj = listaStr;  // NO compila: List<String> no es subtipo de List<Object>
    }
}
```

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un _wildcard_ (`?`) es un comodín que representa un tipo desconocido dentro de los generics de Java. Permite relajar la invarianza de los tipos genéricos de forma controlada y segura. Cuando se escribe `List<?>`, se indica que es una lista de algún tipo concreto pero desconocido. Los wildcards se combinan con `extends` y `super` para establecer límites superiores e inferiores, respectivamente.

`List<? extends T>` establece un **límite superior** (_upper bound_): la lista puede contener elementos de `T` o de cualquier subtipo de `T`. Esto proporciona **covarianza**: `List<? extends Number>` acepta un `List<Integer>`, un `List<Double>`, etc. La restricción es que **solo se pueden leer** elementos de la lista (se obtienen como `T`), pero **no se pueden insertar**, porque el compilador no sabe el tipo concreto de la lista y no puede garantizar que la inserción sea segura.

`List<? super T>` establece un **límite inferior** (_lower bound_): la lista puede ser de `T` o de cualquier supertipo de `T`. Esto proporciona **contravarianza**: `List<? super Integer>` acepta un `List<Integer>`, un `List<Number>` o un `List<Object>`. En este caso, **se pueden insertar** elementos de tipo `T` (ya que la lista es al menos de tipo `T`), pero **al leer** solo se obtiene `Object`, porque no se conoce el tipo exacto de la lista. Esta distinción se resume en el principio **PECS** (_Producer Extends, Consumer Super_): si la colección produce elementos (se leen), se usa `extends`; si consume elementos (se insertan), se usa `super`.

```java
import java.util.List;
import java.util.ArrayList;

public class Wildcards {

    // (i) Método que LEE de la lista: usa ? extends (covarianza)
    // Acepta List<Integer>, List<Double>, List<Number>, etc.
    public static double sumar(List<? extends Number> numeros) {
        double total = 0;
        for (Number n : numeros) {
            total += n.doubleValue();
        }
        return total;
    }

    // (ii) Método que ESCRIBE en la lista: usa ? super (contravarianza)
    // Acepta List<Integer>, List<Number>, List<Object>, etc.
    public static void agregarEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
        lista.add(30);
    }
}

public class Main {
    public static void main(String[] args) {
        // Ejemplo con ? extends: se puede pasar List<Integer> donde se espera List<? extends Number>
        List<Integer> enteros = List.of(1, 2, 3, 4, 5);
        List<Double> reales = List.of(1.5, 2.5, 3.5);

        System.out.println("Suma enteros: " + Wildcards.sumar(enteros));  // 15.0
        System.out.println("Suma reales: " + Wildcards.sumar(reales));    // 7.5

        // Ejemplo con ? super: se puede pasar List<Number> donde se espera List<? super Integer>
        List<Number> numeros = new ArrayList<>();
        Wildcards.agregarEnteros(numeros);
        System.out.println("Números: " + numeros);  // [10, 20, 30]

        List<Object> objetos = new ArrayList<>();
        Wildcards.agregarEnteros(objetos);
        System.out.println("Objetos: " + objetos);  // [10, 20, 30]
    }
}
```
