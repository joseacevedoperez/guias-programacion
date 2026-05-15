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

### Un puntero a una función es un concepto típico de C, pero en Java no existen punteros como tal. Sin embargo, Java tiene algo muy parecido que son las referencias a métodos o el uso de interfaces funcionales (lambdas). Básicamente, en Java puedes guardar una función en una variable usando una interfaz y luego llamarla, que es el comportamiento más cercano a un puntero a función.

### En este caso, lo que vamos a hacer es crear un método que convierta una cadena a mayúsculas y luego usar una interfaz funcional para “apuntar” a ese método, igual que haríamos con un puntero en C. Esa referencia la llamaremos aMayusculas y la usaremos para ejecutar la función.

### import java.util.function.Function;

### public class Main {

###    // Método que convierte una cadena a mayúsculas
###    public static String convertirMayusculas(String cadena) {
###        return cadena.toUpperCase();
###    }

###    public static void main(String[] args) {

###        String texto = "hola mundo";

###        // "Puntero" a función usando una referencia a método
###        Function<String, String> aMayusculas = Main::convertirMayusculas;

###        // Llamada a la función usando la referencia
###        String resultado = aMayusculas.apply(texto);

###        // Mostrar resultado
###        System.out.println(resultado);
###    }
### }

### En este código, aMayusculas actúa como si fuera un puntero a la función convertirMayusculas. En vez de llamarla directamente, usamos aMayusculas.apply(texto). Al ejecutarlo, se imprime HOLA MUNDO. 



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Una función lambda es una función anónima (sin nombre) que se puede definir directamente en una expresión y asignar a una variable o pasar como argumento. Sirve para escribir código más compacto y flexible, sobre todo cuando queremos usar funciones “rápidas” sin tener que declararlas aparte. Es muy común en programación moderna porque permite tratar funciones como si fueran datos.

### En relación con el ejercicio anterior, en vez de usar una función normal, ahora creamos directamente una función lambda que convierta una cadena a mayúsculas y la guardamos en una variable llamada aMayusculas. Después la usamos igual que antes, como si fuera una función.

### // Función lambda (arrow function)
### let aMayusculas = (cadena) => {
###    return cadena.toUpperCase();
### };

### let texto = "hola mundo";

### // Llamada a la función
### let resultado = aMayusculas(texto);

### console.log(resultado);

### import java.util.function.Function;

### public class Main {
###    public static void main(String[] args) {

###        String texto = "hola mundo";

###        // Función lambda en Java
###        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

###        // Llamada a la función
###        String resultado = aMayusculas.apply(texto);

###        // Mostrar resultado
###        System.out.println(resultado);
###    }
### }

### En ambos casos, aMayusculas es una variable local que guarda una función lambda. En JavaScript se usa directamente como función, mientras que en Java se usa apply() porque estamos usando la interfaz Function<String, String>. El resultado en los dos casos será "HOLA MUNDO".


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### El paradigma funcional es una forma de programar en la que se usan funciones como elemento principal del programa, evitando cambiar el estado de las variables y tratando las funciones como si fueran valores. Es decir, en vez de modificar datos paso a paso, se aplican funciones que reciben datos y devuelven nuevos resultados. Esto hace que el código sea más claro, más fácil de razonar y con menos errores relacionados con cambios inesperados de estado.

### A lenguajes como Java 8 se les llama multi-paradigma porque permiten usar varios estilos de programación a la vez, por ejemplo, el paradigma orientado a objetos (clases, objetos) y el funcional (lambdas, streams). Antes Java era más puramente orientado a objetos, pero desde la versión 8 se añadieron funciones lambda y otras herramientas funcionales, lo que permite al programador elegir el enfoque que mejor se adapte a cada problema.

### Por último, decir que las funciones son “ciudadanos de primera clase” significa que se pueden usar como cualquier otro dato: se pueden guardar en variables, pasar como parámetros a otras funciones y devolver como resultado. Esto es clave en el paradigma funcional, porque permite escribir código más flexible y reutilizable. Por ejemplo, en Java con lambdas podemos asignar una función a una variable y usarla donde queramos, igual que hicimos con aMayusculas.


## 4. Explica la sintaxis básica de una función lambda en Java.

### La sintaxis básica de una función lambda en Java es una forma abreviada de escribir funciones sin necesidad de crear un método completo. Tiene la forma general: (parametros) -> expresion

