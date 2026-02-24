<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C, al no existir un mecanismo de excepciones, se debe recurrir a técnicas alternativas para comunicar errores desde una función hacia el código que la invoca. La comunicación del error debe realizarse a través del valor de retorno de la función o mediante parámetros adicionales que actúen como indicadores de estado.

**Opción 1: Usar el valor de retorno para indicar el error**. En este enfoque, la función devuelve un código de error especial (por ejemplo, un número negativo o `NaN`) cuando la operación no puede completarse correctamente. El resultado válido se devuelve normalmente cuando no hay error.

```c
#include <stdio.h>
#include <math.h>

double raiz(double numero) {
    if (numero < 0) {
        return -1.0;  // Código de error
    }
    return sqrt(numero);
}

int main() {
    double resultado = raiz(-9.0);
    if (resultado < 0) {
        printf("Error: No se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```

**Opción 2: Usar un parámetro por referencia para indicar el éxito o error**. En este diseño, la función devuelve el resultado mediante un puntero pasado como argumento, y usa el valor de retorno de la función para indicar si la operación fue exitosa (típicamente devolviendo 0 para éxito y -1 o algún código de error para fallos).

```c
#include <stdio.h>
#include <math.h>

int raiz(double numero, double *resultado) {
    if (numero < 0) {
        return -1;  // Código de error
    }
    *resultado = sqrt(numero);
    return 0;  // Éxito
}

int main() {
    double resultado;
    if (raiz(-9.0, &resultado) != 0) {
        printf("Error: No se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo de control de errores que permite interrumpir el flujo normal de ejecución de un programa cuando ocurre una situación excepcional o un error. A diferencia de los códigos de error tradicionales de C, las excepciones se propagan automáticamente hacia arriba en la pila de llamadas hasta que son capturadas y manejadas.

Cuando se implementa una función, el programador lanza excepciones para señalizar que ha ocurrido un error o una condición excepcional que impide completar la operación normalmente. Esto libera a la función de tener que gestionar todos los posibles errores internamente. Por otro lado, cuando se llama a una función, el programador puede capturar las excepciones que esta pudiera lanzar para manejar los errores de forma apropiada según el contexto específico de uso.

El objetivo principal es separar la lógica del flujo normal del programa de la lógica de manejo de errores, haciendo el código más limpio, legible y mantenible. Además, las excepciones garantizan que los errores no pasen desapercibidos, ya que si no se capturan, el programa terminará abruptamente mostrando información sobre el error ocurrido.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, en lugar de devolver códigos de error, se pueden utilizar excepciones para señalizar condiciones de error. La función que detecta el error lanza una excepción, y el código que realiza la llamada puede capturarla usando un bloque `try-catch` para manejar el error apropiadamente.

```java
class Calculadora {
    public double raiz(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo");
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        try {
            double resultado = calc.raiz(-9.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        // El programa puede continuar normalmente
        System.out.println("El programa continúa su ejecución");
    }
}
```

En este ejemplo, el método `raiz` lanza una excepción de tipo `IllegalArgumentException` cuando recibe un número negativo. El método `main` envuelve la llamada a `raiz` en un bloque `try-catch`, donde el bloque `try` contiene el código que podría lanzar la excepción y el bloque `catch` captura y maneja el error. Si ocurre una excepción, el flujo de ejecución salta inmediatamente al bloque `catch`, permitiendo informar al usuario del error y continuar con la ejecución del programa de forma controlada.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

**Lanzar** una excepción significa crear un objeto excepción y señalizar usando la palabra clave `throw` que ha ocurrido una situación excepcional que interrumpe el flujo normal de ejecución. Cuando se lanza una excepción, la ejecución del método actual se detiene inmediatamente en ese punto. **Controlar** o **capturar** una excepción significa interceptarla usando un bloque `try-catch`, donde se ejecuta el código del bloque `catch` correspondiente para manejar el error de forma apropiada.

La **propagación** de una excepción es el proceso por el cual, si una excepción no es capturada en el método donde se lanza, se propaga automáticamente hacia el método que hizo la llamada. Este proceso continúa hacia arriba en la pila de llamadas hasta que algún método la capture o hasta que llegue al método `main` y, si no se captura allí, termine el programa. Las funciones por donde se propaga la excepción son interrumpidas abruptamente y **no se reanudan** después; su ejecución termina en el punto donde se lanzó o propagó la excepción, y el control pasa directamente al manejador de excepciones.

```java
class Calculadora {
    public double raiz(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException("Número negativo");  // Se lanza la excepción
        }
        return Math.sqrt(numero);  // Esta línea no se ejecuta si numero < 0
    }
    
    public void procesarDatos(double valor) {
        double resultado = raiz(valor);  // Si raiz lanza excepción, esta línea se interrumpe
        System.out.println("Procesado");  // Esta línea NO se ejecuta si hubo excepción
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        try {
            calc.procesarDatos(-9.0);  // Aquí se captura la excepción
            System.out.println("Terminado");  // No se ejecuta si hubo excepción
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        }
        
        System.out.println("El programa continúa");  // Esto sí se ejecuta
    }
}
```

En este ejemplo, cuando `raiz` lanza la excepción, el método `procesarDatos` se interrumpe inmediatamente sin ejecutar las líneas siguientes, y la excepción se propaga hasta `main` donde es capturada. Los métodos `raiz` y `procesarDatos` no se reanudan; simplemente finalizan su ejecución de forma abrupta.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación automática de excepciones presenta varias ventajas significativas frente al manejo de errores tradicional de C. En C, cada función en la cadena de llamadas debe verificar explícitamente el código de error devuelto por la función que llama y decidir si lo maneja localmente o lo propaga manualmente hacia arriba. Esto genera código repetitivo y propenso a errores, ya que es fácil olvidar comprobar un código de error, permitiendo que el error pase desapercibido.

Con excepciones, la propagación es automática: si una función intermedia no necesita manejar un error específico, simplemente no lo captura y la excepción continúa propagándose hacia arriba sin necesidad de código adicional. Esto permite que el error sea manejado en el nivel más apropiado de la aplicación, donde se tiene el contexto necesario para tomar una decisión informada sobre cómo proceder.

Además, las excepciones separan claramente el código de la lógica normal del código de manejo de errores, haciendo el flujo principal del programa más legible y mantenible. En C, el código se ve plagado de comprobaciones `if` para verificar errores después de cada llamada a función, mezclando la lógica de negocio con la gestión de errores. Con excepciones, el código de la lógica principal permanece limpio dentro del bloque `try`, y todo el manejo de errores se concentra en los bloques `catch` correspondientes.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí, en lenguajes orientados a objetos como Java, las excepciones son objetos que pertenecen a clases que heredan de la clase base `Throwable` (o más comúnmente de `Exception`). Esto significa que las excepciones pueden contener datos y métodos como cualquier otro objeto.

En términos de encapsulación, esto ofrece grandes ventajas: las excepciones pueden encapsular toda la información relevante sobre el error ocurrido, incluyendo mensajes descriptivos, códigos de error, contexto adicional, e incluso referencias a otros objetos relacionados con el error. Además, pueden proporcionar métodos para acceder a esta información de forma controlada. Esto contrasta con los simples códigos de error numéricos de C, que por sí solos no proporcionan ningún contexto adicional.

Efectivamente, se pueden crear excepciones personalizadas definiendo nuevas clases que hereden de `Exception` o de alguna de sus subclases. Esto permite modelar errores específicos del dominio de la aplicación con semántica propia.

```java
class NumeroNegativoException extends Exception {
    private double valorProblematico;
    
