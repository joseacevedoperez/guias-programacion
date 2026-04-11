<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### En orientación a objetos, la herencia es el mecanismo por el que una clase nueva (subclase) reutiliza y amplía otra existente (superclase). Su relación con la idea “A es-un B” significa que un Artillero es-un Soldado y un Zapador es-un Soldado: pueden usarse allí donde se espere un Soldado. Esta relación no describe “tiene-un” (composición), sino sustitución: si una función trabaja con Soldado, también puede trabajar con cualquiera de sus subtipos sin cambiar el código.

### Las dos implicaciones principales son: (1) compatibilidad de tipos, que permite asignar un objeto de una subclase a una referencia de la superclase (polimorfismo); y (2) herencia de estado y comportamiento, por la que los atributos y métodos no privados de la superclase están disponibles en la subclase. En Java, el atributo puede ser private (encapsulación) y las subclases accederán a él mediante métodos heredados (por ejemplo, el método saludar()), manteniendo el control del estado.

### // Superclase
### class Soldado {
###    private String nombre;

###    public Soldado(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### // Subclase Artillero
### class Artillero extends Soldado {
###    private int numeroCohetes;

###    public Artillero(String nombre, int numeroCohetes) {
###        super(nombre);
###        this.numeroCohetes = numeroCohetes;
###    }

###    public int getNumeroCohetes() {
###        return numeroCohetes;
###    }
### }

### // Subclase Zapador
### class Zapador extends Soldado {
###    private int numeroMinas;

###    public Zapador(String nombre, int numeroMinas) {
###        super(nombre);
###        this.numeroMinas = numeroMinas;
###    }

###    public int getNumeroMinas() {
###        return numeroMinas;
###    }
### }

### // Uso: compatibilidad de tipos (polimorfismo)
### public class Main {
###    public static void main(String[] args) {
###        Soldado[] escuadron = new Soldado[] {
###            new Soldado("Luis"),
###            new Artillero("Ana", 5),
###            new Zapador("Carlos", 3)
###        };

###        for (Soldado s : escuadron) {
###            s.saludar(); // todos pueden saludar aunque sean tipos distintos
###        }
###    }
### }



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Al crear un objeto de una subclase en Java, siempre se ejecutan dos constructores: primero el de la clase base y después el de la clase concreta. El orden es fijo y va de arriba hacia abajo en la jerarquía de herencia, aunque el objeto final sea de la subclase. Esto garantiza que el estado heredado del objeto se inicialice correctamente antes de añadir el estado propio de la subclase.

### La palabra clave super dentro de un constructor sirve para invocar explícitamente un constructor de la clase padre. Esta llamada debe ser la primera instrucción del constructor. Si no se escribe super(...), Java intenta automáticamente llamar a super() (el constructor sin parámetros de la clase base). Por tanto, super es el mecanismo que asegura que la inicialización de la superclase se haga correctamente.

### Si la clase base no tiene un constructor sin parámetros visible, entonces sí, estás obligado a llamar a super explícitamente con los parámetros adecuados; de lo contrario, el código no compila. Esto es muy habitual cuando la clase base fuerza a inicializar ciertos atributos (por ejemplo, nombre) desde el constructor.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

###  los atributos privados de la superclase sí forman parte de la instancia de una subclase en memoria. Cuando se crea un objeto de una subclase, ese objeto contiene internamente la parte correspondiente a la superclase y la parte específica de la subclase. Es decir, un objeto Artillero incluye en memoria tanto sus propios atributos como el atributo nombre definido en Soldado, aunque este sea private.

### Sin embargo, que el atributo privado exista dentro del objeto no implica que la subclase pueda acceder a él directamente en el código. El modificador private protege el acceso desde fuera de la propia clase que declara el atributo, incluidas sus subclases. Por tanto, una subclase no puede usar directamente el atributo nombre, pero sí puede utilizar métodos públicos o protegidos heredados de la superclase que acceden internamente a ese atributo.

### Esto se ve claramente con el ejemplo de Soldado y Artillero: el Artillero contiene el atributo nombre, pero no puede acceder a él directamente. En cambio, puede llamar al método saludar(), que pertenece a Soldado y usa internamente ese atributo privado. Así, la herencia reutiliza el estado, pero la encapsulación sigue intacta.

