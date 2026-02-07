<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación es el principio de agrupar datos (atributos) y comportamientos (métodos) relacionados dentro de una misma unidad o clase. La ocultación de información, por su parte, busca restringir el acceso directo a los datos internos de un objeto, permitiendo que solo se manipulen a través de métodos específicos que controlan cómo se accede y modifica el estado interno.

Las ventajas principales de la ocultación de información incluyen: mejora del mantenimiento del código (se pueden cambiar implementaciones internas sin afectar al código cliente), mayor robustez (se previenen modificaciones no controladas del estado), facilita la depuración (al centralizar las modificaciones en métodos específicos), y promueve un diseño más modular y escalable.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de una clase es el conjunto de métodos y atributos accesibles desde fuera de la clase, es decir, aquellos elementos que otras clases pueden utilizar para interactuar con objetos de esa clase. Es análoga a un contrato que especifica qué operaciones se pueden realizar con los objetos, sin revelar cómo se implementan internamente.

La interfaz pública está directamente relacionada con la ocultación de información: mientras que la interfaz pública expone la funcionalidad disponible, la ocultación mantiene privados los detalles de implementación. De esta forma, los usuarios de la clase solo ven "qué" pueden hacer con ella, sin preocuparse del "cómo" se hace. Esto permite cambiar la implementación interna sin afectar al código que usa la clase.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

Es fundamental diseñar con cuidado la interfaz pública porque una vez publicada y utilizada por otros desarrolladores o en diferentes partes del código, cualquier cambio puede romper el código existente que depende de ella. Cambiar la interfaz pública implica modificar todas las llamadas a esos métodos en todo el código que los utilice, lo cual puede ser costoso y propenso a errores.

Por tanto, no es fácil cambiarla una vez que la clase está en uso. Es mucho más sencillo modificar la implementación interna (gracias a la ocultación de información) que cambiar la interfaz pública. Por eso se recomienda dedicar tiempo al diseño inicial, pensando en las necesidades actuales y futuras, y exponer solo lo estrictamente necesario.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones o propiedades que deben cumplirse siempre sobre el estado interno de un objeto durante toda su vida útil. Por ejemplo, en una clase `Rectangulo`, una invariante podría ser que el ancho y el alto siempre sean valores positivos, o en una clase `CuentaBancaria`, el saldo no debería poder ser negativo (dependiendo de las reglas de negocio).

La ocultación de información es esencial para mantener estas invariantes porque al hacer privados los atributos y forzar que toda modificación pase por métodos públicos controlados, se puede validar que los cambios no violen las invariantes. Si los atributos fueran públicos, cualquier código externo podría modificarlos directamente y romper estas condiciones, dejando al objeto en un estado inconsistente o inválido.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

```java
public class Punto {
    private double x;
    private double y;
    
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
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La interfaz pública de la clase `Punto` está compuesta por el constructor `Punto(double, double)`, los métodos `getX()`, `getY()` y `calcularDistanciaAOrigen()`. Estos son los elementos accesibles desde otras clases.

El modificador `private` indica que un miembro (atributo o método) solo es accesible desde dentro de la propia clase, mientras que `public` significa que es accesible desde cualquier otra clase. En este ejemplo, los atributos `x` e `y` son privados para ocultar la implementación interna, mientras que los métodos que forman parte de la interfaz con el exterior son públicos.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores `public` y `private` se pueden aplicar a varios elementos de una clase. Se pueden usar en atributos (variables de instancia y de clase), métodos (tanto de instancia como de clase), constructores, clases internas (clases definidas dentro de otras clases) y constantes.

Sin embargo, las clases de nivel superior (no anidadas) solo pueden ser `public` o tener visibilidad de paquete (sin modificador explícito), pero no pueden ser `private`. Esto se debe a que una clase privada de nivel superior no tendría sentido, ya que nadie podría acceder a ella para crear instancias.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí, existen más niveles de visibilidad además de público y privado. En Java hay cuatro niveles: `public` (accesible desde cualquier clase), `private` (solo accesible dentro de la propia clase), `protected` (accesible desde la propia clase, subclases y clases del mismo paquete), y visibilidad de paquete o por defecto (sin modificador, accesible solo desde clases del mismo paquete).

Otros lenguajes orientados a objetos también ofrecen diferentes niveles de visibilidad, aunque pueden variar en su nomenclatura y funcionamiento. Por ejemplo, C++ tiene `public`, `private` y `protected`, pero el significado de `protected` es ligeramente diferente (solo accesible desde la clase y sus subclases, no desde el paquete). Python utiliza una convención menos estricta con guiones bajos para indicar que algo es "privado" por convención, pero no impone restricciones reales de acceso.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

```java
public class Punto {
    private double x;
    private double y;
    
