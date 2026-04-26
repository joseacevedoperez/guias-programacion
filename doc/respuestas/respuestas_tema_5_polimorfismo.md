<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### En programación orientada a objetos, el polimorfismo es la capacidad que tienen distintos objetos de responder de manera diferente a un mismo mensaje o método, aunque se use el mismo nombre. Desde lo que hemos visto en primero, esto sirve sobre todo para escribir programas más flexibles y reutilizables, porque el código que usa los objetos no necesita saber exactamente de qué tipo concreto son, solo que cumplen una interfaz común o heredan de una misma clase base.

### Gracias al polimorfismo, se puede tratar a varios objetos distintos como si fueran del mismo tipo general y dejar que cada uno se comporte a su manera. Por ejemplo, una función puede trabajar con una clase padre y, en tiempo de ejecución, ejecutarse el método correspondiente de la clase hija concreta. Esto ayuda mucho a organizar programas grandes y a evitar duplicar código, que es algo que nos recalcan bastante en la asignatura.

### La sobreescritura de métodos está muy relacionada con esto. Consiste en que una clase hija redefine un método que ya existía en su clase padre, pero manteniendo el mismo nombre (y normalmente la misma firma). De esta forma, cuando se llama a ese método desde una referencia al padre, se ejecuta la versión de la clase hija. En resumen, la sobreescritura es uno de los mecanismos clave que permiten que el polimorfismo funcione correctamente en la programación orientada a objetos.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### La ligadura dinámica, también llamada enlace tardío, consiste en que la decisión de qué método concreto se va a ejecutar se toma en tiempo de ejecución y no en tiempo de compilación. Es decir, el programa decide qué versión del método usar según el tipo real del objeto en ese momento, y no según el tipo de la variable que lo referencia. Esto está directamente relacionado con el polimorfismo, porque permite que una referencia a una clase base ejecute el método sobrescrito de una clase hija sin cambiar el código que hace la llamada.

### En cuanto a si hay que indicarlo explícitamente, depende del lenguaje. En C++, la ligadura dinámica no es automática: hay que marcar los métodos como virtual en la clase base para que se resuelvan dinámicamente. Si no se hace, el enlace es estático y se ejecuta el método según el tipo de la variable. En cambio, en Java la ligadura dinámica es el comportamiento por defecto para los métodos de instancia; basta con sobrescribir el método y Java se encarga de resolverlo en tiempo de ejecución, sin que el programador tenga que hacer nada especial.

### En Python, el enlace dinámico es aún más natural, porque es un lenguaje dinámicamente tipado. Python no decide qué método se llama hasta el momento exacto de la ejecución, basándose únicamente en el objeto real. No hay que declarar métodos virtuales ni usar palabras clave especiales: si un objeto tiene un método con ese nombre, se ejecuta. Por eso, en Python el polimorfismo y la ligadura dinámica están siempre presentes, incluso sin herencia, algo que al principio cuesta entender pero que da mucha flexibilidad.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### En este ejemplo se puede ver claramente cómo funciona el polimorfismo en Java usando una clase base Soldado y dos subclases. La idea es que todas las referencias sean de tipo Soldado, pero que el comportamiento real del método saluda() dependa del tipo concreto del objeto en tiempo de ejecución. Esto es posible porque en Java los métodos se enlazan dinámicamente por defecto cuando se sobrescriben.

### El Zapador sobrescribe completamente el método saluda(), cambiando su comportamiento respecto al de la clase base, mientras que Artillero hereda el método sin modificarlo. Al recorrer un array de Soldado y llamar a saluda(), Java decide automáticamente qué versión ejecutar según el objeto real, no según el tipo de la referencia.

### // Clase base
### public class Soldado {
###    public void saluda() {
###        System.out.println("Soy un soldado del ejército.");
###    }
### }

### // Subclase Zapador
### public class Zapador extends Soldado {
###    @Override
###    public void saluda() {
###        System.out.println("Soy un zapador y me encargo de explosivos.");
###    }
### }

### // Subclase Artillero
### public class Artillero extends Soldado {
###    // No sobrescribe saluda(), usa el de Soldado
### }

### // Clase principal para probar el polimorfismo
### public class Main {
###    public static void main(String[] args) {
###        Soldado[] ejercito = new Soldado[2];
###        ejercito[0] = new Zapador();
###        ejercito[1] = new Artillero();

###        for (Soldado s : ejercito) {
###            s.saluda();
###        }
###    }
### }