### class Soldado {
###    private String nombre;

###    public Soldado(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### class Artillero extends Soldado {
###    private int cohetes;

###    public Artillero(String nombre, int cohetes) {
###        super(nombre);
###        this.cohetes = cohetes;
###    }

###    public void mostrarCohetes() {
###        System.out.println("Tengo " + cohetes + " cohetes");
###        // System.out.println(nombre); // ERROR: nombre es private en Soldado
###    }
### }

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### En términos de extensibilidad del código, que los objetos sean compatibles a nivel de tipos implica que el sistema puede crecer añadiendo nuevas clases sin modificar el código existente que trabaja con la superclase. Este es uno de los grandes beneficios del polimorfismo y está directamente relacionado con el principio open/closed: el código está abierto a extensión (se pueden añadir nuevos tipos) pero cerrado a modificación (no hay que tocar lo que ya funciona).

### Aplicado al ejemplo, si todo el código que gestiona soldados trabaja con el tipo Soldado, podemos crear nuevos tipos concretos (por ejemplo, un Medico) y seguir usándolos exactamente igual. El código que “pide el saludo a todos los soldados” no necesita saber qué tipo concreto es cada uno; solo necesita saber que todos son Soldado y saben saludar. Esto reduce errores, mejora el mantenimiento y hace el sistema más flexible a cambios futuros.

### Esto se demuestra añadiendo un nuevo subtipo de Soldado y comprobando que el código que recorre el array no se modifica en absoluto:

### // NUEVO TIPO, añadido sin tocar el código existente
### class Medico extends Soldado {
###    public Medico(String nombre) {
###        super(nombre);
###    }
### }

### public class Main {
###    public static void main(String[] args) {
###        Soldado[] ejercito = {
###            new Artillero("Ana"),
###            new Zapador("Luis"),
###            new Medico("Carmen")
###        };

###        for (Soldado s : ejercito) {
###            s.saludar(); // este código no cambia nunca
###        }
###    }
### }

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### En Java es totalmente válido tener una referencia del supertipo que apunte a un objeto real de un subtipo. Esto es la base del polimorfismo: por ejemplo, una variable de tipo Soldado puede referenciar un objeto Artillero o Zapador. Sin embargo, usando una referencia del supertipo solo se pueden invocar los métodos que estén definidos en el supertipo, aunque el objeto real sea de un subtipo. No es posible llamar directamente a métodos públicos exclusivos del subtipo desde la referencia del supertipo.

### El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo; es implícito y seguro, ya que siempre se cumple la relación “es-un”. Por el contrario, el downcasting es convertir una referencia del supertipo en una del subtipo para acceder a comportamientos específicos; este proceso es explícito y puede ser peligroso si el objeto real no es del tipo esperado. Para evitar errores en tiempo de ejecución, Java proporciona el operador instanceof, que permite comprobar el tipo real del objeto antes de hacer el casting.

### En el contexto de un array de Soldado, todos los objetos pueden almacenarse gracias al upcasting, pero para acceder a datos específicos (como el número de cohetes de un Artillero) es necesario comprobar primero el tipo real con instanceof y luego hacer downcasting. Así se combina flexibilidad con seguridad de tipos.

### class Soldado {
###    private String nombre;

###    public Soldado(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### class Artillero extends Soldado {
###    private int cohetes;

###    public Artillero(String nombre, int cohetes) {
###        super(nombre);
###        this.cohetes = cohetes;
###    }

###    public int getCohetes() {
###        return cohetes;
###    }
### }

### class Zapador extends Soldado {
###    public Zapador(String nombre) {
###        super(nombre);
###    }
### }

### public class Main {
###    public static void main(String[] args) {
###        Soldado[] grupo = {
###            new Artillero("Ana", 5),
###            new Zapador("Luis"),
###            new Artillero("Carlos", 3)
###        };

