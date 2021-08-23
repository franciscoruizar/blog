---
author: "Francisco Ruiz"
title: "Patrones de diseño en dos minutos: ¿Cuales son y cómo se organizan los patrones que analizaremos?"
date: "Aug 23, 2021 9:00 AM"
tags: ["dessign patterns"]
ShowToc: false
ShowBreadCrumbs: false
---

¡Hola a todxs! Hoy presentaremos en la serie de **Patrones de diseño en dos minutos de lectura** cuáles son y cómo se organizan los patrones de diseño que analizaremos?

## Clasificación

El catálogo de patrones de diseño que analizaremos serán los siguientes, a su vez veremos cómo se organizan y cuáles son las relaciones entre ellos

- Abstract Factory: Proporciona una interfaz para crear familias de objetos relacionados o que dependen entre sí, sin especificar sus clases concretas
- Adapter: Convierte la interfaz de una clase en otra distinta que es la que esperan los clientes. Permite que cooperen clases que de otra manera no podían por tener interfaces incompatibles
- Bridge: Desacopla una abstracción de su implementación, de manera que ambas puedan variar de forma independiente.
- Builder: Separa la construcción de un objeto complejo de su representación, de forma que el proceso de construcción pueda crear diferentes representaciones.
- Chain of responsability: Evita acoplar el emisor de una petición a su receptor, al dar a más de un objeto la posibilidad de responder a la petición. Crea una cadena con los objetos receptores y pasa la petición a través de la cadena hasta que ésta sea tratada por algún objeto
- Command & Query: Encapsula una petición en un objeto, permitiendo así parametrizar a los clientes con  distintas peticiones, encolar o llevar un registro de las peticiones y poder deshacer las operaciones.
- Composite: Combina objetos en estructuras de arbol para representar jerarquias de parte-todo. Permite que los clientes traten de manera uniforme a los objetos indivuales y a los compuestos.
- Decorator: Añade dinámicamente nuevas responsabilidades a un objeto, proporcionando una alternativa flexible a la herencia para extender la funcionalidad.
- Facade: Proporciona un interfaz unificada para un conjunto de interfaces de un subsistemas. Define una interfaz de alto nivel que hace que el subsistema sea mas facil de usar.
- Factory Method: Define una interfaz para crear un objeto, pero deja que sean las subclases quienes decidan qué clase instanciar. Permite que una clase delegue en sus subclases la creación de objetos.
- Flyweight: Sirve para eliminar o reducir la redundancia cuando tenemos gran cantidad de objetos que contienen información idéntica, además de lograr un equilibrio entre flexibilidad y rendimiento.
- Interpreter: Dado un lenguaje, define una representación de su gramática junto con un intérprete que usa dicha representación para interpretar sentencias del lenguaje.
- Iterator: Proporciona un modo de acceder secuencialmente a los elementos de un objeto agregado sin exponer su representación interna.
- Mediator: Define un objeto que encapsula cómo interactúan un conjunto de objetos. Promueve un bajo acoplamiento al evitar que los objetos se refieran unos a otros explícitamente, y permite variar la interacción entre ellos de forma independiente.
- Memento: Representa y externaliza el estado interno de un objeto  sin violar la encapsulación, deforma que éste puede volver a dicho estado más tarde.
- Observer: Define una dependencia de uno-a-muchos entre objetos, de forma que cuando un objeto cambie de estado se notifica y se actualiza automáticamente todos los objetos que dependen de él.
- Prototype: Especifica los tipos de objetos a crear por medio de una instancia prototipica, y crea nuevos objetos copiando de este prototipo.
- Proxy: Proporciona un sustituto o representante de otro objeto para controlar al acceso a éste.
- Singleton: Garantiza que una clase sólo tenga una instancia, y proporciona un punto de acceso global a ella.
- Sate: Permite que un objeto modifique su comportamiento cada vez que cambie su estado interno, Parecerá que cambia la clase del objeto
- Strategy: Define una familia de algoritmos, encapsula cada uno de ellos y los hace intercambiables. PErmiente que un algoritmovaríe independientemente de los clientes que lo usan.
- Template Method: Define en una operación el esqueleto de un algoritmo, delegando en las subclases algunos de sus pasos. Permite que las subclases redefinan  ciertos pasos del algoritmo sin cambiar sus estructura
- Visitor: Representa una operación sobre los elementos de una estructura de objetos. Permite definir una nueva operación sin cambiar las clases de los elementos sobre los que opera.

## Organización y relaciones

¿Cómo podemos clasificar los diferentes patrones listados? Los mismos varían en su granulidad y nivel de abstracción. Por un lado tenemos el **propósito**, reflejando qué hace un patrón. Gracias a ello podemos dividirlos en los patrones: 

- De creación: Responsabilidad en la creación de objetos;
- Estructurales: Tratan con la composición e implementación de clases u objetos;
- De comportamiento: Caracterizan el modo en que las clases y objetos interactuan y se reparten responsabilidades;

Por el otro lado, tenemos el criterio del **ámbito**, especificando si el patrón se aplica principalmente a clases o a objetos. Los patrones de clases se ocupan de la relaciones entre las clases y sus subclases . Estas relaciones se establecen a través de la herencia. Y los patrones de objeto de tratan con las relaciones entre objetos que pueden cambiarse en tiempo de ejecución y son más dinámicas.

```go
+-----------+--------+------------------+---------------+-------------------------+
|                                         Propósito                               |
+-----------+--------+------------------+---------------+-------------------------+
|                    | De creación      | Estructurales | De comportamiento       |
+-----------+--------+------------------+---------------+-------------------------+
|           |        | Factoy Method    | Adapter       | Interpreter             |
+           + Clase  +------------------+---------------+-------------------------+
|           |        |                  |               | Template Method         |
+           +--------+------------------+---------------+-------------------------+
|           |        | Abstract Factory | Adapter       | Chain of Responsability |
+           +        +------------------+---------------+-------------------------+
|           |        | Builder          | Bridge        | Command                 |
+           +        +------------------+---------------+-------------------------+
|           |        | Prototype        | Composite     | Iterator                |
+           +        +------------------+---------------+-------------------------+
|           |        | Singleton        | Decorator     | Mediator                |
+   Ámbito  + Objeto +------------------+---------------+-------------------------+
|           |        |                  | Facade        | Memento                 |
+           +        +------------------+---------------+-------------------------+
|           |        |                  | Flyweigth     | Observer                |
+           +        +------------------+---------------+-------------------------+
|           |        |                  | Proxy         | State                   |
+           +        +------------------+---------------+-------------------------+
|           |        |                  |               | Strategy                |
+           +        +------------------+---------------+-------------------------+
|           |        |                  |               | Visitor                 |
+-----------+--------+------------------+---------------+-------------------------+
```

![/dessign-pattern-in-two-minutes-3_1.jpeg](/dessign-pattern-in-two-minutes-3_1.jpeg)