    public NumeroNegativoException(double valor) {
        super("No se puede calcular la raíz de: " + valor);
        this.valorProblematico = valor;
    }
    
    public double getValorProblematico() {
        return valorProblematico;
    }
}

class Calculadora {
    public double raiz(double numero) throws NumeroNegativoException {
        if (numero < 0) {
            throw new NumeroNegativoException(numero);
        }
        return Math.sqrt(numero);
    }
}
```

Este enfoque orientado a objetos hace que las excepciones sean mucho más expresivas y útiles que los simples códigos de error, permitiendo un manejo de errores más robusto y mantenible.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Cualquier objeto excepción en Java encapsula automáticamente información esencial sobre el contexto del error que no está disponible en los simples códigos de error de C. La información más importante es la **pila de llamadas** (stack trace), que registra la secuencia exacta de métodos que se estaban ejecutando cuando ocurrió el error, incluyendo los nombres de los archivos fuente y los números de línea. Esta información es invaluable para la depuración, ya que permite rastrear exactamente dónde y cómo se originó el problema.

Además del stack trace, el objeto excepción contiene un **mensaje descriptivo** que explica qué salió mal, el **tipo específico de la excepción** (que permite distinguir entre diferentes categorías de errores), y puede incluir información adicional personalizada según la excepción. Por ejemplo, una excepción de archivo no encontrado puede incluir la ruta del archivo que se intentaba abrir.

```java
try {
    calc.raiz(-25.0);
} catch (Exception e) {
    System.out.println("Tipo: " + e.getClass().getName());
    System.out.println("Mensaje: " + e.getMessage());
    System.out.println("Pila de llamadas:");
    e.printStackTrace();  // Muestra toda la secuencia de llamadas
}
```

En C, cuando una función devuelve un código de error como `-1`, no hay forma automática de saber dónde se originó el error en una cadena de llamadas profunda, ni de obtener un mensaje descriptivo sin programar explícitamente ese mecanismo. Esta riqueza de información contextual que proporcionan las excepciones facilita enormemente la diagnóstico y resolución de problemas.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, en Java es posible y común tener múltiples bloques `catch` asociados a un mismo bloque `try`. Esto permite manejar diferentes tipos de excepciones de formas distintas según su naturaleza. Los bloques `catch` se evalúan en orden secuencial de arriba hacia abajo, y se ejecuta el primer `catch` cuyo tipo de excepción coincida con la excepción lanzada.

**Solo se ejecuta un único bloque `catch`**: una vez que se encuentra una coincidencia y se ejecuta su código, la ejecución continúa después de todos los bloques `catch` (o del bloque `finally` si existe). Los demás bloques `catch` son ignorados. Es importante ordenar los bloques `catch` desde las excepciones más específicas hasta las más generales, ya que el compilador generará un error si un `catch` de una excepción más general aparece antes que uno de una más específica.

```java
try {
    // Código que puede lanzar diferentes tipos de excepciones
    double resultado = calc.raiz(-9.0);
    int division = 10 / 0;
} catch (IllegalArgumentException e) {
    System.out.println("Error de argumento inválido: " + e.getMessage());
} catch (ArithmeticException e) {
    System.out.println("Error aritmético: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Error general: " + e.getMessage());
}
```

En este ejemplo, si se lanza una `IllegalArgumentException`, solo se ejecutará el primer `catch`. Si se lanza una `ArithmeticException`, solo se ejecutará el segundo `catch`. El tercer `catch` con `Exception` actúa como capturador genérico para cualquier otro tipo de excepción no manejada por los anteriores.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Java proporciona el bloque `finally` para garantizar que cierto código se ejecute siempre, independientemente de si se lanza una excepción o no. Este bloque es especialmente útil para operaciones de limpieza como cerrar archivos, liberar conexiones de base de datos, o liberar cualquier recurso que deba ser liberado obligatoriamente. El código dentro del `finally` se ejecuta incluso si el bloque `try` termina normalmente, si se captura una excepción en un `catch`, o si la excepción se propaga sin ser capturada.

**Ejemplo con `catch` y `finally`:**

```java
import java.io.*;

public class EjemploFinally {
    public static void leerArchivo(String ruta) {
        FileReader archivo = null;
        try {
            archivo = new FileReader(ruta);
            // ... leer contenido del archivo ...
            System.out.println("Archivo leído");
        } catch (IOException e) {
            System.out.println("Error al leer archivo: " + e.getMessage());
        } finally {
            // Este bloque SIEMPRE se ejecuta
            if (archivo != null) {
                try {
                    archivo.close();
                    System.out.println("Archivo cerrado");
                } catch (IOException e) {
                    System.out.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
    }
}
```

**Ejemplo sin `catch` (solo `try-finally`):**

```java
public static void procesarArchivo(String ruta) throws IOException {
    FileReader archivo = null;
    try {
        archivo = new FileReader(ruta);
        // ... leer contenido del archivo ...
    } finally {
        // Garantiza cierre incluso si la excepción se propaga
        if (archivo != null) {
            archivo.close();
        }
    }
}
```

En el segundo ejemplo, aunque el método no captura la excepción y ésta se propaga hacia arriba, el bloque `finally` se ejecuta antes de que se propague, garantizando que el archivo se cierre correctamente. Este mecanismo es fundamental para evitar fugas de recursos en aplicaciones robustas.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, el bloque `finally` puede usarse sin un bloque `catch` asociado. La estructura `try-finally` (sin `catch`) es completamente válida y se utiliza cuando se quiere garantizar la ejecución de código de limpieza pero no se desea capturar la excepción en ese nivel, permitiéndola propagar hacia arriba en la pila de llamadas.

El bloque `finally` se ejecuta **siempre**, en todos los escenarios posibles: cuando el código del `try` se completa normalmente sin excepciones, cuando se lanza una excepción (capturada o no), e incluso cuando hay una sentencia `return` dentro del bloque `try` o `catch`. En el caso de tener un `return`, el bloque `finally` se ejecuta antes de que el método realmente retorne, lo que permite realizar tareas de limpieza incluso cuando hay salidas tempranas del método.

```java
public class EjemploReturn {
    public static int metodoConReturn() {
        try {
            System.out.println("En try");
            return 1;  // El return no se ejecuta inmediatamente
        } finally {
            System.out.println("En finally");  // Esto se ejecuta primero
        }
        // Salida: "En try" y luego "En finally", y finalmente retorna 1
    }
    
    public static int metodoConReturnEnFinally() {
        try {
            return 1;
        } finally {
            return 2;  // Este return sobreescribe el anterior (MALA PRÁCTICA)
        }
        // Retorna 2, aunque no es recomendable hacer esto
    }
}
```

La única excepción donde `finally` no se ejecutaría es en casos extremos como que la JVM se termine abruptamente (por ejemplo, con `System.exit()`) o que el proceso sea terminado por el sistema operativo. En prácticamente todos los escenarios normales de programación, el bloque `finally` ofrece una garantía sólida de ejecución.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java existen dos categorías de excepciones: **controladas** (checked) y **no controladas** (unchecked). Las excepciones controladas son aquellas que heredan directamente de `Exception` pero no de `RuntimeException`, y el compilador obliga a que el código que las puede lanzar esté dentro de un bloque `try-catch` o que el método declare que las lanza usando `throws`. Las excepciones no controladas son aquellas que heredan de `RuntimeException` o de `Error`, y el compilador no obliga a capturarlas ni declararlas explícitamente.

`RuntimeException` es la clase base para todas las excepciones no controladas. Estas excepciones representan errores de programación o situaciones que generalmente no son recuperables en tiempo de ejecución, como acceder a un índice inválido de un array, intentar una operación sobre una referencia nula, o pasar argumentos inválidos a un método.

**Ejemplos de excepciones controladas típicas:**
- `IOException` - Error de entrada/salida
- `SQLException` - Error de base de datos
- `FileNotFoundException` - Archivo no encontrado
- `ClassNotFoundException` - Clase no encontrada

**Ejemplos de excepciones no controladas típicas:**
- `NullPointerException` - Referencia nula
- `IndexOutOfBoundsException` - Índice fuera de rango
- `IllegalArgumentException` - Argumento inválido
- `ArithmeticException` - Error aritmético (ej. división por cero)

**Situaciones donde se prefiere una excepción controlada:**
- Operaciones de entrada/salida con ficheros externos (puede no existir el archivo)
- Conexiones de red o base de datos (pueden fallar por razones externas)
- Lectura de configuraciones desde archivos (el archivo puede estar corrupto)
- Parseo de datos del usuario que pueden estar en formato incorrecto

**Situaciones donde se prefiere una excepción no controlada:**
- Validación de argumentos de métodos (errores de programación)
- Violaciones de precondiciones o postcondiciones de métodos
- Errores de lógica del programa (acceso a índices inválidos)
- Operaciones sobre estados inválidos de objetos (llamar a un método en momento inapropiado)


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra clave `throws` se utiliza en la firma de un método para declarar explícitamente que ese método puede lanzar ciertas excepciones controladas sin capturarlas internamente. Esto forma parte del contrato del método e informa a los llamadores que deben estar preparados para manejar esas excepciones. La declaración `throws` se coloca al final de la firma del método, antes de las llaves, seguida del tipo o tipos de excepciones separados por comas.

Cuando un método puede lanzar una excepción controlada, el programador tiene dos opciones para satisfacer al compilador: puede capturar la excepción usando un bloque `try-catch` y manejarla localmente, o puede declararla con `throws` para que se propague automáticamente al método llamador. Esta segunda opción es apropiada cuando el método actual no tiene el contexto suficiente para manejar el error de forma significativa, delegando esa responsabilidad a un nivel superior del programa donde se pueda tomar una decisión más informada.

```java
// Opción 1: Capturar la excepción localmente
public void metodoConCaptura() {
    try {
        FileReader archivo = new FileReader("datos.txt");
        // ... usar el archivo ...
    } catch (FileNotFoundException e) {
        System.out.println("Archivo no encontrado: " + e.getMessage());
    }
}

// Opción 2: Declarar con throws y propagar
public void metodoConThrows() throws FileNotFoundException {
    FileReader archivo = new FileReader("datos.txt");
    // ... usar el archivo ...
    // La excepción se propaga automáticamente si ocurre
}
```

El uso de `throws` es especialmente útil en métodos de bajo nivel que realizan operaciones básicas pero no tienen suficiente información sobre el contexto de uso para decidir cómo manejar los errores. De esta forma, el código de alto nivel, que conoce la lógica de negocio, puede decidir si reintenta la operación, usa valores por defecto, informa al usuario, o toma alguna otra acción apropiada.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

En este ejemplo se muestra un método que abre un fichero y declara mediante `throws` que la excepción `IOException` (que incluye `FileNotFoundException`) se propagará hacia el método llamador. A pesar de declarar la excepción con `throws`, se utiliza un bloque `finally` para garantizar que el recurso se cierre correctamente si fue abierto, independientemente de si se lanzó una excepción o no.

```java
import java.io.*;

public class ManejoArchivos {
    
    public String leerPrimeraLinea(String rutaArchivo) throws IOException {
        BufferedReader lector = null;
        try {
            // Esto puede lanzar FileNotFoundException
            lector = new BufferedReader(new FileReader(rutaArchivo));
            
            // Esto puede lanzar IOException
            String linea = lector.readLine();
            return linea;
            
        } finally {
            // Garantiza que se cierre el archivo incluso si hay excepción
            if (lector != null) {
                try {
                    lector.close();
                } catch (IOException e) {
                    // Si falla el cierre, se podría registrar o ignorar
                    System.err.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
    }
    
    // Ejemplo de uso desde otro método
    public static void main(String[] args) {
        ManejoArchivos manejador = new ManejoArchivos();
        try {
            String contenido = manejador.leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + contenido);
        } catch (IOException e) {
            System.out.println("Error al leer archivo: " + e.getMessage());
        }
    }
}
```

En este ejemplo, el método `leerPrimeraLinea` no captura `IOException` ni `FileNotFoundException`, sino que las declara en su firma con `throws`, permitiendo que se propaguen al llamador. Sin embargo, el bloque `finally` asegura que si el lector fue creado exitosamente antes de que ocurriera un error, se cerrará correctamente. Este patrón es fundamental para evitar fugas de recursos en aplicaciones robustas.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, es perfectamente válido poner excepciones no controladas (como `RuntimeException` o cualquiera de sus subclases) en la cláusula `throws` de un método, aunque no es obligatorio hacerlo. Cuando se declaran excepciones no controladas en `throws`, el método llamador **no está obligado** por el compilador a capturarlas con `try-catch` ni a declararlas en su propia cláusula `throws`, ya que son excepciones no controladas por definición.

Declarar excepciones no controladas en `throws` tiene un propósito principalmente **documentativo**. Sirve para informar explícitamente a los programadores que usarán ese método sobre qué tipos de excepciones pueden esperar en condiciones excepcionales, permitiendo que decidan si quieren manejarlas o dejar que se propaguen. Esto es especialmente útil cuando una excepción no controlada es parte importante del comportamiento esperado del método.

```java
public class Calculadora {
    
    // Se documenta explícitamente aunque no es obligatorio
    public double dividir(double a, double b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("División por cero");
        }
        return a / b;
    }
    
    public double raiz(double numero) throws IllegalArgumentException {
        if (numero < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(numero);
    }
}

public class Programa {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        // El programador PUEDE capturarlas si lo desea
        try {
            double resultado = calc.raiz(-5);
        } catch (IllegalArgumentException e) {
            System.out.println("Entrada inválida: " + e.getMessage());
        }
        
        // O puede no capturarlas y dejar que se propaguen
        double resultado2 = calc.dividir(10, 2);  // Sin try-catch
    }
}
```

Aunque no es obligatorio, declarar excepciones no controladas en `throws` mejora la API del método al hacer más explícito su comportamiento, funcionando como parte de la documentación del contrato del método. Esto ayuda a que otros desarrolladores comprendan mejor los posibles casos excepcionales sin tener que examinar la implementación completa del método.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Las **excepciones controladas** se recomiendan para situaciones excepcionales que están fuera del control del programador y que razonablemente se pueden anticipar y recuperar. Esto incluye operaciones de entrada/salida, acceso a recursos externos, problemas de red, o cualquier situación donde factores externos pueden causar fallos. El propósito es forzar al programador a considerar explícitamente cómo manejar estos escenarios, ya que son parte esperada del flujo de la aplicación en entornos reales.

Las **excepciones no controladas** se prefieren para errores de programación o violaciones de precondiciones que representan bugs en el código, como pasar argumentos inválidos a un método, acceder a índices fuera de rango, o manipular referencias nulas. Estos errores no deberían ocurrir si el programa está correctamente implementado, por lo que no tiene sentido obligar a capturarlos en cada llamada; en su lugar, deberían corregirse en el código fuente.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. Java es uno de los pocos lenguajes que implementan excepciones controladas explícitamente. Otros lenguajes populares como **C#, Python, Ruby, JavaScript, y C++** solo tienen excepciones no controladas; no existe diferencia en cómo el compilador o intérprete trata las excepciones. En estos lenguajes, todas las excepciones se comportan como las no controladas de Java: no hay obligación de capturarlas o declararlas.

La decisión de Java de incluir excepciones controladas ha sido controvertida. Muchos programadores las encuentran verbosas y molestas, especialmente cuando se propagan a través de múltiples capas de abstracción. Por este motivo, la tendencia en lenguajes modernos ha sido usar únicamente excepciones no controladas, que es la opción más habitual en la mayoría de lenguajes actuales. Esto da más flexibilidad al programador para decidir dónde y cuándo capturar excepciones sin que el compilador lo fuerce.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido lanzar excepciones dentro de un bloque `catch`, y es una práctica común en varios escenarios. Se puede tanto lanzar una excepción completamente nueva como relanzar la misma excepción que se acaba de capturar. Ambas técnicas permiten realizar alguna acción de limpieza o logging local antes de dejar que la excepción continúe propagándose hacia arriba en la pila de llamadas.

Relanzar la misma excepción capturada tiene sentido cuando se quiere realizar alguna acción local (como registrar el error, limpiar recursos, o notificar al sistema) pero no se puede o no se debe manejar completamente el error en ese nivel. Esto permite que un nivel superior tenga la oportunidad de manejar la excepción de forma más apropiada según su contexto.

**Ejemplo 1: Lanzar una nueva excepción**

```java
public class ServicioDatos {
    
    public void guardarDatos(String archivo, String datos) throws ErrorServicioException {
        try {
            FileWriter escritor = new FileWriter(archivo);
            escritor.write(datos);
            escritor.close();
        } catch (IOException e) {
            // Se captura una excepción de bajo nivel y se lanza una de alto nivel
            System.err.println("Error al guardar: " + archivo);
            throw new ErrorServicioException("No se pudo guardar los datos", e);
        }
    }
}

class ErrorServicioException extends Exception {
    public ErrorServicioException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}
```

**Ejemplo 2: Relanzar la misma excepción**

```java
public class Procesador {
    
    public void procesarArchivo(String ruta) throws IOException {
        try {
            FileReader lector = new FileReader(ruta);
            // ... procesar contenido ...
        } catch (IOException e) {
            // Registrar el error localmente
            System.err.println("Error procesando archivo: " + ruta);
            registrarEnLog(e);
            
            // Relanzar la misma excepción para que niveles superiores la manejen
            throw e;
        }
    }
    
    private void registrarEnLog(Exception e) {
        // Lógica para guardar en archivo de log
    }
}
```

En el primer ejemplo, se transforma una excepción de bajo nivel (`IOException`) en una excepción de dominio de más alto nivel (`ErrorServicioException`), ocultando detalles de implementación. En el segundo ejemplo, se realiza una acción local de logging pero se permite que la excepción original continúe propagándose, manteniendo toda su información contextual intacta para que otros manejadores puedan procesarla.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

En Java, una excepción puede tener otra excepción como su **causa**, creándose así una cadena de excepciones que preserva toda la información sobre el origen del problema. Esto se logra pasando la excepción original como segundo parámetro al constructor de la nueva excepción mediante `super(mensaje, causa)` o usando el método `initCause()`. Esta técnica es fundamental para el patrón de encapsulación de excepciones, donde se captura una excepción de bajo nivel (relacionada con detalles de implementación) y se lanza una excepción de alto nivel (relacionada con la lógica de negocio), manteniendo la información de la excepción original.

Cuando una excepción que tiene una causa se imprime por pantalla (usando `printStackTrace()` o simplemente cuando el programa termina con una excepción no capturada), **sí se ve la cadena completa de excepciones**. La salida muestra primero la excepción más externa, seguida de "Caused by:" y la excepción causa, con sus respectivos stack traces. Esto proporciona un rastreo completo desde el error de alto nivel hasta la causa raíz técnica.

```java
// Excepción personalizada de alto nivel
class ConfiguracionInvalidaException extends Exception {
    public ConfiguracionInvalidaException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class SistemaConfiguracion {
    
    public void cargarConfiguracion(String rutaArchivo) throws ConfiguracionInvalidaException {
        try {
            // Intento de operación de bajo nivel
            FileReader lector = new FileReader(rutaArchivo);
            BufferedReader buffer = new BufferedReader(lector);
            // ... parsear configuración ...
            
        } catch (FileNotFoundException e) {
            // Capturar excepción de bajo nivel y encapsularla
            throw new ConfiguracionInvalidaException(
                "No se pudo cargar el archivo de configuración del sistema", 
                e  // La excepción original se guarda como causa
            );
        } catch (IOException e) {
            throw new ConfiguracionInvalidaException(
                "Error al leer la configuración del sistema", 
                e
            );
        }
    }
    
    public static void main(String[] args) {
        SistemaConfiguracion sistema = new SistemaConfiguracion();
        try {
            sistema.cargarConfiguracion("config.properties");
        } catch (ConfiguracionInvalidaException e) {
            System.err.println("Error capturado: " + e.getMessage());
            e.printStackTrace();
            
            // También se puede acceder programáticamente a la causa
            Throwable causa = e.getCause();
            if (causa != null) {
                System.err.println("Causa original: " + causa.getClass().getName());
            }
        }
    }
}
```

En este ejemplo, cuando el archivo no existe, se lanza `FileNotFoundException` (bajo nivel), que es capturada y encapsulada en `ConfiguracionInvalidaException` (alto nivel). Al imprimir el stack trace, se vería primero la excepción de configuración con su mensaje de alto nivel, seguido de "Caused by: java.io.FileNotFoundException" con el mensaje y stack trace original. Esto permite que el código de alto nivel maneje excepciones de negocio sin perder los detalles técnicos necesarios para la depuración.

