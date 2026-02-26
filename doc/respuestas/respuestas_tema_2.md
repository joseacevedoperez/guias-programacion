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

### La _**encapsulación**_ busca agrupar datos y métodos dentro de una misma entidad, normalmente una clase, de forma que podamos manejar objetos como unidades lógicas y coherentes. Esto ayuda a organizar el código y a que cada parte del programa tenga responsabilidades bien definidas. La _**ocultación**_ de información complementa este concepto restringiendo el acceso directo a los atributos internos, lo que evita que otras partes del programa puedan modificarlos de forma incorrecta.

`Encapsulación, tiene que ver con "Protección".`

`{- Evito estados no válidos de mis objetos}`

`{- Evito dependecias desde fuera que no quiero}`

`He juntado estado y comportamiento en un artefacto(la clase), y ahora puedo ocultar(private) ciertas partes del exterior.`

### Gracias a la _ocultación de información_ ganamos seguridad y estabilidad, porque evitamos que un objeto pueda quedar en un estado inválido. Además, facilita el mantenimiento: si en el futuro queremos cambiar cómo funciona algo internamente, lo podemos hacer sin afectar al resto del código, siempre que mantengamos la misma interfaz pública.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### La _**interfaz pública**_ de una clase es el conjunto de métodos (y a veces constantes) que otras partes del programa pueden utilizar para interactuar con sus objetos. Es básicamente “lo que la clase ofrece al exterior”, y debería ser lo más claro y estable posible para que los usuarios de la clase sepan exactamente cómo usarla.

`Interfaz pública-> Los miembros que se ven desde fuera, es decir, los que no están ocultos.`

### La relación con la _**ocultación de información**_ es directa: la interfaz pública es lo que dejamos visible, mientras que los detalles internos se mantienen privados. Así, el usuario de la clase no necesita saber cómo se implementan las cosas por dentro; solo necesita conocer los métodos públicos para interactuar con ella. Esto reduce errores y permite modificar la implementación interna sin romper el código que depende de la clase.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Diseñar bien la _**interfaz**_ pública es importante porque, una vez publicada, otros módulos o desarrolladores empezarán a depender de ella. Si después queremos modificarla, podría ser complicado o incluso imposible sin romper funcionalidades. Por eso se dice que una buena interfaz debe ser estable, sencilla y coherente con el propósito de la clase.

`La interfaz pública si se cambia tiene más consecuencias que cualquier cambio en la parte oculta.`

### Además, si la interfaz está mal pensada, la clase puede volverse difícil de usar o demasiado dependiente de cómo está implementada internamente. Esto va en contra de la idea de _**encapsulación**_. En cambio, cuando la interfaz está bien diseñada, trabajar con esa clase resulta más intuitivo y los cambios futuros se pueden hacer con mayor libertad.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Las _**invariantes de clase**_ son condiciones que siempre deben cumplirse para que un objeto esté en un estado válido. Por ejemplo, en una clase CuentaBancaria, podríamos exigir que el saldo nunca sea negativo. Estas reglas forman parte de la lógica fundamental de la clase y deben mantenerse antes y después de ejecutar cualquier método.

`Invariantes de clase-> Condiciones que los objetos de esa clase deben cumplir para ser válidos y durante toda la vida del objeto.`

`Ej:`

`{- CuentaBancaria debe tener saldo positivo}`

`{- Persona debe tener edad => 0}`


### La _**ocultación de información**_ ayuda porque evita que código externo modifique directamente los atributos y rompa estas invariantes. Al obligar a que todas las modificaciones se hagan a través de métodos controlados (setters, validaciones, etc.), garantizamos que los objetos nunca entren en estados incoherentes. Es una forma de proteger la integridad del programa.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### public class Punto {
###    private double x;
###    private double y;

###    public Punto(double x, double y) {
###        this.x = x;
###        this.y = y;
###    }

