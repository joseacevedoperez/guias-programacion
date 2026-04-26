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

### En C y en Java existen mecanismos para poder almacenar distintos tipos de datos en una misma estructura, aunque se haga de forma diferente. En C, esto se consigue usando punteros a void*, que pueden apuntar a cualquier tipo de dato, mientras que en Java se usa la clase base Object, de la cual heredan todas las clases. Desde el punto de vista de un alumno de primero, ambos enfoques permiten escribir estructuras “genéricas”, pero con niveles de seguridad muy distintos.

### En C, un void* no tiene tipo asociado, así que el compilador no hace comprobaciones de tipo. Esto da mucha flexibilidad, pero también es peligroso, porque el programador debe recordar qué tipo real hay detrás y hacer el casting correcto al recuperar el dato. Un ejemplo sencillo es un array de void* que puede almacenar enteros, double o cualquier otra cosa, siempre que se guarden como direcciones.

### #include <stdio.h>

### int main() {
###    void* array[3];

###    int a = 10;
###    double b = 3.14;
###    char c = 'X';

###    array[0] = &a;
###    array[1] = &b;
###    array[2] = &c;

###    printf("%d\n", *(int*)array[0]);
###    printf("%f\n", *(double*)array[1]);
###    printf("%c\n", *(char*)array[2]);

###    return 0;
### }

### En Java, el mismo concepto se consigue usando un array de Object, pero con mucha más seguridad. Todas las clases heredan de Object, así que se pueden guardar referencias a cualquier objeto. Eso sí, al recuperar el elemento, también es necesario hacer casting, aunque Java comprueba tipos en tiempo de ejecución y lanza excepciones si algo va mal. En resumen, void* en C y Object en Java cumplen un papel parecido, pero Java sacrifica flexibilidad a cambio de seguridad, algo muy acorde con su filosofía.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### La programación genérica consiste en escribir código que sea independiente del tipo de datos concreto con el que trabaja. La idea es definir estructuras y algoritmos reutilizables que funcionen igual para distintos tipos, evitando duplicar código para cada caso. En primero suele explicarse como una forma de programar más flexible y más limpia, porque el mismo código sirve para muchos tipos diferentes sin modificarlo.

### El ejemplo anterior con void* en C o Object en Java no es programación genérica “real”, sino una aproximación básica a ese concepto. Es verdad que permite almacenar datos de cualquier tipo, pero se pierde seguridad de tipos, ya que el compilador no puede comprobar si los casts son correctos. Por eso, los errores suelen aparecer en tiempo de ejecución y no en compilación.

### La programación genérica propiamente dicha busca precisamente mantener la flexibilidad sin perder seguridad. En C se consigue mediante macros o void* con mucho cuidado, y en Java se introduce más adelante con los genéricos (<T>), que permiten definir clases y métodos genéricos de forma segura. Así que el ejemplo anterior ayuda a entender la idea, pero no se considera un ejemplo completo de programación genérica, sino un primer paso conceptual hacia ella.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Cuando se usan void* en C o Object en Java para crear estructuras de datos genéricas, el principal problema es que se pierde el chequeo de tipos en tiempo de compilación. El compilador ya no puede asegurar que los datos que se meten y se sacan de la estructura sean del tipo correcto, así que muchos errores que podrían detectarse antes pasan a aparecer en tiempo de ejecución, que es más peligroso y más difícil de depurar.

### Otro problema importante es la necesidad de hacer casting explícito al recuperar los datos. El programador tiene que recordar siempre qué tipo real se almacenó y hacer la conversión adecuada. Si se hace un cast incorrecto, en C puede provocar comportamiento indefinido (fallos de memoria) y en Java una excepción ClassCastException. Esto hace que el código sea más frágil y propenso a errores, sobre todo en programas grandes.

### Además, este enfoque empeora la legibilidad y mantenibilidad del código, ya que no queda claro qué tipo de datos se espera realmente en cada estructura. Cualquier cambio en el tipo usado puede obligar a revisar muchos casts repartidos por el programa. Por eso, aunque void* y Object permiten una especie de genericidad, se consideran soluciones poco seguras, y de ahí la importancia de la programación genérica con genéricos reales, que mantienen la flexibilidad sin renunciar al control de tipos.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Los parámetros de tipo son el mecanismo principal que introduce la programación genérica “correcta”, sobre todo en lenguajes como Java. Consisten en usar tipos abstractos (normalmente letras como T, E, K, V) que actúan como marcadores de tipo, y que se sustituyen por un tipo concreto cuando se usa la clase o el método. Así, el código se escribe una sola vez, pero funciona para distintos tipos de datos de forma segura.

