<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Para crear estructuras más complejas en C, normalmente se empieza definiendo componentes simples y luego combinándolos. En este caso, lo más básico sería el struct Punto, que simplemente guarda dos coordenadas: x e y. A partir de ahí, una línea se puede representar como dos puntos: un punto inicial y otro final. La idea es que “una línea tiene dos puntos”, así que estamos componiendo estructuras: primero definimos el struct Punto, y luego usamos dos de esos puntos dentro de un struct Linea.

### Una vez definidas las estructuras, es bastante natural escribir funciones que trabajen con ellas. Por ejemplo, para calcular la distancia entre dos puntos se puede usar la fórmula típica de la distancia euclidiana, usando sqrt y pow. Esta función sirve de base para calcular la longitud de una línea, porque la línea no es más que dos puntos. Por tanto, la longitud de la línea simplemente será la distancia entre su punto inicial y su punto final.

### A nivel práctico, combinar estructuras hace que el código sea mucho más organizado, porque cada parte representa algo del mundo real: un punto es un punto, una línea son dos puntos. Esto hace que las funciones también sean más fáciles de entender y de reutilizar. Además, si más adelante quisiéramos añadir más información a la línea (como un color o un grosor), bastaría con añadir campos al struct Linea sin tocar el resto del programa.
 
### #include <stdio.h>
#### #include <math.h>

### ty1pedef struct {
###    double x;
###    double y;
### } Punto;

### typedef struct {
###    Punto inicio;
###    Punto fin;
### } Linea;

### double distancia(Punto a, Punto b) {
###    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
### }

### double longitud_linea(Linea l) {
###    return distancia(l.inicio, l.fin);
### }

### int main() {
###    Punto p1 = {0.0, 0.0};
###    Punto p2 = {3.0, 4.0};
###    Linea l = {p1, p2};

###    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
###    printf("Longitud de la linea: %.2f\n", longitud_linea(l));

###    return 0;
### }


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Al pasar el ejemplo de C a orientación a objetos en Java, lo primero es pensar en cómo modelar los conceptos básicos como clases. En este caso, un punto es un objeto con dos coordenadas, y una línea es otro objeto que está formado por dos puntos. Esto refleja muy bien la idea de composición, porque una línea “tiene” dos puntos, igual que en el ejemplo de C una línea contenía dos structs de tipo Punto. Además, en Java podemos encapsular los atributos usando private, lo que nos permite controlar mejor cómo se usan y evitar que alguien modifique un punto sin querer.

### Una diferencia importante con respecto a C es que podemos hacer que los objetos sean inmutables. Como los puntos de una línea no deberían cambiar una vez creada, la clase Punto tiene atributos final y no ofrece métodos para modificarlos. Lo mismo pasa con la clase Linea: los dos puntos que la forman también son finales y se pasan por el constructor. Esto evita problemas y hace que el diseño sea más seguro, porque una línea creada siempre representará exactamente los mismos dos puntos.

### La clase Punto incluye un método para calcular la distancia a otro punto, aplicando la fórmula euclidiana. Luego, la clase Linea reutiliza ese mismo método para calcular su longitud, ya que la longitud de una línea es simplemente la distancia entre sus extremos. Con esto, el diseño queda muy modular: cada clase se encarga de lo suyo y no se mezclan responsabilidades. En general, este tipo de composición hace que el código sea más fácil de mantener y entender.

### class Punto {
###    private final double x;
###    private final double y;

###    public Punto(double x, double y) {
###        this.x = x;
###        this.y = y;
###    }

###    public double distancia(Punto otro) {
###        double dx = otro.x - this.x;
###        double dy = otro.y - this.y;
###        return Math.sqrt(dx*dx + dy*dy);
###    }
### }

### class Linea {
###    private final Punto inicio;
###    private final Punto fin;

###    public Linea(Punto inicio, Punto fin) {
###        this.inicio = inicio;
###        this.fin = fin;
###    }

###    public double longitud() {
###        return inicio.distancia(fin);
###    }
### }

### public class Main {
###    public static void main(String[] args) {
###        Punto p1 = new Punto(0, 0);
###        Punto p2 = new Punto(3, 4);
###        Linea l = new Linea(p1, p2);

###        System.out.println("Longitud de la línea: " + l.longitud());
###    }
### }


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### La multiplicidad en composición se refiere a cuántas instancias de un objeto están asociadas a otra dentro de la relación. Es una forma de expresar, por ejemplo, si un objeto A está compuesto por exactamente un objeto B, por varios, o incluso por un número variable. En programación orientada a objetos esto se usa mucho para representar cómo se construyen estructuras más grandes a partir de objetos más pequeños, igual que hicimos antes con puntos y líneas.