###    public double getX() { 
###        return x; 
###    }

###    public double getY() { 
###        return y; 
###    }

###    public void setX(double x) { 
###        this.x = x; 
###    }

###    public void setY(double y) { 
###        this.y = y; 
###    }

###    public double calcularDistanciaAOrigen() {
###        return Math.sqrt(x * x + y * y);
###    }

###    @Override
###    public String toString() {
###        return "Punto(" + x + ", " + y + ")";
###    }

###    public static void main(String[] args) {
###        Punto p = new Punto(3.0, 4.0);
###        System.out.println(p); // Punto(3.0, 4.0)
###        System.out.println("Distancia al origen: " + p.          calcularDistanciaAOrigen()); // 5.0
###    }
### }


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### En Java, _**public**_ y _**private**_ se pueden aplicar tanto a clases (aunque solo a clases “top-level” en el caso de public) como a atributos, métodos y constructores. Esto permite controlar qué partes del código son visibles desde fuera y cuáles están reservadas para el uso interno de la clase.

`En Java:`

`Pública: clases, atributos, métodos.`

`-> Privada: clases internas(no las vemos), atributos y métodos.`

### Usarlos correctamente nos ayuda a organizar mejor el código y a evitar dependencias indeseadas entre módulos. También ayuda a los demás programadores a entender qué está pensado para ser usado desde fuera y qué forma parte de la implementación interna.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Sí, además de _**public**_ y _**private**_, existen otros niveles de visibilidad. En Java, por ejemplo, están _**protected**_ y el nivel **“default”** (cuando no se especifica nada). Protected permite el acceso desde la clase, sus subclases y clases del mismo paquete. El nivel por defecto permite el acceso desde cualquier clase del mismo paquete, pero no desde fuera.

`En Java:`

`Protected: solo se ve desde "subclases"(las vemos en el tema de herencia).`
`Package-private o sin modificador, solo se ve desde el paquete.`

### En otros lenguajes también existen variaciones. Por ejemplo, C++ tiene _**public, protected y private**_, pero su funcionamiento está más orientado a jerarquías de herencia que a _paquetes_. Python, en cambio, no tiene visibilidad estricta, pero usa convenciones como el guion bajo _atributo para indicar que algo debería tratarse como privado. Cada lenguaje adapta estos conceptos a su propio modelo de acceso.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Los miembros de instancia private están ocultos a cualquier otra clase, pero no entre instancias cuando el acceso ocurre dentro del código de la misma clase. En Java, la restricción de private es por clase, no por objeto: desde un método de Punto puedes leer los campos privados de this y también los de otra instancia Punto pasada como parámetro, pero una clase externa no puede acceder directamente a esos campos. Por eso, para calcular la distancia entre dos puntos, el método de instancia puede usar otro.x y otro.y aunque sean privados.

### public class Punto {
###    private double x;
###    private double y;

###    public Punto(double x, double y) {
###        this.x = x;
###        this.y = y;
###    }

###    public double calcularDistanciaAPunto(Punto otro) {
###        double dx = this.x - otro.x;
###        double dy = this.y - otro.y;
###        return Math.sqrt(dx * dx + dy * dy);
###    }

###    public static void main(String[] args) {
###        Punto a = new Punto(1, 2);
###        Punto b = new Punto(4, 6);
###        System.out.println(a.calcularDistanciaAPunto (b)); // 5.0
###    }
### }


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Los métodos **getter** y **setter** son funciones públicas que permiten acceder y modificar, respectivamente, los atributos privados de un objeto. Existen porque en la programación orientada a objetos los atributos suelen declararse como private para protegerlos de accesos indebidos y mantener la encapsulación. Un getter devuelve el valor de un atributo sin exponer directamente la variable interna, mientras que un setter permite cambiar su valor aplicando validaciones o restricciones si es necesario.