    // ... constructor y otros métodos ...
    
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // Acceso a atributo privado de otro
        double dy = this.y - otro.y;  // Acceso a atributo privado de otro
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

La respuesta correcta es (a): los miembros privados están ocultos para otras clases, no para otras instancias de la misma clase. Como se observa en el código, el método `calcularDistanciaAPunto` puede acceder directamente a los atributos privados `x` e `y` del parámetro `otro`, que es otra instancia de `Punto`.

Esto se debe a que las restricciones de visibilidad en Java (y en la mayoría de lenguajes orientados a objetos) se aplican a nivel de clase, no a nivel de instancia. Cualquier método dentro de la clase `Punto` puede acceder a los miembros privados de cualquier objeto de tipo `Punto`, incluyendo los parámetros que se reciban. Si la visibilidad fuera por instancia, tendríamos que usar getters incluso dentro de métodos de la misma clase, lo cual sería poco práctico.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos "getter" (también llamados accesores) son métodos públicos que permiten consultar u obtener el valor de un atributo privado de una clase. Por convención en Java, se nombran con el prefijo "get" seguido del nombre del atributo con la primera letra en mayúscula, como `getX()` o `getNombre()`. Para atributos booleanos, suele usarse el prefijo "is", como `isActivo()`.

Los métodos "setter" (también llamados mutadores) son métodos públicos que permiten modificar el valor de un atributo privado. Se nombran con el prefijo "set" seguido del nombre del atributo, como `setX(double valor)` o `setNombre(String nombre)`. Estos métodos son importantes porque permiten incluir validaciones antes de modificar el atributo, manteniendo así las invariantes de clase. Por ejemplo, un setter podría rechazar valores negativos si el atributo debe ser siempre positivo.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, la "seguridad" en este contexto no se refiere a seguridad informática contra ataques externos o hackeos. Se refiere más bien a la seguridad o robustez del código contra errores de programación, es decir, a la capacidad de prevenir que el programa entre en estados inconsistentes o inválidos por uso incorrecto.

La ocultación de información mejora esta "seguridad" al garantizar que el estado interno de los objetos solo se pueda modificar de formas controladas y validadas. Esto previene errores accidentales del programador que usa la clase, como asignar valores inválidos directamente a atributos. Es una protección contra bugs y uso inadecuado del código, no contra ataques maliciosos. Este tipo de seguridad también se conoce como "seguridad de tipos" o "robustez del código".


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Los miembros de instancia son atributos o métodos que pertenecen a cada objeto individual creado a partir de una clase. Cada instancia tiene su propia copia de los atributos de instancia y puede tener valores diferentes. Por ejemplo, cada objeto `Punto` tiene sus propios valores de `x` e `y`.

Los miembros de clase (también llamados miembros estáticos) pertenecen a la clase en sí misma, no a las instancias individuales. Existe una única copia compartida por todas las instancias de la clase. Se acceden típicamente usando el nombre de la clase en lugar de una referencia a un objeto.

Sí, los miembros de clase también se pueden ocultar usando el modificador `private`. De hecho, es una práctica común hacer privados los atributos de clase y proporcionar métodos públicos estáticos para acceder a ellos si es necesario. Esto mantiene el mismo principio de encapsulación que con los miembros de instancia.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido en varios casos. Un constructor privado impide que se creen instancias de la clase desde fuera de ella. Esto es útil en el patrón Singleton (para garantizar que solo exista una instancia de la clase), en clases que solo contienen métodos estáticos (como `Math` en Java), o cuando se quiere controlar la creación de objetos mediante métodos factoría.

Los métodos factoría son métodos estáticos públicos que internamente llaman al constructor privado y realizan lógica adicional antes de devolver la instancia. Esto permite tener más control sobre cómo y cuándo se crean los objetos, aplicar validaciones, implementar patrones de diseño, o incluso devolver instancias ya existentes en lugar de crear nuevas (pool de objetos).


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

```java
public class Punto {
    private double x;
    private double y;
    
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        
        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }
    
    public static double getMaxX() {
        return maxX;
    }
    
    public static double getMaxY() {
        return maxY;
    }
    
    // ... resto de métodos ...
}
```

En Java, los miembros de clase se indican con la palabra clave `static`. Los atributos estáticos `maxX` y `maxY` son compartidos por todas las instancias de `Punto` y mantienen los valores máximos encontrados. Cada vez que se crea un nuevo punto, el constructor actualiza estos valores si es necesario.

Los métodos estáticos (como `getMaxX()` y `getMaxY()`) también se declaran con `static` y solo pueden acceder a miembros estáticos, no a miembros de instancia. Se invocan usando el nombre de la clase: `Punto.getMaxX()`, aunque también es posible (pero no recomendado) invocarlos desde una instancia.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeada = Math.round(x);
    double yRedondeada = Math.round(y);
    return new Punto(xRedondeada, yRedondeada);
}
```

Sí, se ha usado `static` porque un método factoría debe ser un método de clase, no de instancia. Esto tiene sentido porque su propósito es precisamente crear nuevas instancias, y no se puede llamar a un método de instancia sin tener antes una instancia.

Este método factoría proporciona una forma alternativa de crear puntos con una lógica de construcción específica (redondeo de coordenadas). Se invocaría como `Punto p = Punto.crearPuntoRedondeado(3.7, 5.2);`. Los métodos factoría son útiles cuando se necesitan diferentes formas de construcción, cuando el constructor requiere validaciones complejas, o cuando se quiere dar un nombre más descriptivo al proceso de creación.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```java
public class Punto {
    private double[] coordenadas;  // [0] = x, [1] = y
    