###        for (Soldado s : grupo) {
###            s.saludar();
###            if (s instanceof Artillero) {
###                Artillero a = (Artillero) s; // downcasting seguro
###                System.out.println("Cohetes: " + a.getCohetes());
###            }
###        }
###    }
### }


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### En ocultación de información y herencia, el modificador de acceso protected significa que un atributo o método no es accesible desde cualquier clase, pero sí lo es desde las subclases y desde las clases del mismo paquete. Es un punto intermedio entre private (solo accesible en la propia clase) y public (accesible desde cualquier lugar). Se usa cuando queremos proteger el estado, pero permitir que las subclases lo reutilicen directamente.

### En Java, protected se implementa simplemente declarando así el atributo o método. A diferencia de private, una subclase puede acceder directamente a un miembro protected sin necesidad de getters o setters. Esto es útil cuando una subclase necesita usar datos internos de la superclase para implementar su propio comportamiento, manteniendo el acceso restringido al exterior del sistema.

### Aplicándolo al ejemplo, si Soldado tiene el atributo nombre como protected, una subclase como Zapador puede usarlo directamente en su método de poner bombas. El atributo sigue estando oculto para el resto del programa, pero accesible para las clases hijas, lo que equilibra encapsulación y reutilización.

### class Soldado {
###    protected String nombre;

###    public Soldado(String nombre) {
###        this.nombre = nombre;
###    }

###    public void saludar() {
###        System.out.println("Hola, soy " + nombre);
###    }
### }

### class Zapador extends Soldado {
###    private int minas;

###    public Zapador(String nombre, int minas) {
###        super(nombre);
###        this.minas = minas;
###    }

###    public void ponerBombas() {
###        System.out.println(nombre + " está poniendo bombas. Minas: " + minas);
###    }
### }


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### En los lenguajes orientados a objetos suele existir la idea de una clase base común de la que heredan todos los objetos, aunque no ocurre igual en todos los lenguajes. Conceptualmente, esta clase raíz permite definir comportamientos y operaciones comunes a cualquier objeto (por ejemplo, comparación, conversión a texto o identidad). Sin embargo, la existencia de esta clase y su obligatoriedad dependen del lenguaje concreto y de su modelo de objetos.

### No todos los lenguajes orientados a objetos tienen una clase base universal explícita. Por ejemplo, en C++ no existe una clase raíz obligatoria: una clase puede no heredar de ninguna otra. En otros lenguajes como Python o Smalltalk, sí existe una clase base común (object en Python), pero el grado en que el programador interactúa con ella varía. Por tanto, la herencia desde una clase base universal es una decisión de diseño del lenguaje, no una característica imprescindible de la orientación a objetos.

### En Java, sí existe claramente una clase base para todos los objetos: java.lang.Object. Toda clase que se define en Java hereda de Object, incluso aunque no se indique explícitamente con extends. Esto permite que todos los objetos compartan métodos comunes como toString(), equals(), hashCode() o getClass(), y hace posible tratar cualquier objeto de forma uniforme cuando se trabaja a nivel genérico.

### En resumen, la idea de una clase base universal es frecuente pero no universal en todos los lenguajes OO. En Java es un elemento central del lenguaje y del diseño del sistema de tipos, mientras que en otros lenguajes puede no existir o no ser obligatoria. 


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### La herencia múltiple es el mecanismo por el cual una clase puede heredar simultáneamente de más de una clase base, obteniendo así atributos y métodos de todas ellas. Esto permite combinar comportamientos de distintas jerarquías, pero también introduce problemas de diseño, como el conflicto de nombres o el llamado problema del diamante, donde no queda claro de qué clase base procede una implementación concreta.

### No todos los lenguajes orientados a objetos admiten herencia múltiple de clases. Algunos, como C++, la permiten directamente, dejando en manos del programador la resolución de conflictos. Otros lenguajes, en cambio, deciden prohibirla para simplificar el modelo de herencia y evitar ambigüedades difíciles de detectar y mantener en programas grandes.

### En Java no existe herencia múltiple de clases: una clase solo puede extender una única clase base. Sin embargo, Java introduce una alternativa segura mediante las interfaces, que sí pueden implementarse múltiples a la vez. De este modo, Java permite heredar comportamiento abstracto (qué métodos existen) de varias fuentes, pero evita heredar estado y código concreto de más de una clase, eliminando los problemas clásicos de la herencia múltiple.