### Estos métodos forman parte de la **interfaz pública** del objeto y ofrecen un punto de control entre el exterior y el interior de la clase. Gracias a ellos, el programador puede decidir cómo y cuándo se puede modificar un atributo, evitando inconsistencias y protegiendo la lógica interna del objeto. Además, facilitan el mantenimiento: si más adelante cambia la representación interna, los getters y setters pueden adaptarse sin afectar al código que usa la clase.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Cuando decimos que la ocultación de información mejora la “seguridad” del programa, no nos referimos a que el software sea imposible de hackear en el sentido de ciberseguridad. La palabra “seguridad” aquí se usa en un contexto puramente de diseño orientado a objetos, donde significa proteger los datos internos del objeto contra usos incorrectos por parte del propio programador o de otras partes del código. Es decir, evita errores lógicos, estados inválidos y dependencias no deseadas, pero no protege contra ataques externos reales.

### El objetivo de la encapsulación es obligar a que los atributos solo se manipulen mediante métodos controlados, permitiendo validar, filtrar o limitar lo que puede hacerse con ellos. Esto hace que el programa sea más robusto, más fácil de mantener y menos propenso a fallos causados por accesos directos e indiscriminados. Aun así, un sistema bien encapsulado puede seguir siendo vulnerable a ataques si no cuenta con medidas reales de ciberseguridad.

### Por tanto, la “seguridad” en este contexto es seguridad de diseño, no seguridad informática. La encapsulación evita que el propio desarrollador cometa errores que dejen a un objeto en un estado inconsistente, pero no impide que un atacante explote vulnerabilidades como inyecciones, desbordamientos o fallos de autenticación. En resumen, la ocultación de información mejora la calidad interna del código, no la protección ante ataques maliciosos.

`No, esto no es ciberseguridad, es facilitar una programación con menos bugs de programación.`

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Un `miembro de instancia` es un `atributo` o `método` que pertenece a cada objeto individual de una clase. Esto significa que cada instancia tiene su propia copia del dato, y su valor puede variar de un objeto a otro. Por el contrario, un miembro de clase es aquel declarado como static, lo que implica que existe una única copia compartida por todas las instancias. Los miembros de clase no dependen de un objeto concreto, sino de la propia clase como estructura común.

### En cuanto a la **ocultación**, los miembros de clase también pueden ocultarse usando modificadores de acceso como private, protected o public. La encapsulación se aplica por igual tanto a los elementos estáticos como a los de instancia; lo que cambia no es su accesibilidad, sino si pertenecen a cada objeto o a la clase en conjunto. Así, un atributo static private solo será accesible desde métodos de la propia clase, del mismo modo que un atributo private normal. La diferencia funcional está en el ámbito al que pertenece el dato, no en las reglas de visibilidad.

### Este mecanismo permite mantener un control estricto sobre la información compartida por todos los objetos. Cuando un miembro de clase está oculto, solo puede manipularse desde dentro de la clase, lo que previene usos indebidos y asegura que los cambios globales sigan las reglas establecidas por el programador. De esta forma, la encapsulación actúa de la misma manera tanto en datos particulares de cada objeto como en información común a toda la clase.

`Miembro de clase-> No asociado a ninguna estancia; es compartido por todas las instancias.`
`Miembro de instancia-> Está asociado a cada instancia, ya son compartidos.`

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Sí, tiene sentido que los **constructores** puedan ser **privados** en ciertos casos. Un constructor privado impide que se creen objetos libremente desde fuera de la clase, lo cual permite controlar estrictamente cómo y cuándo se construyen las instancias. Esta técnica se utiliza cuando una clase no debe permitir que otros módulos creen objetos directamente, ya sea por razones de diseño, eficiencia o para garantizar que solo exista un número limitado de instancias posibles.