### Si ejecutamos este programa, se verá que, aunque todas las posiciones del array son de tipo Soldado, cada objeto responde con su propio comportamiento. Esto demuestra que el método que se ejecuta se decide en tiempo de ejecución, que es justo lo que se explica cuando hablamos de ligadura dinámica y polimorfismo en Java.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Sí, cuando se sobreescribe un método es perfectamente posible invocar el método de la clase base y trabajar a partir de su resultado. Esto es muy útil cuando no queremos cambiar completamente el comportamiento, sino solo añadir o modificar una pequeña parte. Desde el punto de vista de primero, se puede ver como “reutilizar” el comportamiento del padre sin tener que reescribirlo todo, lo cual sigue el principio de no duplicar código.

### En Java esto se hace usando la palabra clave super, que permite acceder directamente a los métodos (o atributos) de la clase padre. En el caso del ejemplo, el Zapador puede llamar primero al saluda() del Soldado base y después añadir su propio mensaje. Así, el método sobrescrito no sustituye todo el comportamiento, sino que lo amplía, manteniendo la coherencia con la clase base.

### // Clase base
### public class Soldado {
###    public void saluda() {
###        System.out.println("Soy un soldado del ejército.");
###    }
### }

### // Subclase Zapador
### public class Zapador extends Soldado {
###    @Override
###    public void saluda() {
###        super.saluda();
###        System.out.println("ZAPADOR A SUS ORDENES");
###    }
### }

### De esta forma, cuando se llama a saluda() sobre un objeto Zapador, primero se ejecuta el saludo normal del soldado base y luego se añade el mensaje específico del zapador. La palabra clave utilizada para invocar el método de la clase base es super, y es fundamental para combinar herencia, sobreescritura y polimorfismo de manera correcta.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Al sobreescribir un método en Java, hay varias restricciones importantes. Los parámetros deben ser exactamente los mismos que en el método de la clase base, tanto en número como en tipo y orden; si cambian, ya no es sobreescritura. En cuanto al tipo de retorno, debe ser el mismo o un subtipo del original (lo que se llama tipo de retorno covariante). Además, no se puede reducir la visibilidad del método (por ejemplo, no se puede pasar de public a protected) y tampoco lanzar excepciones más generales que las declaradas en el método base.

### La sobreescritura (overriding) y la sobrecarga (overloading) son conceptos distintos aunque a veces se confundan. La sobreescritura ocurre entre una clase padre y una hija, y sirve para cambiar o ampliar el comportamiento de un método heredado, estando relacionada directamente con el polimorfismo y la ligadura dinámica. En cambio, la sobrecarga consiste en definir varios métodos con el mismo nombre en la misma clase, pero con parámetros diferentes, y se resuelve en tiempo de compilación. Es decir, la sobrecarga no tiene que ver con herencia ni con polimorfismo.

### La anotación @Override se utiliza para indicar explícitamente que un método está sobrescribiendo otro de la clase base. Aunque no es obligatoria, es muy recomendable usarla siempre porque el compilador comprueba que la sobreescritura sea correcta. Si el nombre está mal escrito o los parámetros no coinciden, el compilador dará error, evitando fallos difíciles de detectar. Desde el punto de vista de un estudiante de primero, se puede entender como una forma de “protegerse” de errores y hacer el código más claro y mantenible.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Sí, el polimorfismo en Java se empieza a usar prácticamente desde el principio, aunque muchas veces no seamos conscientes de ello. Cuando en las primeras prácticas se trabaja con clases básicas como Object, ya se está apoyando el lenguaje en polimorfismo. Por ejemplo, todas las clases heredan implícitamente de Object, así que cuando se usan métodos como toString() o equals() con distintos objetos, Java decide en tiempo de ejecución qué implementación concreta debe ejecutar según el objeto real.

### Cuando sobreescribimos toString() o equals(), efectivamente ya estamos usando polimorfismo, aunque sea de forma muy básica. Si una variable es de tipo Object pero apunta a una instancia de nuestra clase, y se llama a toString(), se ejecuta la versión sobrescrita de nuestra clase y no la de Object. Esto es exactamente la idea del polimorfismo: la misma llamada a un método produce comportamientos distintos según el objeto concreto.