### La gran ventaja de los parámetros de tipo es que el compilador sí puede comprobar los tipos, a diferencia de lo que pasaba con void* u Object. Esto significa que los errores de tipo se detectan en tiempo de compilación y no en ejecución, y además no es necesario hacer casting al recuperar los datos. Desde primero, se puede entender como una forma de decirle al compilador: “esto va a ser genérico, pero siempre coherente con un mismo tipo”.

### Gracias a los parámetros de tipo, se pueden definir estructuras como listas, pilas o colas que funcionen con cualquier tipo sin perder seguridad. Por ejemplo, una estructura genérica garantiza que si se guarda un tipo concreto, solo se podrán recuperar elementos de ese mismo tipo. En resumen, los parámetros de tipo son una mejora clave porque combinan flexibilidad y seguridad, resolviendo los principales problemas que tenían las soluciones basadas en void* o Object.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### En Java y en C++ la programación genérica se usa para crear estructuras de datos que funcionen con un tipo concreto de forma segura, sin perder flexibilidad. En Java esto se hace con generics usando parámetros de tipo como <T>, mientras que en C++ se hace con templates. En ambos casos, la idea es que el compilador garantice que todos los elementos almacenados son del mismo tipo y que no haya errores de conversión.

### En Java, podemos crear una lista que solo admita String usando ArrayList<String>. El compilador impide meter objetos de otro tipo y, al recorrer la lista, sabemos con seguridad que cada elemento es un String, sin necesidad de hacer casting. Esto es una mejora clara frente a usar Object.

### import java.util.ArrayList;

### public class Main {
###    public static void main(String[] args) {
###        ArrayList<String> lista = new ArrayList<>();

###        lista.add("Hola");
###        lista.add("Programación");
###        lista.add("Genérica");

###        for (String s : lista) {
###            System.out.println(s.toUpperCase());
###        }
###    }
### }

### En C++, el mismo concepto se aplica usando templates, por ejemplo con std::vector<std::string>. Aquí también el compilador asegura que el vector solo contiene string y permite usarlos directamente como tal. Si se intenta insertar otro tipo, el error aparece en compilación, lo cual es muy seguro.

### #include <iostream>
### #include <vector>
### #include <string>

### int main() {
###    std::vector<std::string> v;

###    v.push_back("Hola");
###    v.push_back("Programacion");
###    v.push_back("Generica");

###    for (const std::string& s : v) {
###        std::cout << s << std::endl;
###    }

###    return 0;
### }

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Cuando se instancia una clase genérica, el compilador actúa de forma distinta según el lenguaje. La idea general es que el compilador adapta la clase o estructura genérica al tipo concreto que se está usando, para mantener la seguridad de tipos. Desde primero se explica como que el compilador “especializa” la clase genérica para el tipo indicado, aunque el cómo se hace exactamente cambia bastante entre Java y C++.

### En Java, el compilador utiliza lo que se llama type erasure (borrado de tipos). Esto significa que en tiempo de compilación se comprueba que los tipos son correctos, pero en tiempo de ejecución la información genérica desaparece. Internamente, la clase genérica se trata como si usara Object (o el límite superior indicado). Por eso en ejecución no se puede preguntar, por ejemplo, si una lista es ArrayList<String> o ArrayList<Integer>, porque esa información ya no existe; solo queda que es un ArrayList.

### En C++, en cambio, los templates sí generan código real distinto para cada tipo concreto que se utiliza. Esto se llama instanciación de plantillas: si se usa vector<int> y vector<string>, el compilador genera dos versiones diferentes del código. Gracias a esto, en C++ los tipos genéricos existen plenamente en compilación y no hay borrado de tipos, lo que hace el sistema más potente pero también puede aumentar el tamaño del ejecutable.

