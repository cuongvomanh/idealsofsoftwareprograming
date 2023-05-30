# Why Polymorphism is Often Better Than Conditionals in Software Development


Polymorphism and conditional statements are essential elements of software development. Both of them help us to write code that can handle different situations and solve complex problems. However, using polymorphism is often a better choice than conditionals, especially when we consider the principles of loose coupling and high cohesion.

## Loose Coupling and High Cohesion

In software development, the concept of loose coupling entails that software components should be developed in such a way that their dependencies are minimized, which makes the code more modular and less susceptible to bugs. On the other hand, high cohesion concerns how classes and methods are defined to ensure that they are focused on a single well-defined task. 

Now let's take a look at how polymorphism can help us achieve these goals.

## Polymorphism Encourages Loose Coupling

Polymorphism allows us to minimize dependencies between components by using abstraction. In an abstract class, we define methods without providing an implementation, which means that the implementation is left to subclasses to decide. This abstraction allows us to modify the implementation of a method in a subclass without modifying the code in the parent class.

Now let's take the example of the following code that relies on conditionals:

```
if(shape instanceof Rectangle) {
    Rectangle rectangle = (Rectangle) shape;
    rectangle.drawRectangle();
} else if(shape instanceof Circle) {
    Circle circle = (Circle) shape;
    circle.drawCircle();
} else if(shape instanceof Triangle) {
    Triangle triangle = (Triangle) shape;
    triangle.drawTriangle();
}
```

This implementation using conditional statements leads to tightly coupled code. It means that if we need to add a new shape to this code, we would have to modify the entire block of conditionals. However, we can fix this problem using polymorphism by defining an abstract method in the parent class:

```
abstract void draw();
```

Now each shape subclass can define its own implementation of this method. This provides us with a more loosely coupled codebase.

## Polymorphism Encourages High Cohesion

Polymorphism also encourages better high cohesion for our classes and methods. When we write code using polymorphism, we can create smaller, more focused classes, each with a well-defined responsibility. Referring to the same example we have discussed above, if we were not using polymorphism and we define the method "draw()" and its supporting functions in the parent class, this would violate the principle of high cohesion by spreading the responsibilities across multiple objects.

## Conclusion:

In conclusion, polymorphism is often a better choice than using conditional statements in programming. Using polymorphism, we can produce a more loosely coupled codebase with high cohesion. Not only that, but polymorphism allows us to add new behaviors more easily, resulting in a more Flexible and adaptable system. It is always worth considering polymorphic design patterns in your software development projects if you want to achieve maintainable, scalable, extendible, and readable code.