### o si hay varias instrucciones: (parametros) -> {
###    // varias líneas de código
### }

### Primero van los parámetros entre paréntesis, después el símbolo -> (que se lee como “flecha”), y luego el cuerpo de la función. Si solo hay una expresión, no hace falta poner {} ni return, pero si hay varias instrucciones, sí se usan llaves y return si es necesario. En Java, las lambdas siempre se usan junto con interfaces funcionales, como Function, Predicate, etc.

### En resumen, la sintaxis de las lambdas permite escribir funciones de forma más corta y clara. Lo importante es entender la estructura (parámetros) -> cuerpo y que siempre necesitan un tipo de referencia funcional como Function<String, String> para poder usarlas.



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### En este ejercicio se trata de pasar una función como parámetro a otro método, lo cual es una idea típica del paradigma funcional. Es decir, en vez de que un método haga siempre lo mismo, le pasamos una función (como aMayusculas) y así puede aplicar distintas transformaciones según lo que le pasemos. Esto hace el código más reutilizable y flexible.

### La idea es crear un método transformar que reciba un String y una función transformadora, y dentro de ese método llamar a la función que se ha pasado como parámetro. Vamos a hacerlo tanto en JavaScript como en Java.

### // Función transformar
### function transformar(texto, funcion) {
###    return funcion(texto);
### }

### // Función lambda
### let aMayusculas = (cadena) => cadena.toUpperCase();

### let resultado = transformar("hola mundo", aMayusculas);

### console.log(resultado);

### import java.util.function.Function;

### public class Main {

###    // Método transformar
###    public static String transformar(String texto, Function<String, String> funcion) {
###        return funcion.apply(texto);
###    }

###    public static void main(String[] args) {

###        // Función lambda
###        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

###        String resultado = transformar("hola mundo", aMayusculas);

###        System.out.println(resultado);
###    }
### }

### En ambos casos, el método transformar recibe una cadena y una función, y la ejecuta dentro. La variable aMayusculas es la función que se pasa como argumento, y se invoca dentro del método. El resultado final es "HOLA MUNDO".



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### En este ejercicio seguimos con la misma idea que antes, pero en vez de crear la función lambda en una variable, ahora la definimos directamente en la llamada al método transformar. Esto es muy típico en programación funcional, porque permite escribir código más compacto cuando la función solo se va a usar una vez.

### La función que vamos a pasar como parámetro será una lambda que invierte la cadena. Es decir, recibe un String y devuelve ese mismo texto pero al revés. Lo importante es que la lambda se define justo dentro de la llamada a transformar.

### // Función transformar
### function transformar(texto, funcion) {
###    return funcion(texto);
### }

### // Llamada con lambda directamente
### let resultado = transformar("hola mundo", (cadena) => {
###    return cadena.split("").reverse().join("");
### });

### console.log(resultado);

### import java.util.function.Function;

### public class Main {

###    // Método transformar
###    public static String transformar(String texto, Function<String, String> funcion) {
###        return funcion.apply(texto);
###    }

###    public static void main(String[] args) {

###        // Llamada con lambda directamente
###        String resultado = transformar("hola mundo", (cadena) -> {
###            return new StringBuilder(cadena).reverse().toString();
###        });

###        System.out.println(resultado);
###    }
### }

### En ambos casos, la lambda no se guarda en ninguna variable, sino que se crea directamente al llamar a transformar. Esto hace el código más directo y es útil cuando la función solo se usa una vez. El resultado final será "odnum aloh", ya que la cadena se invierte completamente.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Un closure (cierre) en el contexto de funciones lambda es cuando una función puede acceder a variables que están fuera de ella, es decir, variables del entorno donde fue creada. Aunque la función se ejecute en otro sitio, “recuerda” esas variables externas. En Java, esto se puede hacer siempre que esas variables sean finales o efectivamente finales (que no cambien de valor después de asignarse).

### En este ejercicio vamos a modificar el ejemplo anterior para que la lambda no solo transforme la cadena, sino que además use una variable externa para añadir texto. Así se demuestra el concepto de closure, porque la función lambda está usando una variable que no está dentro de ella, sino fuera.

### import java.util.function.Function;

### public class Main {

###    // Método transformar
###    public static String transformar(String texto, Function<String, String> funcion) {
###        return funcion.apply(texto);
###    }

###    public static void main(String[] args) {

###        String sufijo = "!!!";  // Variable local externa (closure)

