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


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