### En el ejemplo concreto, una Linea está compuesta exactamente por dos Punto. Esto significa que desde la perspectiva de Linea hacia Punto, la multiplicidad es 2. No puede ser ni 1 ni 3, porque una línea siempre necesita dos puntos para definirse: un punto inicial y un punto final. Como además hicimos la línea inmutable, estos dos puntos no cambian nunca después de su creación.

### Si miramos la dirección contraria, la multiplicidad de Punto hacia Linea es 0..1 si hablamos de la relación de composición. Un punto puede no pertenecer a ninguna línea, o puede pertenecer a una única línea (la que lo creó), porque la composición implica propiedad y vida conjunta: si la línea desaparece, los puntos que la componen también deberían desaparecer. Por eso no tendría sentido que un mismo punto formase parte de varias líneas bajo composición estricta, ya que entonces no estaría "poseído" por una sola.

### En resumen:

### De Linea a Punto: multiplicidad = 2 (una línea tiene exactamente dos puntos).

### De Punto a Linea: multiplicidad = 0..1 (un punto puede no estar en ninguna línea o pertenecer a una sola).


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### En orientación a objetos, hablamos de composición fuerte cuando un objeto “dueño” controla totalmente la existencia de los objetos que contiene. Es decir, si el objeto principal se destruye, también se destruyen las partes que lo componen. Este tipo de relación implica un ciclo de vida compartido: las partes no pueden existir por sí mismas. Por eso se dice que la composición fuerte es una relación muy estrecha, como cuando un Libro contiene Capítulos que no tienen sentido fuera del libro.

### En cambio, la composición débil (a veces llamada agregación) describe una relación más flexible donde el objeto “contenedor” usa o agrupa a otros objetos, pero no controla su vida. En este caso, si el objeto principal se destruye, las partes pueden seguir existiendo sin problema. Por ejemplo, un Aula puede tener una lista de Estudiantes, pero si el aula desaparece, los estudiantes obviamente siguen existiendo. Aquí, cada objeto mantiene su ciclo de vida independiente.

### La diferencia importante está en el ciclo de vida de los objetos:

### En composición fuerte, las partes dependen completamente del todo.
### En composición débil, las partes son independientes y solo están asociadas temporalmente o lógicamente.

### Por eso, solemos llamar “asociación” o “agregación” a la composición débil, porque los objetos simplemente están relacionados, mientras que reservamos el término “composición” propiamente dicho para la composición fuerte, donde hay verdadera pertenencia y dependencia entre objetos.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Cuando una clase usa a otra solo dentro de un método —por ejemplo, recibiendo un objeto como parámetro, devolviéndolo, creándolo con new dentro del propio método o empleándolo como variable local— no estamos hablando de composición, sino de dependencia. En estos casos, la clase no “posee” al otro objeto ni forma parte de su estructura interna; simplemente lo utiliza de manera puntual para realizar alguna operación. Es una relación muy débil y temporal, que solo existe mientras dura la ejecución del método.

### La composición, en cambio, implica que un objeto contiene a otro como parte de su estado permanente, normalmente a través de atributos. Por ejemplo, en el ejercicio anterior, una Linea tiene dos Punto que forman parte esencial de ella y existen mientras exista la línea. Eso es composición, porque los puntos son elementos internos del objeto. Pero si un método “usa” un Punto momentáneamente, eso no significa que haya composición.

### En términos de diseños de clases, la dependencia expresa que una clase necesita a otra para hacer alguna tarea específica, pero no controla su vida ni la guarda como parte de su identidad. En Java, esto es lo más común cuando las relaciones son muy ligeras, como pasar objetos a funciones o devolverlos como resultado.

### Por tanto, todas las situaciones mencionadas en el enunciado —parámetros, valores de retorno, objetos creados dentro de métodos o variables locales— describen dependencia, no composición. La composición solo aparece cuando la relación forma parte del estado permanente del objeto.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### En el caso de la línea y los puntos, podemos programar la relación de dos formas dependiendo de si queremos que los puntos dependan totalmente de la línea o no. Si implementamos composición fuerte, significa que los puntos son creados dentro de la propia clase Linea y no existen fuera de ella. En otras palabras, su ciclo de vida queda “atado” al de la línea: cuando la línea desaparece, los puntos también. Esto se suele hacer usando atributos final y no aceptando puntos externos en el constructor, sino construyéndolos dentro de la línea.

### Por otro lado, la composición débil (o agregación) permite que una línea simplemente “use” puntos que ya existen fuera. En este caso, los puntos podrían vivir más tiempo que la línea o incluso ser compartidos por otras líneas. Esto implica que la línea no es propietaria de los puntos, solo los referencia. Esta es una relación más flexible y depende de cómo queramos modelar el problema.