###        // Llamada con lambda que usa la variable externa
###        String resultado = transformar("hola mundo", (cadena) -> {
###            return cadena + sufijo;
###        });

###        System.out.println(resultado);
###    }
### }

### En este código, la variable sufijo está definida fuera de la lambda, pero la lambda puede usarla igualmente. Eso es un closure, porque la función “captura” esa variable del entorno. El resultado final será "hola mundo!!!", ya que la lambda concatena el texto original con el sufijo externo.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### La principal diferencia entre una función lambda y un puntero a función en C es el nivel de abstracción y las capacidades que ofrecen. Un puntero a función simplemente guarda la dirección de memoria de una función y permite llamarla de forma indirecta. Es algo más “bajo nivel” y más cercano a cómo funciona el hardware. En cambio, una función lambda es un concepto más moderno y potente, que no solo representa una función, sino que puede comportarse como un objeto y trabajar con el paradigma funcional.

### Otra diferencia importante es que las lambdas pueden capturar variables del entorno (closures), mientras que los punteros a funciones en C no tienen esa capacidad de forma directa. Es decir, en Java una lambda puede usar variables externas sin necesidad de pasarlas como parámetro, mientras que en C eso no ocurre, tendrías que hacerlo manualmente pasando argumentos adicionales o usando estructuras. Esto hace que las lambdas sean más expresivas y cómodas para ciertos tipos de problemas.

### Además, en lenguajes como Java, las lambdas están integradas con el sistema de tipos (interfaces funcionales), lo que permite escribir código más seguro y reutilizable. En cambio, en C los punteros a función son más simples y menos seguros, ya que dependen mucho del programador para que coincidan los tipos correctamente. En resumen, los punteros a función son una herramienta básica para llamar funciones de forma indirecta, mientras que las lambdas son una evolución más avanzada que permite programación funcional y tratamiento de funciones como datos.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### En este ejercicio estamos trabajando con funciones que devuelven otras funciones, lo cual es típico del paradigma funcional. La idea es que crearDescuento reciba un porcentaje (por ejemplo, 0.2 para un 20%) y devuelva una función que, cuando se use, aplique ese descuento a una cantidad. Esto es muy potente porque podemos generar funciones personalizadas.

### Además, aquí se ve claramente el concepto de closure, porque la función que devolvemos “recuerda” el valor de porcentaje aunque el método crearDescuento ya haya terminado. Es decir, el valor queda “capturado” dentro de la lambda.

### import java.util.function.Function;

### public class Main {

###    // Función que crea funciones de descuento
###    public static Function<Double, Double> crearDescuento(double porcentaje) {
###        return (precio) -> precio - (precio * porcentaje);
###    }

###    public static void main(String[] args) {

###        // Crear dos descuentos distintos
###        Function<Double, Double> descuento20 = crearDescuento(0.20);
###        Function<Double, Double> descuento50 = crearDescuento(0.50);

###        // Aplicarlos a una cantidad
###        double precio = 100.0;

###        double resultado1 = descuento20.apply(precio);
###        double resultado2 = descuento50.apply(precio);

###        System.out.println("Precio con 20%: " + resultado1);
###        System.out.println("Precio con 50%: " + resultado2);
###    }
### }

### En este código, crearDescuento devuelve una lambda que usa la variable porcentaje. Aunque esa variable está fuera de la lambda, queda guardada dentro de ella, lo que es el closure. Por ejemplo, descuento20 “recuerda” que su porcentaje es 0.20 y lo aplica cuando se usa. Esto permite crear funciones especializadas sin tener que escribirlas todas a mano.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Una interfaz funcional en Java es una interfaz que tiene un único método abstracto. Se usa principalmente como el tipo de las funciones lambda, ya que Java necesita saber qué tipo tiene esa lambda (porque es un lenguaje con tipado estático). Es decir, cuando escribimos una lambda, en realidad estamos creando una implementación de ese único método de la interfaz.

### Para que una interfaz sea funcional tiene algunos requisitos importantes. El principal es que solo puede tener un método abstracto (aunque puede tener más métodos default o static, eso sí está permitido). Además, suele marcarse con la anotación @FunctionalInterface, que no es obligatoria pero ayuda al compilador a detectar errores si añadimos más métodos abstractos sin querer.

### Un ejemplo típico es Function<T, R>, que tiene un único método apply(). Cuando hacemos algo como:

### Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

### esa lambda está implementando el método apply de la interfaz. En resumen, las interfaces funcionales son la base que permite usar lambdas en Java, porque les dan un tipo concreto y una estructura que seguir.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Una interfaz funcional creada a mano es simplemente una interfaz que definimos nosotros mismos y que cumple la condición de tener un solo método abstracto. Esto es útil cuando queremos definir nuestro propio tipo de función en vez de usar las interfaces ya existentes como Function. En este caso, vamos a crear una interfaz llamada Transformador, que represente una función que recibe un String y devuelve otro String.

### Lo importante es que esta interfaz solo tenga un método, por ejemplo transformar, que será el que implementen las funciones lambda. Luego podremos usar esa interfaz como tipo para nuestras lambdas, igual que hacíamos antes con Function<String, String>.

### // Definición de la interfaz funcional
### @FunctionalInterface
### interface Transformador {
###    String transformar(String texto);
### }

### public class Main {

###    // Método que usa la interfaz
###    public static String transformarTexto(String texto, Transformador t) {
###        return t.transformar(texto);
###    }

###    public static void main(String[] args) {

###        // Lambda usando nuestra interfaz
###        Transformador aMayusculas = (cadena) -> cadena.toUpperCase();

###        String resultado = transformarTexto("hola mundo", aMayusculas);

###        System.out.println(resultado);
###    }
### }

### En este código, Transformador es una interfaz funcional porque tiene un único método (transformar). Luego usamos una lambda para implementar ese método sin necesidad de crear una clase. Esto demuestra cómo podemos crear nuestros propios tipos de funciones en Java y usarlos de forma flexible, igual que con las interfaces ya definidas.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Para hacer la interfaz más genérica, usamos generics en Java, es decir, tipos parametrizados. En vez de limitar la interfaz a String -> String, definimos la interfaz con dos tipos <T, R>, donde T es el tipo de entrada y R el tipo de salida. Así podemos reutilizar la misma interfaz para transformar cualquier tipo de dato en otro.

### La sintaxis sería algo así como Transformador<T, R>, con un único método que reciba un T y devuelva un R. De esta forma, podemos crear funciones mucho más flexibles, por ejemplo una que convierta un Double en un Integer, como pide el ejercicio.

### // Interfaz funcional genérica
### @FunctionalInterface
### interface Transformador<T, R> {
###    R transformar(T valor);
### }

### public class Main {

###    public static void main(String[] args) {

###        // Lambda que redondea un Double a Integer
###        Transformador<Double, Integer> redondear = (numero) -> (int) Math.round(numero);

###        Double valor = 3.7;

###        Integer resultado = redondear.transformar(valor);

###        System.out.println(resultado);
###    }
### }

### En este código, la interfaz Transformador<T, R> puede usarse con cualquier tipo. En el ejemplo concreto, usamos Double como entrada y Integer como salida. La lambda implementa el método transformar, redondeando el número con Math.round. Esto demuestra cómo los generics permiten crear funciones mucho más reutilizables y adaptables a distintos casos.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### La interfaz Transformador<T, R> que hemos creado es muy útil, pero en realidad en Java ya existe una interfaz funcional estándar que hace exactamente lo mismo: Function<T, R>. Esta interfaz forma parte del paquete java.util.function y se usa para representar funciones que reciben un dato de tipo T y devuelven otro de tipo R. Por eso, en la práctica no suele ser necesario crear una interfaz propia como Transformador.

### Java incluye varias interfaces funcionales predefinidas para cubrir los casos más comunes. Algunas de las más importantes son:

### Function<T, R> → recibe un valor y devuelve otro
### Predicate<T> → recibe un valor y devuelve un boolean (true/false)
### Consumer<T> → recibe un valor y no devuelve nada
### Supplier<T> → no recibe nada y devuelve un valor

### import java.util.function.Function;

### public class Main {
###    public static void main(String[] args) {

###        // Función que redondea un Double a Integer
###        Function<Double, Integer> redondear = (numero) -> (int) Math.round(numero);

###        Double valor = 4.6;

###        Integer resultado = redondear.apply(valor);

###        System.out.println(resultado);
###    }
### }