### En resumen, Java y C++ no hacen lo mismo aunque el concepto de programación genérica sea parecido. Java apuesta por el type erasure para mantener compatibilidad y simplicidad en tiempo de ejecución, mientras que C++ opta por generar código específico para cada tipo mediante plantillas, con más potencia pero más complejidad. Entender esta diferencia ayuda mucho a comprender las limitaciones de los genéricos en Java frente a los templates de C++.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### En Java, una clase con parámetros de tipo permite definir estructuras que pueden trabajar con distintos tipos de datos manteniendo la seguridad de tipos. En este ejercicio se pide una clase Par que aloje dos valores de tipos diferentes, algo muy típico en programación genérica. Desde primero, se entiende como una generalización de una estructura que, en vez de fijar tipos concretos, los deja abiertos y los concreta cuando se usa la clase.

### La clase Par se puede definir usando dos parámetros de tipo, por ejemplo <T, U>. Cada uno representa un tipo distinto, y el compilador se encarga de que se usen de forma coherente. Incluimos un constructor para inicializar ambos valores y un getter para cada uno. Esto permite reutilizar la clase Par con cualquier combinación de tipos sin hacer casting ni perder seguridad.

### // Clase genérica Par
### public class Par<T, U> {
###    private T primero;
###    private U segundo;

###    public Par(T primero, U segundo) {
###        this.primero = primero;
###        this.segundo = segundo;
###    }

###    public T getPrimero() {
###        return primero;
###    }

###    public U getSegundo() {
###        return segundo;
###    }
### }

### Un uso típico de esta clase es como tipo de retorno de una función. Por ejemplo, podemos devolver en un Par<Double, Double> la media y la desviación típica de un array de double. El tipo de retorno deja claro qué se devuelve y con qué tipos, y el compilador garantiza que todo es coherente.

### public class Estadistica {

###    public static Par<Double, Double> mediaYDesviacion(double[] datos) {
###        double suma = 0.0;
###        for (double d : datos) {
###            suma += d;
###        }
###        double media = suma / datos.length;

###        double sumaCuadrados = 0.0;
###        for (double d : datos) {
###            sumaCuadrados += Math.pow(d - media, 2);
###        }
###        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

###        return new Par<>(media, desviacion);
###    }
### }


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### En Java, además de usar genéricos a nivel de clase, también se pueden declarar parámetros de tipo a nivel de método. Esto es muy útil cuando solo necesitamos comportamiento genérico en una función concreta y no en toda la clase. Un método genérico define su parámetro de tipo justo antes del tipo de retorno, por ejemplo <T>, y garantiza que todos los parámetros y el valor devuelto sean coherentes con ese mismo tipo, reforzando la seguridad en tiempo de compilación.

### Si definimos el método usando Object, el compilador no puede asegurar que los dos objetos sean del mismo tipo ni evitar el downcasting al recuperar el resultado. En cambio, usando parámetros de tipo, evitamos tener que hacer casts y, además, forzamos que ambos argumentos sean exactamente del mismo tipo. Esto elimina una fuente muy común de errores y hace el código más claro y seguro, algo que se valora mucho en programación genérica.

### import java.util.Random;

### public class Selector {

###    // Versión usando Object
###    public static Object seleccionaUnoObject(Object a, Object b) {
###        Random r = new Random();
###        return r.nextBoolean() ? a : b;
###    }

###    // Versión genérica
###    public static <T> T seleccionaUno(T a, T b) {
###        Random r = new Random();
###        return r.nextBoolean() ? a : b;
###    }

###    public static void main(String[] args) {
###        // Uso con Object (necesita casting)
###        String s1 = "Hola";
###        String s2 = "Adios";
###        String resultado1 = (String) seleccionaUnoObject(s1, s2);

###        // Uso con genéricos (sin casting y con seguridad de tipos)
###        String resultado2 = seleccionaUno(s1, s2);
###    }
### }


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Sí, en Java sí se pueden establecer restricciones en los parámetros de tipo, y se hace usando límites (bounds). Por ejemplo, si escribimos <T extends Number>, le estamos diciendo al compilador que T solo puede ser Number o una subclase suya (Integer, Double, Float, etc.). Esto es justo lo que necesitamos cuando queremos tratar un valor como un número y asegurarnos de que tiene métodos como doubleValue(), sin perder seguridad de tipos. Desde primero, se puede ver como “acotar” el genérico a una familia concreta de tipos.

### Una primera solución sencilla es definir las coordenadas directamente como Number. Esto permite usar cualquier tipo numérico, pero perdemos información concreta del tipo real (no sabemos si es Integer, Double, etc.), y todo se trata de forma más general. Funciona, pero no es la opción más fuerte en chequeo de tipos.

