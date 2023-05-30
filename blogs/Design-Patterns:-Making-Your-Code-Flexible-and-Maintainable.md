# Design Patterns: Making Your Code Flexible and Maintainable

Design Patterns are a set of established solutions to recurring design problems in software development. They provide developers with an easy-to-use technique for building software that is flexible, stable, and maintainable. When applied correctly, they help to establish loose coupling and high cohesion between components of your code, enabling you to build scalable and extensible systems that adapt to future needs. In this blog, we will explore a few of the most important design patterns that promote loose coupling and high cohesion, providing several code examples to illustrate each concept.

1. Observer Pattern

The Observer Pattern is a common solution used to notify a group of objects when a change has occurred in an object. This pattern allows dependent objects to be kept up-to-date with changes in a system, without tightly coupling the objects together. The Observer Pattern involves two key components: the Subject and the Observers. The Subject maintains a list of Observers, and when it changes, it notifies all the Observers. This pattern offers low coupling among objects, where objects communicate through shared interfaces, making the system more maintainable.

Here's an example code to illustrate how to implement the Observer Pattern using TypeScript:

```
interface Observer {
    update(): void
}

class Subject {
    private observers: Observer[] = []

    public subscribe(observer: Observer): void {
        this.observers.push(observer)
    }

    public unsubscribe(observer: Observer): void {
        const index = this.observers.indexOf(observer)
        if (index > -1) {
            this.observers.splice(index, 1)
        }
    }

    public notify(): void {
        for (const observer of this.observers) {
            observer.update()
        }
    }
}

class ConcreteObserver implements Observer {
    public update(): void {
        // do something when the subject state changes
    }
}

const subject = new Subject()
const observer = new ConcreteObserver()
subject.subscribe(observer)

// When the state of Subject changes, it notifies all registered observers:
subject.notify()
```

2. Factory Method Pattern

The Factory Method Pattern is a solution for creating objects without specifying the exact class of object that will be created. The Factory Method Pattern involves a Factory class that is responsible for creating new objects. The Factory Method Pattern offers low coupling by abstracting the creation of objects away from the rest of the code.

Here's an example code to illustrate how to implement the Factory Method Pattern using Java:

```
interface Product {
    void use();
}

class ConcreteProduct implements Product {
    @Override
    public void use() {
        System.out.println("Using Concrete Product...");
    }
}

class ProductFactory {
    public Product createProduct() {
        return new ConcreteProduct();
    }
}

public class Main {
    public static void main(String[] args) {
        ProductFactory factory = new ProductFactory();
        Product product = factory.createProduct();
        // Use the product
        product.use();
    }
}
```

3. Model-View-Controller Pattern (MVC)

The Model-View-Controller Pattern is a pattern for breaking down an application into three distinct parts: a model that stores data, a view that presents data to the user, and a controller that abstracts user actions to control the system. The MVC Pattern promotes low coupling between the View and the Model and high cohesion within each component.

Here's an example code to illustrate how to implement the MVC Pattern using Python:

```
class Model:
    def __init__(self):
        self._data = []

    def add_data(self, data):
        self._data.append(data)

    def get_data(self):
        return self._data

class View:
    def render(self, data):
        print(f"Data: {data}")

class Controller:
    def __init__(self, model, view):
        self._model = model
        self._view = view

    def add_data(self, data):
        self._model.add_data(data)
        self._view.render(self._model.get_data())

model = Model()
view = View()
controller = Controller(model, view)

controller.add_data("Data 1")
controller.add_data("Data 2")
controller.add_data("Data 3")
```


4. Dependency Injection (DI)

Dependency Injection (DI) is a pattern that promotes loose coupling by allowing objects to be independent of their dependencies. Instead of creating dependencies within a class, they are provided externally, typically through constructor parameters or setter methods. This pattern makes code more testable and reduces the risk of tightly coupling objects.

Here's an example code to illustrate how to implement the DI Pattern using Java:

```
public class MyApp {
    private final MyService myService;

    public MyApp(MyService myService) {
        this.myService = myService;
    }

    public void run() {
        myService.doSomething();
    }
}

public class MyService {
    public void doSomething() {
        // Business logic
    }
}

public class Main {
    public static void main(String[] args) {
        MyService myService = new MyService();
        MyApp myApp = new MyApp(myService);
        myApp.run();
    }
}
```

5. Strategy Pattern

The Strategy Pattern is a pattern that encapsulates interchangeable algorithms or behaviors into separate classes. It promotes high cohesion by ensuring each algorithm has a single responsibility, and clients can switch between algorithms at runtime without tightly coupling to a specific implementation. This pattern is commonly used in game development and machine learning.

Here's an example code to illustrate how to implement the Strategy Pattern using Python:

```
class Context:
    def __init__(self, strategy):
        self.strategy = strategy

    def execute_strategy(self, data):
        return self.strategy.execute(data)

class Strategy:
    def execute(self, data):
        pass

class ConcreteStrategy1(Strategy):
    def execute(self, data):
        # Use algorithm 1
        pass

class ConcreteStrategy2(Strategy):
    def execute(self, data):
        # Use algorithm 2
        pass

context = Context(ConcreteStrategy1())
result1 = context.execute_strategy(data)
context.strategy = ConcreteStrategy2()
result2 = context.execute_strategy(data)
```

Command Pattern

Command Pattern is a behavioral design pattern that allows decoupling between the sender and the receiver of a request. It separates the request for an action and the execution of the action. In other words, it abstracts the request for an action as an object, and this object holds all the information required to perform the action.

Here's a basic implementation of the Command Pattern in Java:

```java
interface Command {
    void execute();
}

class ConcreteCommand implements Command {

    private Receiver receiver;

    ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.action();
    }
}

class Receiver {
    void action() {
        ... // perform some action
    }
}

class Invoker {
    private Command command;

    void setCommand(Command command) {
        this.command = command;
    }

    void executeCommand() {
        command.execute();
    }
}
```

Loose coupling is achieved in the Command Pattern as the sender and receiver are decoupled from each other. The Command object acts as an intermediary, which helps in reducing the dependencies between the sender and receiver. High cohesion is achieved because each Command class has only one responsibility, that is, to execute a specific action.

Template Method Pattern

Template Method Pattern is a behavioral design pattern that defines the skeleton of an algorithm in a base class. It allows derived classes to provide specific implementations of some of the steps without changing the core algorithm's structure.

Here's a basic implementation of the Template Method Pattern in Java:

```java
abstract class AbstractClass {
    void templateMethod() {
        step1();
        step2();
        step3();
    }

    abstract void step2();

    void step1() {
        ... // do something
    }

    void step3() {
        ... // do something
    }
}

class ConcreteClass extends AbstractClass {

    void step2() {
        ... // provide specific implementation for step2
    }
}
```

Loose coupling is achieved in the Template Method Pattern as the abstract base class defines the algorithm, and the derived classes only implement specific steps. High cohesion is achieved because the algorithm's structure is preserved in the base class, and the individual steps are implemented in the derived classes.

Adapter Pattern

Adapter Pattern is a structural design pattern that allows incompatible interfaces to work together. It wraps an existing class with a new interface, and the adapter translates the client's requests to the format required by the wrapped class.

Here's a basic implementation of the Adapter Pattern in Java:

```java
interface Target {
    void request();
}

class Adaptee {
    void specificRequest() {
        ... // do something
    }
}

class Adapter implements Target {

    private Adaptee adaptee;

    Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void request() {
        adaptee.specificRequest();
    }
}

class Client {
    public static void main(String[] args) {

        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);

        target.request();
    }
}
```

Loose coupling is achieved in the Adapter Pattern as the client only communicates with the Target interface, and the Adapter shields the client from the complexity of the Adaptee interface. High cohesion is achieved because the Adapter's sole responsibility is to adapt the Adaptee interface to the Target interface.