    public Punto(double x, double y) {
        coordenadas = new double[2];
        coordenadas[0] = x;
        coordenadas[1] = y;
    }
    
    public double getX() {
        return coordenadas[0];
    }
    
    public double getY() {
        return coordenadas[1];
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + 
                        coordenadas[1] * coordenadas[1]);
    }
    
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Como se puede observar, la implementación interna de la clase ha cambiado completamente (ahora usa un array en lugar de dos variables separadas), pero la interfaz pública permanece idéntica. Los métodos públicos siguen funcionando exactamente igual desde el punto de vista del código cliente.

Este es uno de los beneficios clave de la encapsulación: se puede modificar la estructura interna de los datos sin que afecte al código que utiliza la clase. Si los atributos hubieran sido públicos, todos los accesos directos a `x` e `y` en el código cliente se habrían roto con este cambio.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

No, no es mejor declararlo público. Aunque aparentemente proporcionar getter y setter públicos es equivalente a tener el atributo público, en realidad los métodos ofrecen ventajas importantes: permiten añadir validaciones en el setter para mantener invariantes, pueden incluir lógica adicional (como logging o notificaciones), permiten cambiar la representación interna sin afectar al código cliente, y facilitan la depuración al centralizar el acceso.

La convención más habitual y recomendada en POO es que los atributos sean privados. Esto es un principio fundamental de la encapsulación. Incluso si inicialmente no se necesita validación, mantener los atributos privados permite añadirla en el futuro sin romper la interfaz pública.