### // Solución 1: usando Number directamente
### public class Punto {
###    private Number x;
###    private Number y;

###    public Punto(Number x, Number y) {
###        this.x = x;
###        this.y = y;
###    }

###    public Number getX() {
###        return x;
###    }

###    public Number getY() {
###        return y;
###    }

###    public double calcularDistanciaA(Punto otro) {
###        double dx = this.x.doubleValue() - otro.x.doubleValue();
###        double dy = this.y.doubleValue() - otro.y.doubleValue();
###        return Math.sqrt(dx * dx + dy * dy);
###    }
### }

### La mejor solución usando genéricos es definir Punto<T extends Number>. Así seguimos permitiendo cualquier tipo de número, pero ahora el compilador sabe exactamente con qué tipo trabaja cada Punto, y obliga a que ambos puntos sean compatibles. Esto refuerza mucho el chequeo de tipos y evita errores de diseño.

### // Solución 2: usando genéricos con restricción
###  class Punto<T extends Number> {
###    private T x;
###    private T y;

###    public Punto(T x, T y) {
###        this.x = x;
###        this.y = y;
###    }

###    public T getX() {
###        return x;
###    }

###    public T getY() {
###        return y;
###    }

###    public double calcularDistanciaA(Punto<T> otro) {
###        double dx = this.x.doubleValue() - otro.x.doubleValue();
###        double dy = this.y.doubleValue() - otro.y.doubleValue();
###        return Math.sqrt(dx * dx + dy * dy);
###    }
### }

### Respecto al type erasure, tras la compilación el tipo genérico T desaparece y se reemplaza por su límite, en este caso Number. Es decir, en tiempo de ejecución el tipo final real es Number, no Integer ni Double. Aun así, el beneficio ya se ha obtenido en compilación, porque el compilador ha garantizado que todo el uso del tipo es correcto. Esto resume muy bien cómo Java combina genéricos con seguridad sin mantener información de tipos en ejecución.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Si comparamos las dos soluciones del ejercicio anterior, es verdad que ambas permiten trabajar con distintos tipos numéricos sin duplicar la clase Punto, pero el nivel de chequeo de tipos que ofrece cada una es muy distinto. En la solución sin genéricos, al usar directamente Number, el compilador permite crear un punto con una coordenada entera y otra real sin ningún problema, por ejemplo un Integer para x y un Double para y. Esto da mucha flexibilidad, pero también hace que el diseño sea menos estricto y más propenso a incoherencias.

### En la solución sin genéricos, el método getX() (y getY()) devuelve siempre un Number. Esto significa que, al usar el valor, el programador no sabe exactamente qué tipo real hay detrás y, si necesita algo específico de Integer o Double, tendría que hacer casting. El compilador no puede ayudar mucho aquí, porque toda la información concreta del tipo se pierde desde el diseño de la clase.

### En cambio, con la solución con genéricos (Punto<T extends Number>), el refuerzo del chequeo de tipos es mucho mayor. El compilador obliga a que ambas coordenadas sean del mismo tipo numérico, así que no se podría mezclar un entero y un real en el mismo punto. Además, getX() devuelve exactamente el tipo T concreto con el que se ha creado el punto (por ejemplo Integer o Double), lo que elimina ambigüedades y hace el código más claro y seguro.

### En resumen, ambas soluciones funcionan, pero la versión con genéricos expresa mucho mejor la intención del diseño y evita mezclas de tipos accidentales. La solución sin genéricos prioriza flexibilidad, mientras que la genérica prioriza seguridad y precisión en el chequeo de tipos, que es justo uno de los grandes objetivos de la programación genérica en Java.


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

### En este caso, el problema del código original es que la interfaz Punto no impide en compilación que se calcule la distancia entre puntos de distinto tipo, y por eso obliga a usar instanceof y downcasting. Para solucionarlo de forma “avanzada”, se puede usar genéricos autorreferenciados (también llamados F-bounded generics), de manera que cada tipo de punto solo pueda calcular la distancia con otro punto exactamente del mismo tipo. Así, el chequeo lo hace el compilador y no hace falta comprobar nada en tiempo de ejecución.

### La idea es definir la interfaz como Punto<T extends Punto<T>>, donde el parámetro de tipo representa “mi propio tipo”. Entonces el método distanciaA acepta directamente un T, no cualquier Punto. Cuando Punto2D implementa la interfaz, fija ese T como Punto2D, y lo mismo para Punto3D. De esta forma, Java garantiza que nunca se mezclarán tipos y podemos eliminar por completo instanceof y los casts.