### La diferencia clave es el ciclo de vida: en la composición fuerte, las partes internas nacen y mueren con el objeto; mientras que en la composición débil, las partes pueden existir por separado. Ambos enfoques son útiles dependiendo de si queremos garantizar la propiedad exclusiva de los datos (fuerte) o si queremos reutilizar objetos existentes (débil).



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### En Java, aunque hablemos de composición fuerte, el programador no destruye los objetos manualmente. Esto se debe a que Java tiene recolección automática de basura (garbage collector). Esto significa que los objetos se destruyen cuando ya no existe ninguna referencia hacia ellos, y no cuando una clase “decide” destruirlos. Por eso, aunque Linea contenga dos Punto, nunca veremos una instrucción donde la línea “borre” explícitamente esos puntos.

### La razón es que, al eliminarse la instancia de Linea (por ejemplo, cuando sale de ámbito o deja de tener referencias), automáticamente deja de haber referencias a sus puntos internos. Como esos Punto ya no están accesibles desde ninguna parte, el garbage collector los marca como objetos para eliminar. Así, en composición fuerte, la dependencia en el ciclo de vida se cumple sin necesidad de código específico para destruir.

### En otras palabras, la composición fuerte en Java no se implementa destruyendo objetos desde dentro del contenedor, sino garantizando que los objetos compuestos no tengan vida propia fuera del contenedor. Cuando el contenedor desaparece, los objetos internos quedan inalcanzables y el garbage collector se encarga del resto.

### Por eso, aunque no veamos un “destroy” en el código, la composición fuerte sigue siendo real: los puntos solo viven mientras la línea los mantenga referenciados.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### class Profesor {
###    private final String nombre;

###    public Profesor(String nombre) {
###        this.nombre = nombre;
###    }

###    public String getNombre() {
###        return nombre;
###    }
### }

### class Departamento {
###    private final Profesor[] profesores = new Profesor[50];
###    private int numProfesores = 0;
###    private Profesor director;

###    public Departamento(Profesor directorInicial) {
###        if (directorInicial == null)
###            throw new IllegalArgumentException("El director no puede ser null");

###        // El director debe estar en la lista desde el inicio
###        profesores[0] = directorInicial;
###        numProfesores = 1;
###        director = directorInicial;
###    }

###    public void añadirProfesor(Profesor p) {
###        if (numProfesores == 50)
###            throw new IllegalStateException("No caben más profesores");

###        profesores[numProfesores++] = p;
###    }

###    public void eliminarProfesor(int posicion) {
###        if (posicion < 0 || posicion >= numProfesores)
###            throw new IndexOutOfBoundsException("Posición inválida");

###        Profesor aEliminar = profesores[posicion];

###        // No se permite eliminar al director
###        if (aEliminar == director)
###            throw new IllegalStateException("No se puede eliminar al director");

###        // Desplazar el array hacia la izquierda
###        for (int i = posicion; i < numProfesores - 1; i++) {
###            profesores[i] = profesores[i + 1];
###        }

###        numProfesores--;
###    }

###    public int getNumeroProfesores() {
###        return numProfesores;
###    }

###    public Profesor getProfesor(int posicion) {
###        if (posicion < 0 || posicion >= numProfesores)
###            throw new IndexOutOfBoundsException("Posición inválida");

###        return proesores[posicion];
###    }

###    public void cambiarDirector(Profesor nuevoDirector) {
###        // El nuevo director debe estar dentro del departamento
###        boolean encontrado = false;
###        for (int i = 0; i < numProfesores; i++) {
###            if (profesores[i] == nuevoDirector) {
###                encontrado = true;
###                break;
###            }
###        }

###        if (!encontrado)
###            throw new IllegalArgumentException("El director debe ser profesor del departamento");

###        director = nuevoDirector;
###    }

###    public Profesor getDirector() {
###        return director;
###    }
### }


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Reescribir el ejercicio usando List<Profesor> simplifica bastante el código, porque ya no necesitamos preocuparnos de manejar manualmente el tamaño máximo del array ni de desplazar elementos al eliminar un profesor. Con una lista, añadir y borrar son operaciones mucho más sencillas y no hace falta llevar un contador separado como numProfesores. Esto elimina bastante código repetitivo y reduce errores que normalmente ocurren al manipular arrays manualmente.

### En cuanto a devolver la lista interna directamente, el problema es que eso rompería la encapsulación. Si alguien recibiera la lista real del departamento, podría modificarla desde fuera sin que la clase Departamento lo controle: podría borrar profesores, añadir otros o incluso eliminar al director sin respetar las invariantes. Devolver la estructura interna permite que cualquier código externo manipule el estado del objeto por completo, lo cual es peligroso y contrario a un buen diseño.