Sí está directamente relacionado con las invariantes de clase. Si los atributos fueran públicos, no habría forma de garantizar que se mantengan las invariantes, porque cualquier código externo podría modificarlos sin restricciones. Con setters, se puede validar que cada modificación respete las reglas de negocio y las restricciones de la clase.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase inmutable es aquella cuyos objetos no pueden ser modificados después de su creación. Una vez construido el objeto, su estado permanece constante durante toda su vida. Para lograr esto, todos los atributos deben ser finales (`final`), no se proporcionan setters, y se debe tener cuidado con atributos que sean referencias a objetos mutables.

Un método modificador es cualquier método que cambia el estado interno del objeto, no solo los setters. Por ejemplo, en una clase `Lista`, métodos como `añadir()`, `eliminar()` o `ordenar()` serían modificadores aunque no sean setters en el sentido tradicional.

Las clases inmutables tienen varias ventajas: son inherentemente thread-safe (seguras en entornos multi-hilo sin necesidad de sincronización), son más fáciles de razonar sobre ellas porque su estado no cambia, pueden compartirse libremente sin necesidad de hacer copias defensivas, y son ideales para usar como claves en estructuras como `HashMap`. La clase `String` en Java es un ejemplo clásico de clase inmutable.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, no es recomendable incluir setters automáticamente para todos los atributos. Solo se deben proporcionar setters cuando realmente sea necesario que el atributo se pueda modificar después de la construcción del objeto. Crear setters innecesarios viola el principio de diseñar interfaces mínimas y puede comprometer la inmutabilidad o las invariantes de clase.

En muchos casos, los atributos deben establecerse en el constructor y no modificarse posteriormente, haciendo innecesarios los setters. Además, proporcionar setters para todos los atributos puede hacer que sea más difícil mantener invariantes complejas que involucren múltiples atributos relacionados.

La recomendación es ser deliberado: incluir getters cuando sea necesario exponer información, pero ser mucho más restrictivo con los setters. Cada setter debe evaluarse individualmente preguntándose si tiene sentido que ese atributo cambie después de la creación del objeto y si se pueden mantener las invariantes de clase con ese cambio.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase `String` en Java es inmutable. Esto significa que cualquier operación que parezca modificar una cadena en realidad crea una nueva cadena en memoria, dejando la original sin cambios. Por ejemplo, si se ejecuta `str1 = str1 + "hola"`, se crea un nuevo objeto `String` con el contenido concatenado y se asigna a `str1`, pero el objeto original permanece en memoria hasta que el recolector de basura lo elimine.

Cuando se concatenan dos cadenas con el operador `+`, se crea un nuevo objeto `String` con el contenido de ambas. Si esto se hace repetidamente en un bucle, se estarán creando muchos objetos temporales, lo cual es muy ineficiente tanto en tiempo como en uso de memoria.

Para concatenaciones múltiples, se debe usar `StringBuilder` (o `StringBuffer` si se necesita thread-safety). Esta clase es mutable y permite construir cadenas eficientemente mediante su método `append()`. Al final, se llama a `toString()` para obtener el `String` resultante. Por ejemplo: `StringBuilder sb = new StringBuilder(); sb.append("hola"); sb.append(" mundo"); String resultado = sb.toString();`


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, la comparación depende del operador o método utilizado. En Java, el operador `==` compara identidad (si ambas referencias apuntan al mismo objeto en memoria), mientras que el método `equals()` está diseñado para comparar contenido o igualdad lógica.

El método `equals()` es un método de la clase `Object` que todas las clases heredan. Por defecto, `equals()` realiza la misma comparación que `==` (comparación por referencia), pero está pensado para ser sobrescrito por las clases que necesiten comparación por contenido. Por ejemplo, la clase `String` sobrescribe `equals()` para comparar el contenido de las cadenas carácter por carácter.

