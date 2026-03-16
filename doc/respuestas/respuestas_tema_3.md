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

### En C no existen las excepciones, así que normalmente los errores se comunican desde una función mediante códigos de retorno o modificando una variable externa (como una variable int* o una global). La primera opción es que la función raiz devuelva un valor especial cuando hay error, por ejemplo -1.0, y que el usuario compruebe ese valor fuera. Esto funciona pero tiene el problema de que si el valor especial puede confundirse con un resultado real, hay que documentarlo muy bien.

### include <math.h>

### double raiz(double x) {
###    if (x < 0)
###        return -1.0;  // valor especial de error
###    return sqrt(x);
### }

### double r = raiz(-4);
### if (r == -1.0) {
###    printf("Error: número negativo\n");
### }

### Otra opción más “limpia” es que la función devuelva un código de error y entregue el resultado por un parámetro de salida. De esta forma el usuario sabe exactamente si hubo error sin depender de valores especiales. El resultado solo se utiliza si la función devolvió éxito (por convención, 0 suele significar que no hubo problemas).

### #include <math.h>

### int raiz(double x, double *resultado) {
###    if (x < 0)
###        return 1;  // código de error
###    *resultado = sqrt(x);
###    return 0;      // OK
### }

### double r;
### if (raiz(-4, &r) != 0) {
###    printf("Error: número negativo\n");
### }



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Una **excepción** es un **mecanismo** que tienen muchos lenguajes de programación (como Java, C++ o Python) para indicar que ha ocurrido un error o una situación inesperada durante la ejecución de un programa. En vez de devolver un código de error como en C, la función “lanza” una señal especial que interrumpe el flujo normal del programa y transfiere el control a un manejador de errores. Es como si el programa dijera: “ha pasado algo que no puedo resolver aquí, que lo gestione otro”.

### El objetivo principal de usar **excepciones** al implementar funciones es **simplificar el código y separar la lógica normal de la lógica de errores**. Esto evita llenar las funciones de comprobaciones constantes y hace que el código sea más legible. Por ejemplo, en vez de devolver -1 cuando algo falla, una función puede lanzar una excepción InvalidArgumentException.

### Cuando un programador llama a una función que puede lanzar excepciones, su objetivo es **decidir cómo reaccionar**: capturar la excepción y resolver el problema, o simplemente dejar que se propague a niveles superiores. Esto permite **controlar los errores de forma estructurada**. Por ejemplo, un try–catch en Java puede atrapar un error sin que el programa entero falle.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### En Java es más natural usar excepciones, así que metemos el método raiz dentro de una clase Calculadora y luego lo llamamos desde main, controlando el error con un bloque try–catch. De esta forma se ve claramente cómo el error se detecta dentro de la función pero se gestiona desde fuera.

### La clase Calculadora tendrá un método raiz(double x) que lanzará una excepción si el número es negativo. Luego, en main, simplemente instanciamos la calculadora, llamamos a raiz y usamos un catch para reaccionar al error. Así se ilustra la idea de separar la lógica normal del manejo de errores.

### class Calculadora {

###    public double raiz(double x) throws Exception {
###        if (x < 0) {
###            throw new Exception("Número negativo: no se puede calcular la raíz");
###        }
###        return Math.sqrt(x);
###    }
### }

### public class Main {
###    public static void main(String[] args) {
###        Calculadora calc = new Calculadora();

###        try {
###            double r = calc.raiz(-4);
###            System.out.println("Resultado: " + r);
###        } catch (Exception e) {
###            System.out.println("Error recibido desde raiz(): " + e.getMessage());
###        }
###    }
### }

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

###  “Lanzar” una excepción significa que una **función detecta un error y decide interrumpir su ejecución normal, enviando una señal especial (throw) hacia el exterior**. En cambio, “capturar” o “controlar” una excepción significa que otra parte del programa, normalmente quien llama a la función, la recibe dentro de un bloque try–catch y decide qué hacer con ese error, evitando que el programa termine abruptamente.

### Cuando decimos que una **excepción se propaga**, nos referimos a que **va subiendo por la pila de llamadas**. Es decir, si la función donde ocurrió el error no la captura, la excepción vuelve a la función que la llamó; si esa tampoco la captura, sigue subiendo, y así sucesivamente. Las funciones por las que pasa la excepción no se reanudan, sino que su ejecución se interrumpe inmediatamente y no continúan después del punto donde ocurrió el error.

