# Composition Over Inheritance: The High-Performance Method for Building Resilient Software Solutions.

When it comes to designing software solutions, there are various techniques at our disposal. Two popular ones are inheritance and composition. While inheritance can help you to reuse code, it can also create dependencies that can lead to hard-to-maintain codebases. That's where composition comes in. In this article, we'll dive into the benefits of using composition over inheritance and how it can improve the loose coupling and high cohesion of your code.

What is Composition?

In composition, objects are created by combining simpler objects to build more complex ones. Rather than inheriting behavior from a parent class, a class uses other objects to perform a task. We can see this in action with the concept of a car. A car is composed of an engine, a transmission, wheels, and many other components that work together to make it function.

Here's an example of how composition might be used to represent a car in code:

```
class Engine {
  startEngine() {
    // code for starting the engine
  }

  stopEngine() {
    // code for stopping the engine
  }
}

class Transmission {
  shiftUp() {
    // code for shifting up
  }

  shiftDown() {
    // code for shifting down
  }
}

class Wheel {
  rotate() {
    // code for rotating the wheel
  }
}

class Car {
  constructor(engine, transmission, wheels) {
    this._engine = engine;
    this._transmission = transmission;
    this._wheels = wheels;
  }

  start() {
    this._engine.startEngine();
  }

  shiftUp() {
    this._transmission.shiftUp();
  }

  rotateWheels() {
    this._wheels.forEach(wheel => wheel.rotate());
  }
}
```

The `Car` class is composed of an `Engine`, a `Transmission`, and multiple `Wheel` objects. When the `start`, `shiftUp`, and `rotateWheels` methods are called, the corresponding methods of the composed objects are called to perform the desired actions.

Why Use Composition?

One key benefit of composition is that it promotes loose coupling between parts of a system. Loose coupling means that different parts of a system can operate independently of each other. With composition, a class can use an object without knowing the details of its implementation. This means that the class doesn't rely on outside classes to perform its tasks, which can make the code easier to understand and maintain.

Here's an example of how composition promotes loose coupling between two classes:

```
class Car {
  constructor(tire) {
    this._tire = tire;
  }

  drive() {
    this._tire.roll();
  }
}

class Tire {
  roll() {
    // code for rolling the tire
  }
}
```

In this example, the `Car` class uses a `Tire` object to perform its `drive` method. However, the `Car` class doesn't need to know the details of the `Tire` implementation. The `Tire` class could be swapped out for a different type of tire, and the `Car` class wouldn't need to be modified.

We can compare this to inheritance, where a subclass inherits all the behavior and properties of its parent class. This can lead to tight coupling, meaning that changes to a parent class can have a ripple effect on all its subclasses. This tight coupling can make it difficult to make changes or add new features to the codebase since any modifications to the parent class can break the child classes. In contrast, composition allows each class to be more independent, and modifications to one class are less likely to impact others.

Another benefit of composition is that it promotes high cohesion, which means that each class has a well-defined responsibility. In a system with well-defined responsibilities, each part of the system does something specific, which can make it easier to modify or remove parts of the system.

Inheritance, on the other hand, can lead to classes with multiple responsibilities since a subclass inherits behavior from its parent class. This can cause a lot of code repetition since functionality shared by many classes may be replicated in multiple places.

When to Use Inheritance

It's important to note that inheritance still has a place in programming. Inheritance is an effective way to reuse code when there is a need to model a hierarchy of parent-child relationships. For example, animal classes can be created with a superclass (Animal) and subclasses for each type of animal (Mammal, Reptile, Bird, etc.). Common behavior such as eating or moving can be defined in the superclass, while subclasses can add their own specific behavior, such as a bird's ability to fly.

Here's an example of how inheritance might be used to represent animals in code:

```
class Animal {
  eat() {
    // code for eating
  }

  move() {
    // code for moving
  }
}

class Bird extends Animal {
  fly() {
    // code for flying
  }
}

class Mammal extends Animal {
  sleep() {
    // code for sleeping
  }
}
```

In Conclusion

In software development, there are many techniques and strategies at our disposal to produce effective solutions. Composition and inheritance are two such techniques that we can use to implement code reuse. Composition offers several benefits over inheritance, including loose coupling and high cohesion, which can lead to more maintainable and understandable code. While each technique has its strengths and weaknesses, understanding when to use each one is critical to creating efficient and maintainable software solutions.