Facade Pattern

Facade Pattern is a structural design pattern that provides a simple interface to a complex system. It encapsulates the complexity of a subsystem by providing a unified interface to expose only the relevant features.

Here's a basic implementation of the Facade Pattern in Java:

```java
class Subsystem1 {
    void method1() {
        ... // do something
    }
}

class Subsystem2 {
    void method2() {
        ... // do something
    }
}

class Facade {
    private Subsystem1 subsystem1;
    private Subsystem2 subsystem2;

    Facade(Subsystem1 subsystem1, Subsystem2 subsystem2) {
        this.subsystem1 = subsystem1;
        this.subsystem2 = subsystem2;
    }

    void doSomething() {
        subsystem1.method1();
        subsystem2.method2();
    }
}

class Client {
    public static void main(String[] args) {
        Subsystem1 subsystem1 = new Subsystem1();
        Subsystem2 subsystem2 = new Subsystem2();

        Facade facade = new Facade(subsystem1, subsystem2);
        facade.doSomething();
    }
}
```

Loose coupling is achieved in the Facade Pattern as the client interacts only with the Facade, and the Facade interacts with the subsystem. High cohesion is achieved because each subsystem has only one responsibility, and the Facade's responsibility is to provide a unified interface.

Composite Pattern

Composite Pattern is a structural design pattern that allows the creation of a tree-like structure of objects. It represents part-whole hierarchies of objects, and a single interface is used to interact with both leaf and composite objects.

Here's a basic implementation of the Composite Pattern in Java:

```java
interface Component {
    void operation();
}

class Leaf implements Component {
    void operation() {
        ... // do some operation
    }
}

class Composite implements Component {

    private List<Component> children;

    Composite(List<Component> children) {
        this.children = children;
    }

    void operation() {
        for (Component component : children) {
            component.operation();
        }
    }
}

class Client {
    public static void main(String[] args) {
        Component leaf1 = new Leaf();
        Component leaf2 = new Leaf();

        List<Component> children = new ArrayList<>();
        children.add(leaf1);
        children.add(leaf2);

        Component composite = new Composite(children);
        composite.operation();
    }
}
```

Loose coupling is achieved in the Composite Pattern as the client interacts with the Component interface, which is implemented by both leaf and composite objects. High cohesion is achieved because each Component implementation has only one responsibility, either to perform the operation or to act as a container for other Component objects.

Bridge Pattern

Bridge Pattern is a structural design pattern that decouples the abstraction from its implementation so that the two can vary independently. It helps in separating the high-level logic from the low-level implementation details.

Here's a basic implementation of the Bridge Pattern in Java:

```java
interface Implementor {
    void operationImpl();
}

class ConcreteImplementor implements Implementor {
    void operationImpl() {
        ... // do some low-level implementation
    }
}

abstract class Abstraction {

    protected Implementor implementor;

    Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    abstract void operation();
}

class RefinedAbstraction extends Abstraction {

    RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    void operation() {
        ... // do some high-level logic
        implementor.operationImpl();
        ... // do some more high-level logic
    }
}

class Client {
    public static void main(String[] args) {
        Implementor implementor = new ConcreteImplementor();
        Abstraction abstraction = new RefinedAbstraction(implementor);

        abstraction.operation();
    }
}
```

Loose coupling is achieved in the Bridge Pattern as the Abstraction and Implementor are decoupled, and each can vary independently. High cohesion is achieved because each class has only one responsibility, either to implement the low-level details or the high-level logic.


Conclusion

These are just a few examples of design patterns that align with the principles of loose coupling and high cohesion. Each pattern addresses different aspects of software design and can be used to achieve modular, maintainable, and flexible systems. By using these patterns and others available, developers can create robust and efficient software that delivers real value to users. When working with these patterns, keep in mind that proper implementation can be the difference between strong, maintainable code and difficult-to-update, inflexible code.