### En resumen, la herencia múltiple consiste en heredar de varias clases a la vez; Java no la permite para clases, pero la reemplaza por la implementación múltiple de interfaces, logrando un equilibrio entre flexibilidad, claridad y seguridad del diseño orientado a objetos.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### En los lenguajes orientados a objetos, las excepciones son objetos, por lo que pueden extender clases de excepción existentes y contener estado adicional. En Java, una excepción es no controlada (unchecked) cuando hereda de RuntimeException. Este tipo de excepciones no obliga a capturarlas ni declararlas con throws, y suelen usarse para errores de programación o situaciones excepcionales difíciles de recuperar.

### Crear una excepción personalizada permite encapsular información relevante sobre el error. En este caso, la excepción UsuarioNoEncontradoException contiene un objeto Usuario para identificar qué usuario causó el problema, y además permite encadenar causas mediante la sobrecarga de constructores que aceptan un Throwable. Esto es importante para no perder el contexto del error original y facilitar el diagnóstico.

### class Usuario {
###    private String nombre;

###    public Usuario(String nombre) {
###        this.nombre = nombre;
###    }

###    public String getNombre() {
###        return nombre;
###    }
### }

### // Excepción personalizada no controlada
### class UsuarioNoEncontradoException extends RuntimeException {
###    private Usuario usuario;

###    public UsuarioNoEncontradoException(Usuario usuario) {
###        super("Usuario no encontrado: " + usuario.getNombre());
###        this.usuario = usuario;
###    }

###    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
###        super("Usuario no encontrado: " + usuario.getNombre(), causa);
###        this.usuario = usuario;
###    }

###    public Usuario getUsuario() {
###        return usuario;
###    }
### }


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### La recomendación de no usar herencia solo para reutilizar código se debe a que la herencia establece una relación muy fuerte y rígida entre clases: la relación “es‑un”. Cuando una clase hereda de otra, queda acoplada a su implementación y a su evolución futura. Si la clase base cambia, puede romper el comportamiento de todas las subclases, incluso aunque esas subclases solo se crearan para “aprovechar” unos métodos ya hechos.

### Además, usar herencia únicamente por reutilización suele llevar a jerarquías mal diseñadas, donde la relación conceptual no es correcta. Esto hace el código más difícil de entender, mantener y extender, y puede provocar que las subclases hereden métodos o atributos que no tienen sentido para ellas. En esos casos, aunque se gane reutilización a corto plazo, se pierde claridad y robustez a largo plazo.

### La composición es preferible cuando solo se quiere reutilizar funcionalidad porque crea una relación “tiene‑un” en lugar de “es‑un”. Con composición, una clase utiliza otras clases internamente sin depender de su jerarquía, lo que reduce el acoplamiento y hace el diseño mucho más flexible frente a cambios. Se puede cambiar la implementación sin afectar a la interfaz pública ni a otras clases.

### En resumen, la herencia debe usarse cuando existe una verdadera relación conceptual de especialización, no como un atajo para reutilizar código. Para reutilización simple, la composición es más segura, más flexible y conduce a diseños más mantenibles.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Se dice que hay que favorecer la composición frente a la herencia porque la composición produce diseños más flexibles y menos acoplados. La herencia establece una relación rígida “es‑un” y crea una dependencia fuerte entre la subclase y la superclase, lo que hace que los cambios en la clase base puedan afectar de forma inesperada a todas las subclases. En cambio, la composición establece relaciones “tiene‑un”, permitiendo usar funcionalidades de otras clases sin quedar atado a su jerarquía.

### Otro motivo clave es que la composición evita heredar comportamientos que no tienen sentido. Con herencia, una subclase hereda todos los métodos y atributos visibles de la superclase, incluso aquellos que no aplica a su lógica, lo que puede llevar a diseños forzados o inconsistentes. La composición permite elegir exactamente qué funcionalidad utilizar y cómo hacerlo, manteniendo las responsabilidades mejor delimitadas.

### Además, la composición mejora la extensibilidad y el mantenimiento del software. Al estar menos cohesionadas las clases, es más fácil cambiar implementaciones internas, sustituir objetos o añadir nuevas combinaciones de comportamiento sin modificar código existente. Esto encaja mejor con sistemas grandes, donde los requisitos cambian con frecuencia.