### Este enfoque es habitual en patrones como Singleton, donde solo debe existir una única instancia de la clase, o en clases que ofrecen métodos estáticos de creación controlada, como las factory methods. En estos casos, el constructor privado ayuda a evitar usos incorrectos, obliga a seguir un proceso definido para crear objetos y mejora la coherencia interna del programa. Por tanto, aunque no siempre se use, es una herramienta útil en diseños específicos donde el programador necesita controlar la construcción de instancias.

`A veces:`
`-> Un constructor auxiliar oculto, llamado desde otros constructores públicos.`
`-> Cuando prefiero usar métodos factoria.`
`-> Cuando quiero controlar el número de instancias.`


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### En Java, los **miembros de clase** son aquellos que pertenecen a la clase en sí y no a cada instancia individual; se indican usando la palabra clave **static**. Esto significa que existe una única copia compartida por todos los objetos creados. En contraste, los miembros de instancia son propios de cada objeto y su valor puede diferir entre instancias. Los miembros de clase suelen usarse cuando se necesita llevar un registro global o información común a todos los objetos.

### En la clase **Punto**, pueden añadirse miembros de clase para registrar los valores máximos de x e y establecidos en cualquier punto creado hasta ahora. Cada vez que se construye un nuevo objeto o se modifican sus coordenadas, estos valores estáticos pueden actualizarse para reflejar si el nuevo dato supera al máximo previo. Así, es posible consultar en cualquier momento los valores máximos globales alcanzados sin necesidad de recorrer todos los objetos.

### public class Punto {
###    private double x;
###    private double y;

###    private static double maxX = Double.NEGATIVE_INFINITY;
###    private static double maxY = Double.NEGATIVE_INFINITY;

###    public Punto(double x, double y) {
###        this.x = x;
###        this.y = y;

###        if (x > maxX) maxX = x;
###        if (y > maxY) maxY = y;
###    }

###    public static double getMaxX() { return maxX; }
###    public static double getMaxY() { return maxY; }
### }


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Un **método factoría** es un método que devuelve una nueva **instancia** de la clase, pero aplicando algún tipo de transformación o lógica adicional antes de construir el objeto. En este caso, la idea es redondear las coordenadas al entero más cercano y después crear el Punto usando el constructor normal. Para que el método no dependa de ninguna instancia previa, lo habitual es declararlo como static, ya que su propósito es fabricar objetos nuevos desde cero.

### Este tipo de método también mejora la **claridad del código**, ya que refleja explícitamente la intención de crear un punto “ajustado” o “normalizado”, sin obligar al usuario a preocuparse por los detalles internos del redondeo. Además, permite mantener la encapsulación, pues el método factoría actúa como una capa adicional entre el exterior y el constructor real. En muchos lenguajes orientados a objetos, este patrón se usa para proporcionar formas alternativas de construir un objeto.

### public static Punto crearRedondeado(double x, double y) {
###    long xr = Math.round(x);
###    long yr = Math.round(y);
###    return new Punto(xr, yr);
### }

### En efecto, se usa **static** porque el método no pertenece a un objeto ya existente, sino a la clase Punto como entidad encargada de generar nuevas instancias de forma controlada.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Para cambiar la implementación de Punto usando un **array interno** sin modificar la interfaz pública, basta con sustituir las dos variables **double x y double y** por un array privado de dos posiciones. La idea es que desde fuera la clase siga funcionando igual, de modo que los métodos públicos continúen usando los mismos nombres y comportamientos, pero internamente el almacenamiento cambie. Esto demuestra uno de los beneficios de la encapsulación: permite cambiar la representación sin afectar al código que usa la clase.

### Este enfoque mantiene el **constructor, los getters, los setters y cualquier otro método tal como estaban**, simplemente adaptados para leer o escribir sobre el array. Mientras la interfaz pública no cambie, cualquier código que utilice Punto seguirá funcionando sin modificaciones. Con esto se observa que la implementación interna es un detalle oculto, y puede evolucionar sin romper compatibilidad.

### private double[] coords = new double[2];

