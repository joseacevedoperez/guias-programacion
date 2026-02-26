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

### Las características principales son _encapsulamiento_, _abstracción_, _herencia_ y _polimorfismo_.
### El encapsulamiento oculta datos; la abstracción simplifica; la herencia permite reutilizar código; y el polimorfismo cambia el comportamiento según el objeto.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Lenguajes muy usados en POO son JavaScript,Python,PHP (Dinámicos) y C#, C++ y Java (Compilador, comprobación estática de tipos)


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Antes de la estructurada y la modular esta el _ensamblador_,que consiste en un listado de instrucciones y funciones de salto.
### La _programación estructurada_ se basa en secuencia, condicionales y bucles, a diferencia del ensamblador quita los saltos.
### La _modular_ divide el programa en partes pequeñas y reutilizables, acercándose al diseño por objetos, uitlizando librerias,paquetes y interfaces.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Un objeto está formado por _identidad_(estado), _estado_ (atributos,campos,datos) y _comportamiento_ (métodos,funciones).

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Una _clase_ es un molde que define el estado y el comportamiento(coche:marca...).Un _objeto o instancias_ es un ejemplo concreto de la clase.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Normalmente los objetos se guardan en el _heap_, aunque depende del lenguaje.La _recolección de basura_ (GC) borra automáticamente los objetos que ya no se usan.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Un _método_ es una acción de un objeto.La _sobrecarga de métodos_ permite varios métodos con el mismo nombre pero distintas firmas de parámetros.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### 
class Punto{
    int x;
    int y;
}
    //comportamiento(métodos)
    double calculaDistanciaAOrigen(){
        //distancia de p a (0,0)
        return sqrt(`x*x + y*y`);
    }

class Ejercicio1{
        public static void main(String[] args){
            Punto miPunto = new Punto();
            miPunto.x = 5;
            miPunto.y = 3;
            double resultado = miPunto.calculaDistanciaAOrigen();
        }
    }


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### _Main_ es el punto donde empieza un programa Java. _Static_ permite usarlo sin crear un objeto. Si se reemplaza por _final_, el programa no funciona porque final no sustituye a static.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Para ejecutar un programa básico en _Java_ desde la terminal, primero escribimos el archivo _.java_ y luego usamos el comando _javac_ para compilarlo. Este comando revisa el código y genera un archivo .class con _bytecode_, que es un formato intermedio entendido por la _JVM_. Ese archivo no es legible para humanos, pero sí para la máquina virtual.

### Después, para ejecutar el programa, usamos el comando java _NombreDeLaClase_. Este comando no compila nada, solo arranca la JVM y le dice que ejecute el bytecode del archivo _.class_. Todo este proceso separa la compilación de la ejecución, lo que hace que Java sea más controlado y fácil de portear entre sistemas.

### La _“máquina virtual”_ de Java (JVM) es un programa que interpreta el bytecode y lo adapta al sistema operativo donde se ejecuta. Gracias a ella, puedes programar en Windows y ejecutar el programa en Linux o Mac sin modificar nada. La idea es: compilas una vez, ejecutas en cualquier sitio.



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### La palabra clave new sirve para crear un nuevo objeto en Java. Cuando usamos _new_, la _JVM_ reserva memoria para el nuevo objeto y llama automáticamente al _constructor_ de la clase correspondiente. El constructor, por su parte, es un método especial que inicializa los atributos del objeto y se llama igual que la clase.

### El _constructor_ permite que el objeto nazca ya con valores válidos en sus atributos, evitando que estén vacíos o sin inicializar. Aunque se parece a un método normal, no devuelve nada y solo se ejecuta cuando se crea un objeto. Además, se pueden tener varios constructores con diferentes parámetros.

public Empleado(String dni, String nombre, String apellidos) {
    this.dni = dni;
    this.nombre = nombre;
    this.apellidos = apellidos;
}

Empleado e = new Empleado("12345678A", "Juan", "López");


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### La palabra _this_ hace referencia al objeto actual, es decir, al objeto sobre el que se está ejecutando el método. Sirve para diferenciar entre los atributos del objeto y los parámetros que se reciben dentro de un método, especialmente cuando tienen el mismo nombre.