### Con el ejemplo de la raíz cuadrada en Java, **si raiz() lanza una excepción**, esa función **se detiene justo en el throw y no ejecuta nada más**. Si main() sí tiene un bloque try–catch, entonces la excepción llega hasta él y el programa entra en el catch. Si main() tampoco la capturara, el programa terminaría con un error.

### class Calculadora {
###    public double raiz(double x) throws Exception {
###        if (x < 0) {
###            throw new Exception("Número negativo"); // Se lanza y se detiene aquí
###        }
###        return Math.sqrt(x);
###    }
### }

### public class Main {
###    public static void main(String[] args) {
###        Calculadora c = new Calculadora();

###        try {
###            double r = c.raiz(-9);  // La excepción se propaga hasta aquí
###            System.out.println(r);  // Esto NO se ejecuta si hay excepción
###        } catch (Exception e) {
###            System.out.println("Capturada en main: " + e.getMessage());
###        }
###    }
### }


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Una ventaja muy clara de la **propagación natural** de excepciones frente al sistema habitual de C (códigos de error) es que **el programador no tiene que ir comprobando manualmente el resultado de cada función**. En lenguajes como Java, si ocurre un error, la excepción sube automáticamente por la pila de llamadas hasta que alguien la capture. Esto hace que el código sea más limpio y evita que cada función tenga que decidir qué hacer con el error, cuando muchas veces esa función no puede solucionarlo.

### Otra ventaja importante es que las excepciones **permiten separar la lógica normal del programa de la lógica destinada al manejo de errores**. En C, cada función tiene que incluir comprobaciones del tipo if (error)..., mientras que en Java la función simplemente lanza una excepción y un nivel superior se encarga de manejar el problema. Esto hace que el flujo del programa sea más fácil de entender para un principiante, porque no está “salpicado” de controles de error.

### Además, con la propagación natural, las funciones que no quieran o no puedan manejar un error no tienen que hacer nada especial: simplemente dejan que la excepción suba un nivel más. En C, en cambio, si una función no controla el error, igualmente debe propagarlo manualmente devolviendo un código especial, lo que puede llevar a errores si el programador se olvida de comprobar o reenviar ese código. Las excepciones reducen ese riesgo y hacen que la gestión quede centralizada en un punto adecuado del programa.

### Para verlo conectado con el ejemplo anterior, si raiz() lanza una excepción, no hace falta que todas las funciones intermedias la vayan pasando hacia arriba: Java ya se encarga. De esta forma, solo main() (o cualquier otro nivel más adecuado) necesita un try–catch, mientras que el resto del código puede mantenerse más limpio y centrado en su funcionalidad principal.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### En **orientación a objetos**, las **excepciones suelen ser objetos** porque así pueden encapsular información útil sobre el error, igual que cualquier otro objeto del programa. Esto permite guardar dentro de la excepción datos como un mensaje, un código de error, la causa original e incluso métodos que ayuden a diagnosticar el problema. Para un programador, esto es muy cómodo porque no solo recibe un “ha fallado”, sino un objeto completo con toda la información relevante.

### La **ventaja en términos de encapsulación es que cada tipo de error puede definirse como una clase con su propio comportamiento**. Por ejemplo, una excepción de “archivo no encontrado” puede contener la ruta del archivo, mientras que una excepción de “número negativo” puede almacenar el valor que causó el error. Esto permite organizar mejor los errores y tratarlos de forma más específica sin mezclar lógica de control con datos dispersos.

### Y sí, gracias a que las excepciones son objetos, **podemos crear excepciones personalizadas simplemente extendiendo la clase Exception** (o RuntimeException) en Java. Esto es muy útil cuando queremos representar errores específicos de nuestro programa o de un módulo concreto.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Cuando trabajamos con excepciones en lenguajes como Java, cada objeto excepción transporta una información mínima que resulta súper útil cuando el programa llega al manejador (catch). Lo más importante es normalmente el mensaje descriptivo del error, que explica qué salió mal. Esto es mucho más cómodo que en C, donde muchas veces solo tienes un código numérico o te toca gestionar tú mismo las estructuras de error.

### Además del mensaje, el objeto excepción también suele llevar la traza de la pila (stack trace). Esta traza indica exactamente por qué funciones fue pasando el programa hasta llegar al punto donde se lanzó la excepción. Para un programador novel (como yo en primero), esto es oro puro porque te ayuda a localizar el fallo sin tener que poner mil printf o usar el depurador a ciegas.