### public Punto(double x, double y) {
###     coords[0] = x;
###    coords[1] = y;
### }

### public double getX() { return coords[0]; }
### public double getY() { return coords[1]; }

### public void setX(double x) { coords[0] = x; }
### public void setY(double y) { coords[1] = y; }


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Normalmente, aunque un atributo tenga **getter y setter públicos**, no es mejor declararlo público, porque eso **rompería la encapsulación**. Un atributo público puede modificarse libremente desde cualquier parte del código, sin ningún control y sin posibilidad de validar cambios o mantener coherencia interna. En cambio, un atributo privado con métodos de acceso permite intervenir cada modificación, aplicar restricciones o incluso cambiar la representación interna sin afectar a quienes usan la clase.

### La convención más habitual en programación orientada a objetos —incluyendo Java— es declarar todos los atributos como **privados** y exponer solo los métodos necesarios. Esto se debe a que el objetivo es proteger el estado interno del objeto y controlar cómo evoluciona. Incluso cuando existen setters, estos pueden incorporar validaciones o reglas que no serían posibles si el atributo fuera público, manteniendo así un diseño más robusto y predecible.

### Este enfoque está directamente relacionado con las **invariantes de clase**, que son condiciones que siempre deben cumplirse para que un objeto sea válido. Si los **atributos fueran públicos**, cualquier parte del programa podría modificarlos libremente y dejar el objeto en un estado inconsistente. Con **atributos privados**, en cambio, los métodos de la propia clase pueden garantizar que esas invariantes se respetan siempre, haciendo que el comportamiento del objeto sea más seguro y fiable. En conclusión, la encapsulación protege las invariantes y asegura que el objeto siempre permanezca en un estado válido.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Una **clase inmutable** es aquella cuyo estado **no puede cambiar después de ser creada**. Esto significa que todos sus atributos permanecen constantes y no existen métodos que puedan modificar su valor interno una vez construido el objeto. La inmutabilidad garantiza que la instancia es completamente **estable** y **predecible** a lo largo de su vida, lo cual evita errores relacionados con cambios inesperados en el estado. En muchos lenguajes, incluidas las prácticas habituales en Java, esto suele lograrse declarando los atributos como **private** y **final**, y omitiendo cualquier método que permita alterarlos.

### Un método **modificador** es aquel que **cambia** el estado interno del objeto, normalmente actualizando los valores de sus atributos. Aunque un setter es el ejemplo más típico de método modificador, no es el único: cualquier método que altere los atributos, aunque no se llame “set”, se considera modificador. En las clases inmutables, este tipo de métodos simplemente no existe, porque permitirían romper la propiedad de inmutabilidad.

### La **inmutabilidad** tiene varias ventajas importantes: **evita inconsistencias**, facilita el razonamiento sobre el comportamiento del programa y permite compartir objetos sin riesgo de efectos secundarios. Además, las clases inmutables son inherente­mente seguras frente a problemas de concurrencia, ya que varios hilos pueden acceder a ellas simultáneamente sin necesidad de sincronización. Por todo ello, la inmutabilidad es un recurso muy útil cuando se busca diseño robusto y comportamiento predecible en los objetos de un programa. 


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### **No es recomendable incluir métodos setter siempre ni como una simple convención**. En programación orientada a objetos, lo habitual es exponer solo los métodos que realmente se necesitan para mantener las invariantes y el funcionamiento correcto de la clase. Incluir setters indiscriminadamente debilita la encapsulación porque **permite modificar libremente el estado del objeto desde fuera**, lo que puede llevarlo a estados inconsistentes o romper reglas internas que la clase intenta garantizar.

### La convención más aceptada es que los atributos sean privados y que solo existan setters cuando son necesarios para el diseño. Muchas clases no deberían permitir cambios una vez creadas, o solo deberían permitir ciertos tipos de modificaciones controladas. Por eso, un setter no es un requisito obligatorio, sino una herramienta opcional para casos donde tenga sentido que el estado pueda cambiar de forma controlada y validada.