### En muchos lenguajes orientados a objetos existe algo parecido a this, aunque puede llamarse distinto (por ejemplo, self en Python). Su propósito es siempre el mismo: referirse al propio objeto para acceder a sus atributos y métodos.

### Ejemplo en la clase Punto:

public Punto(double x, double y) {
    this.x = x;
    this.y = y;
}



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Para calcular la distancia entre dos puntos, podemos recibir otro objeto Punto como parámetro y usar this para referirnos al punto actual. Esto permite que el cálculo se haga usando los atributos de ambos objetos, sin necesidad de acceder directamente a sus valores desde fuera.

### Una implementación típica podría usar la fórmula de la distancia euclidiana. Desde dentro de la clase, se puede acceder a los atributos privados del otro punto, porque ambos objetos son del mismo tipo. Esto mantiene la encapsulación y evita exponer los atributos.

### Ejemplo:

public double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx*dx + dy*dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### En Java, cuando pasamos un objeto como _parámetro_, lo que se pasa es una _copia de la referencia_, no una copia del objeto. Esto significa que si dentro del método modificamos los atributos del objeto, esos cambios también afectan al objeto fuera del método. No se copia el objeto entero, solo la dirección donde está guardado.

### En cambio, cuando pasamos un tipo primitivo como un int, se pasa una _copia del valor_. Eso implica que si modificamos el parámetro dentro del método, el valor original fuera del método no cambia. Esto se debe a que los primitivos no se almacenan como objetos en el heap, sino como valores independientes.

### Por eso, si llamamos a un método que recibe un Punto y modificamos sus coordenadas, el punto original también cambia. Pero si recibimos un int y lo modificamos dentro de la función, no afecta al valor externo.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### El método _toString()_ es un método que devuelve una representación en texto del objeto. Cuando imprimimos un objeto con System.out.println(objeto), Java llama automáticamente a su toString(). Si no lo sobrescribimos, muestra una cadena genérica poco útil. Por eso es habitual sobrescribirlo.

### Este método existe en muchos otros lenguajes, aunque puede tener otro nombre. Por ejemplo, en Python sería __str__() y en C# también existe un método ToString(). Su función en todos los casos es facilitar la depuración y mostrar el objeto de forma legible.

### Ejemplo en Punto:

@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Un _struct_ en C se parece a una clase en que ambos agrupan varios datos bajo un mismo nombre. Es una forma de crear tipos compuestos que tienen varios campos, como coordenadas, direcciones, etc. En ese sentido, una clase podría verse como un “struct mejorado”.

### Sin embargo, un struct solo guarda datos y no tiene métodos, visibilidad, encapsulación ni herencia. Le falta todo lo que convierte a la POO en un paradigma más potente. En una _clase_, los datos pueden estar protegidos y puedes definir operaciones internas relacionadas con esos datos, mientras que en un struct todo es público.

### Por eso se dice que un _struct_ no llega a ser una clase: no permite ocultación de información ni comportamiento asociado directamente a los datos.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### En C no existen clases ni métodos asociados directamente a un tipo, así que para _“emular”_ una clase Punto tendríamos que crear un struct con los campos x e y, y luego escribir una función aparte que reciba un struct Punto como parámetro. Esa función haría el papel del método calcularDistanciaAlOrigen(), pero no estaría dentro del struct, sino fuera. En C todo es más manual, porque no hay encapsulación ni métodos internos.

### Por ejemplo, tendríamos algo así:

struct Punto {
    double x;
    double y;
};

double distanciaAlOrigen(struct Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}

### Como ves, la función tiene que recibir el struct porque no existe un mecanismo automático como los métodos de Java. Es el programador quien decide qué funciones operan sobre qué tipos.

### En cuanto a _this_, simplemente desaparece, porque en C no existe el concepto de “objeto actual”. En Java, this apunta al objeto que está ejecutando el método. En C, como no hay métodos dentro del struct, tampoco hay un “objeto actual”, así que el equivalente es pasar el struct como argumento. En otras palabras, el parámetro p en la función cumple el papel que tendría this en Java.