### En este ejemplo usamos directamente Function<T, R> en lugar de nuestra interfaz Transformador. La lambda implementa el método apply, que es el único método abstracto de esta interfaz. En resumen, Java ya trae muchas interfaces funcionales listas para usar, por lo que normalmente es mejor reutilizarlas en vez de crear nuevas, a menos que necesitemos algo muy específico.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### El método forEach de List en Java es una forma funcional de recorrer colecciones sin necesidad de usar el bucle for tradicional. En lugar de escribir la estructura del bucle manualmente, le pasamos una función (normalmente una lambda) que se ejecuta para cada elemento de la lista. Esto hace el código más limpio y más acorde al paradigma funcional.

### En este ejercicio, tenemos que recorrer una lista de Integer y mostrar un mensaje solo si el número es positivo. Para ello usamos forEach junto con una lambda que comprueba la condición dentro.

### import java.util.Arrays;
### import java.util.List;

### public class Main {
###    public static void main(String[] args) {

###        List<Integer> numeros = Arrays.asList(3, -1, 7, 0, -5, 10);

###        // Uso de forEach con lambda
###        numeros.forEach(numero -> {
###            if (numero > 0) {
###                System.out.println("El número " + numero + " es positivo");
###            }
###        });
###    }
### }

### En este código, forEach recorre automáticamente todos los elementos de la lista numeros. Para cada elemento, se ejecuta la lambda (numero -> {...}). Dentro de esa lambda comprobamos si el número es mayor que 0, y solo en ese caso se imprime el mensaje. Esto sustituye al típico for y muestra cómo Java permite escribir código más expresivo usando programación funcional.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### En la firma del método forEach se usa Consumer<? super T> en lugar de Consumer<T> por el uso de genéricos con comodines (wildcards). El ? super T significa “cualquier tipo que sea T o una superclase de T”. Esto aporta más flexibilidad, ya que permite pasar funciones que trabajen no solo exactamente con T, sino también con tipos más generales.

### Esto está relacionado con el principio PECS (Producer Extends, Consumer Super). En este caso, como Consumer consume datos (recibe elementos de la lista), se usa super. Es decir:

### Si una estructura produce datos → extends
### Si una estructura consume datos → super

### En forEach, el Consumer recibe elementos de la lista, por eso es un consumidor y se usa ? super T. Así puedes pasar, por ejemplo, un Consumer<Object> a una lista de Integer, porque Object es supertipo de Integer.

### Aplicando esto al método transformar, podemos mejorarlo usando también genéricos con PECS para hacerlo más flexible:

### import java.util.function.Function;

### public class Main {

###    // Método transformar mejorado con PECS
###    public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
###        return funcion.apply(valor);
###    }

###    public static void main(String[] args) {

###        // Función que acepta Object (super de String) y devuelve String
###        Function<Object, String> funcion = (obj) -> obj.toString().toUpperCase();

###        String resultado = transformar("hola mundo", funcion);

###        System.out.println(resultado);
###    }
### }

### En este código, Function<? super T, ? extends R> indica que la función puede aceptar un tipo más general que T y devolver un tipo más específico que R. Esto hace el método más reutilizable. En resumen, PECS ayuda a diseñar genéricos más flexibles y seguros, diferenciando si un tipo se usa para consumir o producir datos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Las referencias a métodos permiten guardar un método en una variable y usarlo más tarde, sin necesidad de llamarlo directamente. Es parecido a lo que hemos hecho con las lambdas, pero en vez de escribir la función manualmente, usamos un método que ya existe. En Java se hace con :: y en JavaScript con funciones ligadas al objeto.

### En este ejercicio se pide crear una clase Persona con un método saludar, y luego obtener una referencia a ese método desde un objeto y llamarlo usando esa referencia.

### class Persona {
###    constructor(nombre) {
###        this.nombre = nombre;
###    }

###    saludar() {
###        console.log("Hola, soy " + this.nombre);
###    }
### }

### // Crear objeto
### let persona = new Persona("Jose");

### // Obtener referencia al método
### let refSaludar = persona.saludar.bind(persona);

### // Llamar al método mediante la referencia
### refSaludar();

### import java.util.function.Consumer;

### class Persona {
###    String nombre;

###    public Persona(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### public class Main {
###    public static void main(String[] args) {

###        Persona p = new Persona("Jose");

###        // Referencia a método
###        Runnable refSaludar = p::saludar;

###        // Invocar el método usando la referencia
###        refSaludar.run();
###    }
### }