Para comparar dos cadenas en Java, se debe usar el método `equals()`, nunca el operador `==`. Por ejemplo: `if (str1.equals(str2))`. Si se usa `==` con cadenas, se estaría comparando si apuntan al mismo objeto en memoria, no si tienen el mismo contenido, lo cual puede dar resultados inesperados. Para comparar ignorando mayúsculas/minúsculas, existe `equalsIgnoreCase()`.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases wrapper (envolventes) son clases que encapsulan tipos de datos primitivos para tratarlos como objetos. En Java, existen clases wrapper para cada tipo primitivo: `Integer` para `int`, `Double` para `double`, `Boolean` para `boolean`, `Character` para `char`, etc. Esto es necesario porque Java distingue entre tipos primitivos (que no son objetos) y tipos referencia (objetos).

En Java, el proceso puede ser manual (creando explícitamente objetos wrapper) o automático mediante autoboxing y unboxing. El autoboxing convierte automáticamente un primitivo en su wrapper (`Integer i = 5;`), mientras que el unboxing hace lo contrario (`int x = i;`). Esta funcionalidad se introdujo en Java 5 para simplificar el código.

Las ventajas principales son: permiten usar primitivos en colecciones genéricas que solo aceptan objetos (como `ArrayList<Integer>`), proporcionan métodos útiles para conversiones y operaciones (`Integer.parseInt()`, `Double.isNaN()`), y permiten usar valores `null` para representar ausencia de valor.

No todos los lenguajes orientados a objetos tienen esta distinción. Por ejemplo, en Python o Smalltalk todo es un objeto, incluidos los números, por lo que no necesitan wrappers. Java mantiene tipos primitivos principalmente por razones de eficiencia y compatibilidad con versiones anteriores.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado (enum) es un tipo especial que representa un conjunto fijo y limitado de constantes relacionadas. Por ejemplo, los días de la semana, los meses del año, los estados de un pedido (PENDIENTE, ENVIADO, ENTREGADO), etc. Los enumerados proporcionan seguridad de tipos al restringir los valores posibles a un conjunto predefinido.

Sí, en Java un enumerado es efectivamente una clase especial. Puede tener constructores, atributos, métodos, e incluso implementar interfaces. Cada constante del enumerado es una instancia de esa clase. Esto hace que los enums de Java sean mucho más potentes que en lenguajes como C, donde son simplemente enteros con nombres.

En términos de encapsulación, los enumerados en Java ofrecen ventajas significativas: permiten ocultar detalles de implementación mediante atributos privados, cada instancia puede tener su propio estado inmutable, se pueden definir comportamientos específicos mediante métodos, y garantizan que no se puedan crear más instancias fuera de las definidas (el constructor es implícitamente privado). Esto proporciona un control total sobre el conjunto de valores válidos y su comportamiento.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

```java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);
    
    private final int dias;
    private final int ordinal;
    
    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }
    
    public int getDias() {
        return dias;
    }
    
    public int getOrdinal() {
        return ordinal;
    }
    
    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 3 && ordinal <= 5;
        } else {
            return ordinal >= 9 && ordinal <= 11;
        }
    }
    
    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 6 && ordinal <= 8;
        } else {
            return ordinal == 12 || ordinal <= 2;
        }
    }
    
    public boolean esDeOtonio(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 9 && ordinal <= 11;
        } else {
            return ordinal >= 3 && ordinal <= 5;
        }
    }
    
    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal == 12 || ordinal <= 2;
        } else {
            return ordinal >= 6 && ordinal <= 8;
        }
    }
}
```

Este enumerado encapsula información sobre cada mes mediante atributos privados `dias` y `ordinal`, que se inicializan a través del constructor (que es implícitamente privado en los enums). Cada constante del enumerado (ENERO, FEBRERO, etc.) es una instancia única con sus propios valores.

Los métodos públicos proporcionan acceso controlado a esta información. Los métodos de estaciones consideran el hemisferio para determinar correctamente la estación correspondiente a cada mes. Por ejemplo, junio es verano en el hemisferio norte pero invierno en el sur. Esta implementación demuestra cómo los enumerados en Java pueden tener comportamiento complejo manteniendo la encapsulación.