### Finalmente, otra cosa útil es que la excepción tiene un tipo concreto (por ejemplo, IOException, NullPointerException, etc.), y ese tipo ya encapsula más contexto sobre el tipo de problema. Gracias a eso puedes escribir manejadores específicos para cada categoría de error, algo que en C tendrías que montar manualmente y con más riesgo de equivocarte.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### En Java sí se pueden tener varios bloques catch después de un mismo try. Esto se hace porque diferentes tipos de excepciones pueden requerir tratamientos distintos, y Java permite capturar cada tipo por separado. De hecho, es bastante común ordenarlos de más específico a más general, para que el manejador adecuado capture la excepción correcta.

### Sin embargo, aunque haya muchos bloques catch, solo se ejecuta uno. El que se ejecuta es el primero cuyo tipo coincide con el tipo real de la excepción lanzada. Una vez que ese bloque catch se ejecuta, los demás se ignoran completamente. Es como si Java hiciera una comparación de arriba abajo, y en cuanto encuentra un catch compatible, detiene la búsqueda y entra ahí.

### Esto tiene sentido porque la idea de las excepciones es que representan un único error en un punto concreto, no varios a la vez. Por eso, aunque puedas preparar distintos manejadores, realmente solo uno debe encargarse de responder a ese error específico.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Cuando una excepción rompe el flujo normal del programa, Java permite garantizar que cierto código siempre se ejecute usando el bloque finally. Este bloque se ejecuta pase lo que pase: haya excepción, no la haya, se capture o incluso si dentro del try hay un return. Por eso se usa mucho para cerrar ficheros, liberar memoria o desconectar recursos, ya que esas operaciones no deberían depender de que el programa vaya “bien”.

### Lo bueno del finally es que funciona tanto si tenemos un catch como si no. Si hay catch, primero se gestiona la excepción en ese bloque y después se ejecuta finally. Si no hay catch, la excepción se sigue propagando hacia arriba, pero antes de eso Java ejecuta igualmente el finally, garantizando que no dejemos recursos abiertos.

### try {
###    int x = 5 / 0;   // Provoca ArithmeticException
### } catch (ArithmeticException e) {
###    System.out.println("Se capturó la excepción.");
### } finally {
###    System.out.println("Este mensaje SIEMPRE se ejecuta.");
### }

### try {
###    int[] v = new int[2];
###    v[5] = 10;   // Provoca ArrayIndexOutOfBoundsException
### } finally {
###    System.out.println("Aunque la excepción no se capture, esto se ejecuta.");
### }



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### En Java, el bloque finally sí puede ir sin catch, siempre que haya un try antes. Es decir, puedes escribir try { ... } finally { ... } sin ningún problema. La idea es que finally representa código que siempre debe ejecutarse, y Java garantiza eso tanto si ocurre una excepción como si no. Por eso se usa para cerrar ficheros, liberar memoria o desconectar recursos de forma segura.

### Este bloque también se ejecuta si dentro del try no hay ningún error. O sea, aunque el programa vaya perfecto, Java llega al finally antes de seguir con la ejecución normal. Y lo más interesante es que incluso si dentro del try hay un return, el finally se ejecuta antes de que el método termine. Es como una especie de “última palabra” del bloque: pase lo que pase, lo que está ahí dentro siempre se ejecuta.

### La única excepción rara (que no suele verse en primero) es si el propio programa se “mata a sí mismo” abruptamente, como con System.exit(), pero para un uso normal el finally es totalmente fiable. En resumen: sí puede ir solo, sí se ejecuta siempre, y sí se ejecuta incluso con un return.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### En Java, las excepciones controladas (checked) son aquellas que el compilador obliga a manejar explícitamente, ya sea con un try-catch o declarándolas con throws. Suelen representar situaciones que se pueden prever, como errores de entrada/salida o problemas al abrir un fichero. En cambio, las excepciones no controladas (unchecked) son las que heredan de RuntimeException, y el compilador no te obliga a capturarlas. Estas representan fallos de programación, como dividir entre cero o acceder a un índice fuera de rango.

### RuntimeException juega un papel central porque todas las excepciones no controladas derivan de ella. Esto significa que, cuando lanzas una excepción que extiende RuntimeException, estás diciendo que es un error lógico del programa que no tiene por qué ser tratado en cada punto del código. En cambio, las excepciones controladas heredan de Exception (pero no de RuntimeException) y se usan cuando el programador debe hacerse cargo del posible fallo sí o sí.