### Por eso se puede decir que Java “enseña” polimorfismo casi sin decirlo explícitamente al principio. Primero se usa de forma práctica y natural, y más adelante se formaliza el concepto al estudiar herencia, ligadura dinámica y sobreescritura. Desde el punto de vista de un alumno de primero, es importante darse cuenta de que no es algo avanzado que aparece de repente, sino un mecanismo fundamental del lenguaje que ya se está usando desde las primeras clases.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Una clase abstracta es una clase que sirve como modelo o base para otras clases, pero que no está pensada para crear objetos directamente. Se usa cuando queremos definir un comportamiento común para varias subclases, pero dejando partes sin implementar para que cada una las concrete a su manera. Un método abstracto es un método que solo se declara (no tiene cuerpo) y obliga a las clases hijas a implementarlo. Por eso, no se pueden crear instancias de una clase abstracta, ya que está incompleta por definición.

### En Java, la palabra clave abstract se debe poner tanto en la clase como en los métodos abstractos. Si una clase tiene al menos un método abstracto, la clase entera debe ser abstracta. En el ejemplo del Soldado, podemos hacer que todos los soldados sepan “atacar”, pero que cada tipo ataque de forma distinta. Así, el método atacar() se declara en Soldado como abstracto y cada subclase está obligada a implementarlo.

### // Clase abstracta
### public abstract class Soldado {
###    public void saluda() {
###        System.out.println("Soy un soldado del ejército.");
###    }

###    public abstract void atacar();
### }

### // Subclase Zapador
### public class Zapador extends Soldado {
###    @Override
###    public void atacar() {
###        System.out.println("El zapador coloca explosivos.");
###    }
### }

### // Subclase Artillero
### public class Artillero extends Soldado {
###    @Override
###    public void atacar() {
###        System.out.println("El artillero dispara con la artillería.");
###    }
### }

### Con este diseño, el polimorfismo vuelve a aparecer, ya que se puede usar una referencia de tipo Soldado y llamar a atacar(), ejecutándose el comportamiento correcto según el tipo real del objeto. En resumen, abstract se pone en la clase para impedir instanciarla y en los métodos para obligar a las subclases a definir su comportamiento, algo muy típico cuando se empieza a diseñar jerarquías en programación orientada a objetos.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### En Java, la palabra clave final aplicada a métodos y clases sirve para impedir cambios mediante herencia. Si un método es final, no puede ser sobrescrito por las clases hijas, aunque se herede. Si una clase es final, no puede tener subclases, es decir, no se puede extender. La idea principal es asegurar que cierto comportamiento o diseño no sea modificado, algo que suele hacerse por motivos de seguridad, rendimiento o coherencia del programa.

### La relación con el polimorfismo es bastante directa: el polimorfismo se basa en poder sobreescribir métodos y que el comportamiento se decida en tiempo de ejecución. Si un método es final, se rompe esa posibilidad, porque todas las llamadas a ese método se comportarán igual, independientemente de la clase hija. Y si una clase es final, directamente no existe polimorfismo con ella, ya que no se pueden crear jerarquías a partir de esa clase.

### Un ejemplo muy típico dentro de la API estándar de Java es la clase String, que es final. Esto significa que no se puede crear una clase que herede de String para cambiar su comportamiento, lo cual es importante para evitar errores y problemas de seguridad. Otras clases finales bastante conocidas son Integer, Double o Math. Desde el punto de vista de un alumno de primero, se puede ver final como una forma de “cerrar” una clase o un método para que no entre en el juego del polimorfismo, dejando claro que ese diseño no debe alterarse.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### En Java, las interfaces son una forma de definir qué métodos debe tener una clase, pero sin implementar su comportamiento. Es decir, una interfaz solo marca un “contrato” que las clases que la implementen están obligadas a cumplir. Desde primero se suelen explicar como una lista de métodos públicos que garantizan que distintas clases, aunque no tengan relación entre ellas, puedan ser usadas de la misma forma. Esto favorece mucho el polimorfismo y el diseño limpio de programas.

### Las interfaces se parecen a las clases abstractas, pero no son exactamente lo mismo. Una clase abstracta puede tener atributos, métodos con código y métodos abstractos, mientras que una interfaz, al principio del curso, se entiende como algo más restrictivo, centrado solo en definir comportamiento esperado. Además, una clase abstracta representa una relación de “es un”, mientras que una interfaz suele expresar “es capaz de” o “se comporta como”. Por ejemplo, una clase puede ser un Soldado y además implementar una interfaz como Atacable.