### Esto se relaciona directamente con las **invariantes de clase**, que **son condiciones que siempre deben cumplirse para que el objeto sea válido**. Si un atributo pudiera modificarse libremente mediante un setter innecesario, esas invariantes podrían romperse. En cambio, limitando la presencia de setters, la clase mantiene un mayor control sobre su propio estado y evita errores lógicos difíciles de detectar.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### La clase **String** en Java **es inmutable**, lo que significa que su contenido no puede modificarse después de ser creado. Cada vez que concatenas dos cadenas, en realidad no se altera ninguna de las existentes: Java construye un nuevo objeto String con el resultado. Este comportamiento aporta seguridad y coherencia, pero puede generar un coste elevado cuando se realizan muchas concatenaciones seguidas, porque se crean múltiples objetos intermedios en memoria.

### Cuando la operación implica concatenar muchas veces para formar una cadena muy larga, lo recomendable es usar clases mutables como **StringBuilder** (en contextos no concurrentes) o **StringBuffer** (si se necesita sincronización). Estas permiten modificar el contenido sin crear objetos adicionales en cada paso, lo que hace que el proceso sea mucho más eficiente. De esta forma, el programa evita sobrecargar la memoria y mejora su rendimiento en operaciones repetitivas de construcción de texto.

### La elección de estas clases mutables está motivada precisamente por la **inmutabilidad de String**, que, aunque segura y conveniente para la mayoría de usos, puede resultar costosa en escenarios de concatenaciones repetidas. Por eso, en algoritmos que construyen una cadena paso a paso, se trabaja con StringBuilder y solo al final, cuando ya se tiene el resultado completo, se convierte a String.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### En programación orientada a objetos, los objetos pueden **compararse por identidad o por contenido**. La identidad hace referencia a si dos referencias apuntan al mismo objeto en memoria, lo cual en Java se comprueba con el operador ==. El contenido, en cambio, implica comparar si dos objetos distintos representan la misma información lógica. Para esto se emplea un método específico que cada clase debe definir según sus propias reglas de igualdad.

### En Java, el **método encargado de comparar el contenido es equals**, heredado de Object. Sin embargo, su comportamiento por defecto es comparar identidad, es decir, funciona como == salvo que la clase lo sobrescriba. Para que dos objetos se consideren iguales por su estado interno, es necesario redefinir equals y establecer allí los criterios apropiados de comparación. Esto es fundamental para estructuras como listas, conjuntos o mapas, donde la igualdad semántica de los objetos determina su funcionamiento.

### En el caso particular de las **cadenas**, Java redefine equals en la **clase String para que compare el contenido de los caracteres**, no la identidad en memoria. Esto significa que dos cadenas con el mismo texto serán iguales aunque sean dos objetos distintos. Por esta razón, para comparar cadenas en Java siempre se debe usar equals, y no ==, que solo indicaría si son la misma referencia. Este diseño evita confusiones y garantiza una comparación basada en el significado del texto, no en su ubicación en memoria.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Las clases **wrapper son clases que encapsulan (“envuelven”) un tipo primitivo para poder tratarlo como un objeto dentro de un lenguaje orientado a objetos**. En Java, por ejemplo, **int** se envuelve en **Integer**, **double** en **Double**, **boolean** en **Boolean**, y así sucesivamente. Su propósito es permitir que los valores primitivos puedan usarse allí donde solo se aceptan objetos, como en colecciones genéricas (ArrayList<Integer>, por ejemplo), ya que estas no pueden almacenar tipos primitivos directamente. Esto proporciona una integración más uniforme entre tipos básicos y tipos definidos por el usuario.