### La forma correcta de resolverlo es devolver una copia de la lista (por ejemplo, usando new ArrayList<>(listaInterna)) o devolver una vista inmodificable usando Collections.unmodifiableList(...). De esta manera, el código externo puede consultar la información, pero no puede romper las reglas internas del departamento.


### import java.util.ArrayList;
### import java.util.Collections;
### import java.util.List;

### class Profesor {
###    private final String nombre;

###    public Profesor(String nombre) {
###        this.nombre = nombre;
###    }

###    public String getNombre() {
###        return nombre;
###    }
### }

### class Departamento {
###    private final List<Profesor> profesores = new ArrayList<>();
###    private Profesor director;

###    public Departamento(Profesor directorInicial) {
###        if (directorInicial == null)
###            throw new IllegalArgumentException("El director no puede ser null");

###        profesores.add(directorInicial);
###        director = directorInicial;
###    }

###    public void añadirProfesor(Profesor p) {
###        if (profesores.size() == 50)
###            throw new IllegalStateException("No caben más profesores");

###        profesores.add(p);
###    }

###    public void eliminarProfesor(int pos) {
###        if (pos < 0 || pos >= profesores.size())
###            throw new IndexOutOfBoundsException("Posición inválida");

###        Profesor aEliminar = profesores.get(pos);

###        if (aEliminar == director)
###            throw new IllegalStateException("No se puede eliminar al director");

###        profesores.remove(pos);
###    }

###    public int getNumeroProfesores() {
###        return profesores.size();
###    }

###    public Profesor getProfesor(int pos) {
###        return profesores.get(pos);
###    }

###    public void cambiarDirector(Profesor nuevoDirector) {
###        if (!profesores.contains(nuevoDirector))
###            throw new IllegalArgumentException("El director debe ser profesor del departamento");

###        director = nuevoDirector;
###    }

###    public Profesor getDirector() {
###        return director;
###    }

###    // Método pedido en el enunciado: devolver todos los profesores sin romper encapsulación
###    public List<Profesor> getProfesores() {
###        return Collections.unmodifiableList(profesores);
###    }
### }


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Una composición recursiva ocurre cuando una clase contiene dentro de sí misma otra instancia de la misma clase. Esto pasa, por ejemplo, con las excepciones en Java, donde una excepción puede tener otra excepción como causa. Ese tipo de estructura permite representar relaciones jerárquicas o encadenadas sin necesidad de crear nuevas clases. En este ejercicio, el caso es una Persona que tiene una madre, que también es una Persona, y así sucesivamente. Como la clase representa a una persona real, lo lógico es que sea inmutable, es decir, que una vez creada no cambie su madre, su nombre, ni ningún otro dato.

### Esta composición es recursiva porque la madre también es una Persona y, por lo tanto, puede tener su propia madre, y así podemos construir una cadena que represente a varias generaciones. En el main podemos crear una familia desde la abuela hasta el nieto, siguiendo esa estructura recursiva. Gracias a la inmutabilidad, evitamos inconsistencias, como cambiar de madre a una persona después de haberla creado.

### Otros ejemplos clásicos de composiciones recursivas aparecen en estructuras de datos. Por ejemplo, los árboles (como los árboles binarios), donde cada nodo contiene otros nodos hijos, o las listas enlazadas, donde cada nodo contiene una referencia al nodo siguiente. Incluso los directorios del sistema de archivos son un ejemplo: un directorio contiene otros directorios, formando una estructura recursiva.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Las relaciones de composición bidireccionales son aquellas en las que dos clases están unidas por una relación de “todo–parte”, pero además ambas clases pueden acceder la una a la otra. Es decir, no solo el objeto “todo” contiene a los objetos “parte”, sino que cada “parte” también guarda una referencia de vuelta al “todo”. Aunque la composición implica que la vida de las partes depende del todo, la bidireccionalidad simplemente añade que ambas se pueden consultar mutuamente.

### En el ejemplo de Profesor y Departamento, esto significaría que un Departamento tendría una colección de Profesor (como suele ser normal), pero además cada Profesor tendría un atributo que señale qué Departamento lo contiene. De esa manera, desde el departamento podemos obtener sus profesores, y desde cada profesor podemos saber en qué departamento trabaja.

### Para implementarlo, habría que modificar ambas clases. En Departamento, se mantendría la lista o vector de objetos Profesor. En Profesor, se añadiría un atributo del tipo Departamento* (o equivalente según el lenguaje) que se establezca cuando ese profesor sea añadido al departamento. Finalmente, cada vez que se agregue o elimine un profesor del departamento, también habría que actualizar esta referencia dentro del propio objeto Profesor para mantener la coherencia entre los dos lados de la relación.