### En ambos ejemplos, guardamos el método saludar en una variable (refSaludar) y lo ejecutamos después. En Java se usa :: para crear la referencia al método, mientras que en JavaScript se usa bind para asegurar el contexto del objeto. Esto hace el código más flexible y reutilizable sin tener que escribir lambdas manualmente.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### En Java hay varios tipos de referencias a método, y se diferencian en función de a qué tipo de método apuntan. Los principales tipos son: referencia a método estático, referencia a método de instancia de un objeto concreto, referencia a método de instancia de cualquier objeto de un tipo, y referencia a constructor. Todos se escriben con el operador :: y permiten usar métodos ya existentes sin tener que escribir una lambda.

### Vamos a ver ejemplos de cada tipo para entenderlo mejor:

### import java.util.function.Function;

### public class Main {
###    public static int doble(int x) {
###        return x * 2;
###    }

###    public static void main(String[] args) {

###        Function<Integer, Integer> f = Main::doble;

###        System.out.println(f.apply(5)); // 10
###    }
### }

### import java.util.function.Function;

### class Persona {
###    String nombre;

###    public Persona(String nombre) {
###        this.nombre = nombre;
###    }
### }

### public class Main {
###    public static void main(String[] args) {

###        Function<String, Persona> creador = Persona::new;

###        Persona p = creador.apply("Jose");
###        System.out.println(p.nombre);
###    }
### }

### class Persona {
###    String nombre;

###    public Persona(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### public class Main {
###    public static void main(String[] args) {

###        Persona p = new Persona("Jose");

###        Runnable r = p::saludar;

###        r.run();
###    }
### }

### import java.util.function.Function;

### public class Main {
###    public static void main(String[] args) {

###        Function<String, Integer> f = String::length;

###        System.out.println(f.apply("hola")); // 4
###    }
### }

### En resumen, estas referencias hacen lo mismo que una lambda pero de forma más corta y limpia. Cada tipo se usa en una situación distinta: si el método es estático, si queremos crear objetos, si ya tenemos una instancia concreta o si queremos aplicar el método a cualquier objeto de ese tipo. Esto ayuda a escribir código más legible y reutilizable.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### En este ejercicio se trata de usar programación funcional para ordenar una lista de objetos Persona. La idea es usar Collections.sort junto con una lambda como comparador, definiendo cómo se comparan dos personas: primero por edad, y si tienen la misma edad, por nombre en orden alfabético.

### Vamos a hacer dos versiones: una manual (comparando nosotros) y otra usando la interfaz Comparator, que ya tiene métodos útiles para simplificar esto.

### import java.util.*;

### class Persona {
###    String nombre;
###    int edad;

###    public Persona(String nombre, int edad) {
###        this.nombre = nombre;
###        this.edad = edad;
###    }
### }

### public class Main {
###    public static void main(String[] args) {

###        List<Persona> lista = new ArrayList<>();
###        lista.add(new Persona("Juan", 20));
###        lista.add(new Persona("Ana", 20));
###        lista.add(new Persona("Luis", 18));

###        // Ordenar con lambda manual
###        Collections.sort(lista, (p1, p2) -> {
###            if (p1.edad != p2.edad) {
###                return p1.edad - p2.edad;
###            } else {
###                return p1.nombre.compareTo(p2.nombre);
###            }
###        });

###        lista.forEach(p -> System.out.println(p.nombre + " " + p.edad));
###    }

### import java.util.*;

### class Persona {
###    String nombre;
###    int edad;

###    public Persona(String nombre, int edad) {
###        this.nombre = nombre;
###        this.edad = edad;
###    }
### }

### public class Main {
###    public static void main(String[] args) {

###        List<Persona> lista = new ArrayList<>();
###        lista.add(new Persona("Juan", 20));
###        lista.add(new Persona("Ana", 20));
###        lista.add(new Persona("Luis", 18));

###        // Ordenar usando Comparator
###        Collections.sort(lista,
###            Comparator.comparing((Persona p) -> p.edad)
###                      .thenComparing(p -> p.nombre)
###        );

###        lista.forEach(p -> System.out.println(p.nombre + " " + p.edad));
###    }
### }

### En la primera versión hacemos la comparación “a mano”, controlando los casos con if. En la segunda usamos Comparator.comparing y thenComparing, que hacen el código mucho más limpio y fácil de leer. En ambos casos, primero se ordena por edad, y si dos personas tienen la misma edad, se ordenan por nombre alfabéticamente.