### Una diferencia muy importante es que una clase puede implementar más de una interfaz, mientras que solo puede heredar de una única clase (abstracta o no). Esto soluciona el problema de la herencia múltiple en Java y permite combinar comportamientos de forma segura. Por eso las interfaces se usan muchísimo en Java y aparecen constantemente en su API estándar, ya que permiten escribir código flexible sin depender de una jerarquía rígida de clases.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### En este ejercicio se usa el polimorfismo para trabajar con puntos sin saber de antemano si son 2D o 3D. Para eso se define una clase abstracta Punto con un método abstracto calcularDistanciaA, que obliga a que cada subtipo implemente su propia forma de calcular la distancia. La clave es que se puede usar siempre una referencia de tipo Punto, pero el comportamiento concreto depende del objeto real, que es justo la idea central del polimorfismo.

### Como tenemos dos posibles subtipos (Punto2D y Punto3D), dentro de cada implementación del método se usa instanceof para comprobar que el punto recibido es del mismo tipo, y luego se hace downcasting para acceder a sus coordenadas. Esto evita errores y garantiza que la distancia se calcula correctamente solo entre puntos compatibles. A nivel de primero, este ejemplo también sirve para entender cuándo y por qué usar instanceof con herencia.

### // Clase abstracta
### public abstract class Punto {
###    public abstract double calcularDistanciaA(Punto p);
### }

### // Punto en 2D
### public class Punto2D extends Punto {
###    private double x, y;

###    public Punto2D(double x, double y) {
###        this.x = x;
###        this.y = y;
###    }

###    @Override
###    public double calcularDistanciaA(Punto p) {
###        if (p instanceof Punto2D) {
###            Punto2D otro = (Punto2D) p;
###            return Math.sqrt(
###                Math.pow(this.x - otro.x, 2) +
###                Math.pow(this.y - otro.y, 2)
###            );
###        }
###        throw new IllegalArgumentException("Puntos incompatibles");
###    }
### }

### // Punto en 3D
### public class Punto3D extends Punto {
###    private double x, y, z;

###    public Punto3D(double x, double y, double z) {
###        this.x = x;
###        this.y = y;
###        this.z = z;
###    }

###    @Override
###    public double calcularDistanciaA(Punto p) {
###        if (p instanceof Punto3D) {
###            Punto3D otro = (Punto3D) p;
###            return Math.sqrt(
###                Math.pow(this.x - otro.x, 2) +
###                Math.pow(this.y - otro.y, 2) +
###                Math.pow(this.z - otro.z, 2)
###            );
###        }
###        throw new IllegalArgumentException("Puntos incompatibles");
###    }
### }

### Aprovechando este diseño, se puede crear una clase Linea que trabaje solo con referencias Punto, sin saber si son 2D o 3D. La línea delega el cálculo de la longitud a los propios puntos, lo que hace el diseño independiente del número de dimensiones y muy flexible.

### public class Linea {
###    private Punto a, b;

###    public Linea(Punto a, Punto b) {
###        this.a = a;
###        this.b = b;
###    }

###    public double longitud() {
###        return a.calcularDistanciaA(b);
###    }
### }


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### En Java, la herencia de interfaces consiste en que una interfaz puede extender a otra interfaz, heredando sus métodos abstractos. Esto permite construir interfaces más completas a partir de otras más simples, siguiendo un diseño por capas. A diferencia de las clases, sí existe herencia múltiple de interfaces, es decir, una interfaz puede extender varias interfaces a la vez, y una clase puede implementar varias interfaces sin problemas. Esto es una de las claves de diseño del lenguaje Java.

### En este sentido, las interfaces se usan para definir capacidades o comportamientos que pueden acumularse. Por ejemplo, una interfaz puede definir que algo es “leíble” y otra que es “escribible”, y una tercera puede combinar ambas. Esto no causa conflictos como pasaría con la herencia múltiple de clases, porque las interfaces no contienen estado ni implementación (al menos en el enfoque clásico que se enseña al principio).

### En el ejemplo, primero definimos una interfaz Fichero que obliga a leer el contenido como String. Después, definimos otra interfaz FicheroEscribible que extiende a Fichero, añadiendo nuevas operaciones como escribir contenido o eliminar el fichero. Así, cualquier clase que implemente FicheroEscribible estará obligada a implementar todos los métodos de ambas interfaces.

### // Interfaz básica
### public interface Fichero {
###    String leerContenido();
### }

### // Interfaz que hereda de Fichero
### public interface FicheroEscribible extends Fichero {
###    void escribirContenido(String contenido);
###    void eliminar();
### }