### // Interfaz genérica autorreferenciada
### public interface Punto<T extends Punto<T>> {
###    double distanciaA(T p);
### }

### // Implementación 2D
### public class Punto2D implements Punto<Punto2D> {
###    private final double x, y;

###    public Punto2D(double x, double y) {
###        this.x = x;
###        this.y = y;
###    }

###    @Override
###    public double distanciaA(Punto2D p) {
###        return Math.sqrt(
###            Math.pow(x - p.x, 2) +
###            Math.pow(y - p.y, 2)
###        );
###    }
### }

### // Implementación 3D
### public class Punto3D implements Punto<Punto3D> {
###    private final double x, y, z;

###    public Punto3D(double x, double y, double z) {
###        this.x = x;
###        this.y = y;
###        this.z = z;
###    }

###    @Override
###    public double distanciaA(Punto3D p) {
###        return Math.sqrt(
###            Math.pow(x - p.x, 2) +
###            Math.pow(y - p.y, 2) +
###           Math.pow(z - p.z, 2)
###        );
###    }
### }

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Aunque String sea subtipo de Object, no significa que List<String> sea subtipo de List<Object>. En Java, los tipos genéricos son invariantes, lo que quiere decir que la relación de herencia no se propaga al parámetro de tipo. Esto se hace por seguridad: si List<String> fuera un List<Object>, se podría insertar cualquier Object (por ejemplo un Integer) en una lista que en realidad debería contener solo String, rompiendo la seguridad de tipos. Por eso el compilador lo prohíbe directamente.

### Sin embargo, con los arrays, la respuesta es diferente: String[] sí es subtipo de Object[]. Los arrays en Java son covariantes, lo que significa que mantienen la relación de herencia de sus elementos. El problema es que esto puede provocar errores en tiempo de ejecución: aunque el compilador lo permita, si se intenta meter un objeto que no es String dentro de un Object[] que en realidad apunta a un String[], Java lanzará una ArrayStoreException. Es decir, el error no se detecta al compilar, sino al ejecutar el programa.

### A partir de aquí se entiende la diferencia de conceptos. Un tipo genérico es covariante si al cumplirse que A es subtipo de B, entonces Gen<A> es subtipo de Gen<B> (como pasa con los arrays). Es contravariante si ocurre al revés, y es invariante cuando no hay relación de subtipado en absoluto entre Gen<A> y Gen<B>, aunque A sea subtipo de B (que es el caso de List en Java).

### La conclusión, desde el punto de vista de primero, es que Java elige la invariancia para los genéricos porque garantiza la seguridad de tipos en compilación, mientras que los arrays, al ser covariantes, arrastran un diseño antiguo que obliga a detectar ciertos errores en tiempo de ejecución. Por eso los genéricos son más seguros y se prefieren claramente frente a los arrays cuando se trabaja con colecciones.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### En Java, un wildcard (?) es una forma de decir “algún tipo que no conozco exactamente”, pero con ciertas restricciones. Se usan para recuperar parte de la covarianza o contravarianza que los genéricos normales no permiten. La idea clave es que, dependiendo de cómo se declare el wildcard, el compilador nos deja leer de una colección o escribir en ella, pero no ambas cosas libremente. Esto sigue la regla clásica: PECS → Producer Extends, Consumer Super.

### List<? extends T> se usa cuando la lista produce valores de tipo T o subtipos de T. Es decir, podemos leer elementos y tratarlos como T, pero no podemos añadir nada (salvo null), porque no sabemos el subtipo exacto. Esto es ideal para operaciones de solo lectura, como calcular una suma. En cambio, List<? super T> se usa cuando la lista consume valores de tipo T: podemos añadir objetos de tipo T o subtipos, pero al leer solo sabemos que obtenemos un Object, porque podría ser una superclase de T.

### // (i) Usando ? extends: solo lectura
### public static double suma(List<? extends Number> lista) {
###    double total = 0.0;
###    for (Number n : lista) {
###        total += n.doubleValue();
###    }
###    return total;
### }

### // (ii) Usando ? super: escritura segura
### public static void añadeEnteros(List<? super Integer> lista) {
###    lista.add(1);
###    lista.add(2);
###    lista.add(3);
### }