### La conversión entre primitivo y wrapper puede hacerse de manera **explícita**, pero en Java es un **proceso automático gracias a autoboxing y unboxing**. Esto significa que el compilador convierte un primitivo en su wrapper cuando es necesario, y a la inversa cuando se requiere un valor primitivo, sin que el programador tenga que escribir esa conversión manualmente. Esta automatización simplifica el código y evita errores frecuentes relacionados con conversiones repetitivas o tediosas.

### Las clases wrapper también ofrecen métodos adicionales, como funciones de conversión, comprobación y manipulación de valores, que no existen para los tipos primitivos. Estas utilidades hacen más flexible el trabajo con datos numéricos o lógicos en contextos avanzados. Sin embargo, no todos los lenguajes orientados a objetos tienen tipos primitivos; algunos, como Python, tratan todos los valores como objetos y no necesitan wrappers. En esos casos, el lenguaje ya unifica el tratamiento de sus tipos sin requerir una capa adicional de envoltura.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Un tipo de **dato enumerado** es una **construcción que permite definir un conjunto finito y cerrado de valores posibles, normalmente representando estados o categorías que no deben cambiar**. En POO, su función es garantizar que una variable solo pueda tomar uno de los valores predefinidos, evitando errores y proporcionando una representación más clara y segura que usar simples enteros o cadenas. Los enumerados expresan la intención del programador y refuerzan la idea de que ciertos valores pertenecen a un dominio limitado y bien definido.

### En Java, un tipo enumerado (enum) es efectivamente una clase especial, aunque el lenguaje la trata con una sintaxis simplificada. Esto significa que un enum puede contener métodos, atributos, constructores privados y lógica interna, como cualquier otra clase. Sin embargo, su conjunto de instancias está fijado de antemano y no se pueden crear nuevos objetos fuera de los valores definidos. Este diseño proporciona encapsulación natural y evita manipulaciones incorrectas del conjunto de valores válidos.

### En términos de encapsulación, los enumerados en Java son especialmente ventajosos porque limitan estrictamente los valores permitidos y encapsulan la representación interna de cada elemento. Además, permiten asociar comportamientos específicos a cada valor, manteniendo el código más organizado y reduciendo la posibilidad de errores relacionados con valores inválidos. Gracias a esto, los enum ofrecen una forma segura, expresiva y robusta de modelar dominios cerrados dentro de un programa orientado a objetos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### public enum Mes {
###    ENERO(1, 31), FEBRERO(2, 28), MARZO(3, 31), ABRIL(4, 30),
###    MAYO(5, 31), JUNIO(6, 30), JULIO(7, 31), AGOSTO(8, 31),
###    SEPTIEMBRE(9, 30), OCTUBRE(10, 31), NOVIEMBRE(11, 30), DICIEMBRE(12, 31);

###    private final int ordinal;
###    private final int dias;

###    private Mes(int ordinal, int dias) {
###        this.ordinal = ordinal;
###        this.dias = dias;
###    }

###    public int getDias() { return dias; }
###    public int getOrdinal() { return ordinal; }

###    public boolean esDePrimavera(boolean enHemisferioNorte) {
###        return enHemisferioNorte ?
###            (ordinal >= 3 && ordinal <= 5) :
###            (ordinal >= 9 && ordinal <= 11);
###    }

###    public boolean esDeVerano(boolean enHemisferioNorte) {
###        return enHemisferioNorte ?
###            (ordinal >= 6 && ordinal <= 8) :
###            (ordinal == 12 || ordinal <= 2);
###    }

###    public boolean esDeOtono(boolean enHemisferioNorte) {
###        return enHemisferioNorte ?
###            (ordinal >= 9 && ordinal <= 11) :
###            (ordinal >= 3 && ordinal <= 5);
###    }

###    public boolean esDeInvierno(boolean enHemisferioNorte) {
###        return enHemisferioNorte ?
###            (ordinal == 12 || ordinal <= 2) :
###            (ordinal >= 6 && ordinal <= 8);
###    }
### }