### En resumen, se favorece la composición porque reduce el acoplamiento, aumenta la flexibilidad y conduce a diseños más robustos y mantenibles. La herencia sigue siendo válida cuando existe una verdadera relación conceptual de especialización, pero no debe ser la primera opción cuando el objetivo principal es reutilizar comportamiento.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Cuando se dice que “la herencia rompe la encapsulación”, se refiere a que las subclases pasan a depender de los detalles internos de la clase base, más allá de su interfaz pública. Aunque la encapsulación busca ocultar la implementación y exponer solo lo necesario, la herencia permite que las subclases conozcan y utilicen miembros protected (e incluso el comportamiento implícito de métodos públicos), creando una dependencia directa con cómo está implementada la superclase.

### El problema aparece cuando la implementación interna de la superclase cambia: aunque su interfaz pública no varíe, esos cambios pueden afectar inesperadamente al comportamiento de las subclases, rompiendo su funcionamiento. En ese sentido, la superclase deja de ser una “caja negra” para las subclases, que pasan a estar acopladas a decisiones internas que no deberían conocer. Esto va en contra del objetivo principal de la encapsulación, que es aislar cambios.

### Además, la herencia puede forzar a las subclases a aceptar invariantes, estados y flujos de ejecución que no controlan completamente, lo que dificulta razonar sobre su comportamiento. Muchas veces, para implementar correctamente una subclase, el programador necesita conocer detalles internos de la superclase, lo que evidencia que la encapsulación se ha debilitado.

### Por eso se dice que la herencia tiende a romper la encapsulación, mientras que la composición la preserva mejor: al usar composición, las clases interactúan a través de interfaces bien definidas y no dependen de la implementación interna de otras, manteniendo así un diseño más robusto y resistente a cambios.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Para modelar los mismos datos comunes (DNI y nombre) en Estudiante y Trabajador podemos hacerlo de dos maneras distintas: mediante herencia o mediante composición. En la alternativa con herencia, ambos tipos son una especialización de una superclase común Persona, lo que expresa una relación clara “es‑una Persona”. En la alternativa con composición, Estudiante y Trabajador no heredan, sino que tienen un objeto DatosPersonales que encapsula la información compartida.

### La opción con herencia es sencilla y natural cuando existe una auténtica relación conceptual. Persona concentra los datos comunes y las subclases los reutilizan directamente. Sin embargo, esta solución crea un acoplamiento fuerte con la superclase y obliga a que cualquier cambio en Persona afecte a todas las subclases, incluso aunque solo compartan parte de la información.

### La opción con composición separa los datos comunes en una clase independiente (DatosPersonales) y hace que Estudiante y Trabajador la utilicen como atributo, recibiendo una instancia en su constructor. Esto reduce el acoplamiento, mejora la encapsulación y permite reutilizar DatosPersonales en otros contextos sin forzar una jerarquía artificial. Por eso, desde el punto de vista del diseño, la composición suele ser preferible.

### Alternativa 1 — Herencia:

### class Persona {
###    protected String dni;
###    protected String nombre;

###    public Persona(String dni, String nombre) {
###        this.dni = dni;
###        this.nombre = nombre;
###    }
### }

### class Estudiante extends Persona {
###    public Estudiante(String dni, String nombre) {
###        super(dni, nombre);
###    }
### }

### class Trabajador extends Persona {
###    public Trabajador(String dni, String nombre) {
###        super(dni, nombre);
###    }
### }

### Alternativa 2 — Composición:

### class DatosPersonales {
###    private String dni;
###    private String nombre;

###    public DatosPersonales(String dni, String nombre) {
###        this.dni = dni;
###        this.nombre = nombre;
###    }

###    public String getDni() {
###        return dni;
###    }

###    public String getNombre() {
###        return nombre;
###    }
### }

### class Estudiante {
###    private DatosPersonales datos;

###    public Estudiante(DatosPersonales datos) {
###        this.datos = datos;
###    }
### }

### class Trabajador {
###    private DatosPersonales datos;

###    public Trabajador(DatosPersonales datos) {
###        this.datos = datos;
###    }
### }