### Ejemplos típicos
### Excepciones controladas (checked):

### IOException
### FileNotFoundException
### SQLException
### ClassNotFoundException

### Excepciones no controladas (unchecked):

### NullPointerException
### ArithmeticException
### ArrayIndexOutOfBoundsException
### IllegalArgumentException

### Situaciones donde usar una excepción controlada:

### Cuando un fichero puede no existir y debemos decidir qué hacer.
### Cuando un servidor puede no responder y queremos manejar el fallo.
### Cuando un método depende de un recurso externo inseguro (como red o disco).
### Cuando queremos obligar a quien use nuestro método a gestionar el error.

### Situaciones donde usar una excepción no controlada:

### Si se pasa un parámetro inválido a un método (IllegalArgumentException).
### Si se produce un error de programación evidente, como índice fuera de rango.
### Si queremos indicar un bug interno y no que el usuario “controle” el fallo.
### Si el error es irrecuperable y no tiene sentido forzar un catch.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### En Java, la palabra clave throws se utiliza en la cabecera de un método para indicar que dicho método puede lanzar una excepción controlada y que no la va a gestionar él mismo. Es decir, es como decirle al compilador: “este método puede fallar de esta forma, pero que lo gestione quien lo llame”. Por eso aparece justo después del nombre del método, para que cualquier programador que lo use esté obligado a tenerlo en cuenta.

### La razón por la que throws es una alternativa a capturar una excepción controlada es que, en Java, el compilador obliga a manejar este tipo de excepciones de dos maneras posibles: o las capturas con un try-catch, o las declaras con throws. Si declaras throws, no estás solucionando la excepción, pero sí cumples las reglas del lenguaje y pasas la responsabilidad hacia arriba, a otro método que quizás tenga más contexto para gestionarla.

### Este mecanismo es especialmente útil cuando estás escribiendo métodos de bajo nivel o muy pequeños, donde no tiene sentido poner un try-catch porque el método que llama es quien debería decidir qué hacer con el error. Por ejemplo, si un método simplemente abre un fichero, seguramente quien lo llame sea quien sepa si debe mostrar un mensaje, cerrar el programa o intentar otra ruta alternativa.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Cuando queremos que un método abra un fichero pero no queremos gestionar ahí mismo la excepción de que el fichero no exista, lo normal en Java es usar throws en la firma del método. De esta forma, el método indica que puede lanzar una IOException, pero deja que sea el método llamador quien decida cómo manejarla. Aun así, aunque no capturemos la excepción, sí podemos usar un bloque finally para asegurarnos de cerrar el fichero si llega a abrirse.

### En este tipo de funciones, el finally es importante porque se ejecuta tanto si todo va bien como si el fichero no existe o incluso si ocurre cualquier otra excepción. Así evitamos pérdidas de recursos. La idea es que el método haga el mínimo imprescindible (abrir el fichero, usarlo un poco) y luego delegue la gestión del error a niveles superiores.

### import java.io.*;

### public static void abrirFichero(String ruta) throws IOException {
###    FileReader fr = null;
    
###    try {
###        fr = new FileReader(ruta);   // Puede lanzar FileNotFoundException (checked)
###        System.out.println("Fichero abierto correctamente.");
                
###    } finally {
###        // Este código se ejecuta ocurra o no la excepción
###        if (fr != null) {
###            fr.close();  // También puede lanzar IOException
###            System.out.println("Fichero cerrado en finally.");
###        }
###    }
### }


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### En Java sí podemos poner excepciones no controladas (como RuntimeException) en la cláusula throws, pero en realidad no es obligatorio. Las excepciones que heredan de RuntimeException son unchecked, y por eso el compilador no exige ni declararlas con throws ni capturarlas con try-catch. Si las ponemos en throws, es más bien como dejar un aviso al resto de programadores indicando que ese método podría lanzar ese tipo de error, pero no cambia nada en el comportamiento del programa.

### El método llamador no está obligado a poner un try-catch en ese caso. Como son excepciones no controladas, puede ignorarlas totalmente, y si ocurren, simplemente se propagan hacia arriba hasta que, si nadie las captura, el programa termina con un error. Por eso normalmente no se usan try-catch para RuntimeException, ya que suelen indicar errores de programación, como pasar un argumento inválido o acceder fuera de los límites de un array.

### El sentido de declarar una RuntimeException en throws sería más documental que práctico: sirve para informar de que un método puede lanzar una excepción concreta, aunque no sea obligatorio gestionarla. En la práctica, la mayoría de programadores no lo hace porque las excepciones no controladas se entienden como errores lógicos que deben corregirse, no “manejarse”.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### En general, en Java se recomienda usar excepciones controladas cuando el error forma parte del funcionamiento normal del programa y es algo que se espera que pueda pasar. Por ejemplo, una IOException es típica cuando abrimos un fichero o trabajamos con red, porque esos fallos dependen del entorno y el programa debe decidir qué hacer al respecto. En estos casos, obligar al programador a gestionar la excepción tiene sentido, ya que hay varias formas de reaccionar y no todas son errores del código.

### Por otro lado, las excepciones no controladas se usan cuando el error indica un problema de programación, como un parámetro inválido (IllegalArgumentException) o un acceso fuera de rango. En estos casos no se suele pedir al programador que ponga try-catch porque la solución real no es “manejar” la excepción, sino corregir el código. Esto hace que las excepciones no controladas sean mucho más ligeras de usar, porque no obligan a llenar todo de manejadores artificiales.

### Respecto a otros lenguajes, no todos ofrecen ambos tipos. De hecho, Java es uno de los pocos lenguajes que distingue entre controladas y no controladas. Lenguajes muy populares como Python, C#, JavaScript, C++ o Go únicamente tienen excepciones “no controladas”, y es el programador quien decide si las gestiona o no. En esos lenguajes, lo habitual es usar excepciones similares a las RuntimeException de Java, es decir, unchecked.

### En los lenguajes donde solo existe una opción, la más habitual es precisamente la equivalente a las no controladas, porque simplifica el diseño del lenguaje y evita obligar a poner try-catch en montones de sitios donde realmente no aporta nada. Por eso, aunque las excepciones controladas tienen ventajas en ciertos contextos, hoy en día la mayoría de lenguajes modernos prefieren el modelo de excepciones no obligatorias.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Sí tiene sentido lanzar excepciones dentro de un catch, y de hecho es algo bastante habitual cuando queremos transformar una excepción en otra más significativa o cuando queremos añadir información extra. Por ejemplo, si capturamos una IOException pero en nuestro contexto lo que realmente queremos indicar es un error de lectura de datos, podemos lanzar desde el catch una excepción diferente y más específica. Esta técnica se llama exception wrapping o “encapsular excepciones”.

### También es totalmente posible relanzar la misma excepción que se ha capturado, simplemente usando throw e;. Esto tiene sentido cuando queremos realizar alguna acción intermedia (como registrar un error en un log, cerrar recursos o hacer una comprobación) pero no queremos ocultar la excepción original. De esta forma, el error sigue propagándose hacia arriba, pero no perdemos la información original del fallo.

### try {
###    leerArchivo("datos.txt");
### } catch (IOException e) {
###    throw new RuntimeException("Error leyendo los datos", e);
### }


### try {
###    int x = 5 / 0;
### } catch (ArithmeticException e) {
###    System.out.println("Ocurrió un error, lo registro y lo relanzo...");
###     throw e;   // Misma excepción propagada hacia arriba
### }

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Cuando decimos que una excepción es la “causa” de otra, nos referimos a que estamos encapsulando una excepción de bajo nivel dentro de una de más alto nivel. Es decir, el error original no se pierde, sino que se adjunta como explicación interna de por qué ocurrió la excepción nueva. Esto es muy útil cuando queremos dar contexto más abstracto: por ejemplo, un método de lectura de datos puede recibir una IOException, pero para el nivel superior quizá sea mejor lanzar una MiExcepcionDeDatos que explique el problema de forma más clara.

### En Java esto se hace pasando la excepción original como parámetro al constructor de la nueva excepción. Así, la excepción de alto nivel almacena la otra como su causa interna. Y sí: cuando la excepción se imprime por pantalla, Java muestra toda la cadena de causas, normalmente con un texto como “Caused by:”, lo cual facilita mucho la depuración.

### class ExcepcionDeAltoNivel extends Exception {
###    public ExcepcionDeAltoNivel(String msg, Throwable causa) {
###        super(msg, causa);
###    }
### }

### public static void procesar() throws ExcepcionDeAltoNivel {
###    try {
###        int x = Integer.parseInt("hola");  // Provoca NumberFormatException
###    } catch (NumberFormatException e) {
###        throw new ExcepcionDeAltoNivel("Error procesando los datos", e);
###    }
### }

