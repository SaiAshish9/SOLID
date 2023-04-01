https://www.educative.io/answers/what-are-the-solid-principles-in-java
https://www.youtube.com/watch?v=BM_lSZPMClo
https://github.com/Java-Techie-jt/solid-principles-example
https://www.geeksforgeeks.org/solid-principle-in-programming-understand-with-real-life-examples/

```
SRP
OCP
LSP
ISP
DIP

Single Responsibility

A class should have only one and only one reason to change.

Example 1:

public class Task {
   public void downloadFile(String location);
   public void parseTheFile(File file);
   public void persistTheData(Data data);
}

we can't reuse above code anywhere else and hence it doesn't 
meet single responsibility principle.

public class Employee {
   public Money calculatePay()
   public String reportHours()
   public void save()
}

This is also a class with multiple responsibilities .

Every class in Java should have a single job to do. 
To be precise, there should only be one reason to change 
a class. Here’s an example of a Java class that does not 
follow the single responsibility principle (SRP):

public class Vehicle {
    public void printDetails() {}
    public double calculateValue() {}
    public void addVehicleToDB() {}
}

The Vehicle class has three separate responsibilities: 
reporting, calculation, and database. By applying SRP, 
we can separate the above class into three classes with 
separate responsibilities.

Open/Closed

Software entities should be open for extension but closed for 
modification.

public double Area(object[] shapes){
   double area = 0;
   foreach(var shape in shapes){
      if(shape is Rectangle){
         Rectangle rectangle = (Rectangle) shape;
         area += rectangle.Width * rectangle.Height;
      }else{
         Circle circle = (Circle) shape;
         area += circle.Radius * circle.Radius * Math.PI;
      }
   }
   return area;
}

What if we need to add few more shapes? We need to modify all the code present.

public abstract class Shape 
{
   public abstract double Area();
}

it could also be a interface;

public class Rectangle : Shape 
{
   public double Width { get; set; };
   public double Height { get; set; };
   public override double Area() { return Width * Height; }
}

public class Circle : Shape 
{
   public double Radius { get; set; };
   public override double Area() { return Radius * Radius * Math.PI; }
}

public double Area(Shape[] shapes){
   double area = 0;
   foreach(var shape in shapes){
      area += shape.Area();
   }
   return area;
}

Area method only has the logic to loop around the shapes
and add the areas of them.

This Area method is closed for modification now, but we are open for 
extension because whenever we want to create a new shape we can add here.

Software entities (e.g., classes, modules, functions) should be open for an
extension, but closed for modification.

Consider the below method of the class VehicleCalculations:

public class VehicleCalculations {
    public double calculateValue(Vehicle v) {
        if (v instanceof Car) {
            return v.getValue() * 0.8;
        if (v instanceof Bike) {
            return v.getValue() * 0.5;
    }
}

Suppose we now want to add another subclass called Truck. We would have to modify the above class by adding another if statement, which goes against the Open-Closed Principle.
A better approach would be for the subclasses Car and Truck to override the calculateValue method:

public class Vehicle {
    public double calculateValue() {...}
}
public class Car extends Vehicle {
    public double calculateValue() {
        return this.getValue() * 0.8;
}
public class Truck extends Vehicle{
    public double calculateValue() {
        return this.getValue() * 0.9;
}

Adding another Vehicle type is as simple as making another subclass and extending from the Vehicle class.

3. Liskov substitution principle

The Liskov Substitution Principle (LSP) applies to inheritance hierarchies such that derived classes must be completely substitutable for their base classes.

Subtypes must be substitutable for their base types.

Example 1:

public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidth(double w) { width = w; }
    ...
}
public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}

The above classes do not obey LSP because you cannot replace the Rectangle base class with its derived class Square. The Square class has extra constraints, i.e., the height and width must be the same. Therefore, substituting Rectangle with Square class may result in unexpected behavior.

Example 2:

class Rectangle {
   void setWidth(double w)
   void setHeight(double h)
   double getWidth()
   double getHeight()
}

class Square {
   void setWidth(double w)
   void setHeight(double h)
   double getWidth()
   double getHeight()
}

LSP says its a bad implementation 

void test(Rectangle r)
{
  r.setWidth(5);
  r.setHeight(4);
  assertEquals(5 * 4, r.getWidth() * r.getHeight());
}

Square won't pass here as width and height will be the same.

Inheritance should only be used when super class is replacable by
sub class. It shouldn't be used just to save a few lines of code.

Interface Segregation Principle ISP

No client should be forced to depend on methods that it does not use.

The dependency of one class to another one should depend on the smallest
possible interface.

Clients should not be forced to implement interfaces they don't use.

Instead of one fat interface many small interfaces are preferred based on
groups of methods, each one serving one submodule.

The Interface Segregation Principle (ISP) states that clients should not be forced to depend upon interface members they do not use. In other words, do not force any client to implement an interface that is irrelevant to them.

Suppose there’s an interface for vehicle and a Bike class:

public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}
public class Bike implements Vehicle {

    // Can be implemented
    public void drive() {...}
    public void stop() {...}
    public void refuel() {...}
    
    // Can not be implemented
    public void openDoors() {...}
}

As you can see, it does not make sense for a Bike class to implement the openDoors() method as a bike does not have any doors! To fix this, ISP proposes that the interfaces be broken down into multiple, small cohesive interfaces so that no class is forced to implement any interface, and therefore methods, that it does not need.

Animal 
void feed(); // abstract
void groom();

Dog implements Animal 
void feed(); // implementation
void groom();

Tiger implements Animal
void feed(); // implementation 
void groom();

we are providing a dummy implementation just to keep the compiler happy.

Ideal way is to create another interface called Pet

Animal
void feed()

Pet extends Animal
void groom()

Dog implements Pet 
void feed(); // implementation
void groom();

Tiger implements Animal
void feed(); // implementation 

Dependency Inversion Principle	

Depend on abstractions (interfaces) rather than concrete classes.

High-level modules should not depend on low-level modules, both should depend on abstractions.

Example 1:

enum OutputDevice {printer, disk};

void copy(OutputDevice dev){
  int c;
  while((c = ReadKeyboard()) != EOF){
     if(dev == printer) writePrinter(c);
     else writeDisk(c);
  }
}

As the number of outputDevices increases copy method needs to keep 
changing

The better implementation is to create a simple interface called 
reader.

interface Reader
char read();

interface Writer
char write(char ch);

void copy(Reader r, Writer w){
   char ch;
   while((ch = r.read()) != EOF) w.write(ch);
}

copy method can read from any reader and write to any writer
it doesn't change when we've a new reader.

The Dependency Inversion Principle (DIP) states that we should depend on abstractions (interfaces and abstract classes) instead of concrete implementations (classes). The abstractions should not depend on details; instead, the details should depend on abstractions.

Consider the example below. We have a Car class that depends on the concrete Engine class; therefore, it is not obeying DIP.

public class Car {
    private Engine engine;
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class Engine {
   public void start() {...}
}

The code will work, for now, but what if we wanted to add another engine type, let’s say a diesel engine? This will require refactoring the Car class.
However, we can solve this by introducing a layer of abstraction. Instead of Car depending directly on Engine, let’s add an interface:

public interface Engine {
    public void start();
}

Now we can connect any type of Engine that implements the Engine interface to the Car class:

public class Car {
    private Engine engine;
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class PetrolEngine implements Engine {
   public void start() {...}
}
public class DieselEngine implements Engine {
   public void start() {...}
}

Projects
Skill Paths
Assessments
Create
Trusted answers to developer questions
What are the SOLID principles in Java?

Educative Answers Team
Free System Design Interview Course

Get Educative's definitive System Design Interview Handbook for free.

Get Free Course
SOLID principles are object-oriented design concepts relevant to software development. SOLID is an acronym for five other class-design principles: Single Responsibility Principle, Open-Closed Principle, Liskov Substitution Principle, Interface Segregation Principle, and Dependency Inversion Principle.

Principle	Description
Single Responsibility Principle	Each class should be responsible for a single part or functionality of the system.
Open-Closed Principle	Software components should be open for extension, but not for modification.
Liskov Substitution Principle	Objects of a superclass should be replaceable with objects of its subclasses without breaking the system.
Interface Segregation Principle	No client should be forced to depend on methods that it does not use.
Dependency Inversion Principle	High-level modules should not depend on low-level modules, both should depend on abstractions.
SOLID is a structured design approach that ensures your software is modular and easy to maintain, understand, debug, and refactor. Following SOLID also helps save time and effort in both development and maintenance. SOLID prevents your code from becoming rigid and fragile, which helps you build long-lasting software.

Examples
1. Single responsibility principle
Every class in Java should have a single job to do. To be precise, there should only be one reason to change a class. Here’s an example of a Java class that does not follow the single responsibility principle (SRP):

public class Vehicle {
    public void printDetails() {}
    public double calculateValue() {}
    public void addVehicleToDB() {}
}
The Vehicle class has three separate responsibilities: reporting, calculation, and database. By applying SRP, we can separate the above class into three classes with separate responsibilities.

2. Open-closed principle
Software entities (e.g., classes, modules, functions) should be open for an extension, but closed for modification.

Consider the below method of the class VehicleCalculations:

public class VehicleCalculations {
    public double calculateValue(Vehicle v) {
        if (v instanceof Car) {
            return v.getValue() * 0.8;
        if (v instanceof Bike) {
            return v.getValue() * 0.5;

    }
}
Suppose we now want to add another subclass called Truck. We would have to modify the above class by adding another if statement, which goes against the Open-Closed Principle.
A better approach would be for the subclasses Car and Truck to override the calculateValue method:

public class Vehicle {
    public double calculateValue() {...}
}
public class Car extends Vehicle {
    public double calculateValue() {
        return this.getValue() * 0.8;
}
public class Truck extends Vehicle{
    public double calculateValue() {
        return this.getValue() * 0.9;
}
Adding another Vehicle type is as simple as making another subclass and extending from the Vehicle class.

3. Liskov substitution principle
The Liskov Substitution Principle (LSP) applies to inheritance hierarchies such that derived classes must be completely substitutable for their base classes.

Consider a typical example of a Square derived class and Rectangle base class:

public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidht(double w) { width = w; }
    ...
}
public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}
The above classes do not obey LSP because you cannot replace the Rectangle base class with its derived class Square. The Square class has extra constraints, i.e., the height and width must be the same. Therefore, substituting Rectangle with Square class may result in unexpected behavior.

4. Interface segregation principle
The Interface Segregation Principle (ISP) states that clients should not be forced to depend upon interface members they do not use. In other words, do not force any client to implement an interface that is irrelevant to them.

Suppose there’s an interface for vehicle and a Bike class:

public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}
public class Bike implements Vehicle {

    // Can be implemented
    public void drive() {...}
    public void stop() {...}
    public void refuel() {...}
    
    // Can not be implemented
    public void openDoors() {...}
}
As you can see, it does not make sense for a Bike class to implement the openDoors() method as a bike does not have any doors! To fix this, ISP proposes that the interfaces be broken down into multiple, small cohesive interfaces so that no class is forced to implement any interface, and therefore methods, that it does not need.

5. Dependency inversion principle
The Dependency Inversion Principle (DIP) states that we should depend on abstractions (interfaces and abstract classes) instead of concrete implementations (classes). The abstractions should not depend on details; instead, the details should depend on abstractions.

Consider the example below. We have a Car class that depends on the concrete Engine class; therefore, it is not obeying DIP.

public class Car {
    private Engine engine;
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class Engine {
   public void start() {...}
}
The code will work, for now, but what if we wanted to add another engine type, let’s say a diesel engine? This will require refactoring the Car class.
However, we can solve this by introducing a layer of abstraction. Instead of Car depending directly on Engine, let’s add an interface:

public interface Engine {
    public void start();
}

Now we can connect any type of Engine that implements the Engine interface to the Car class:

public class Car {
    private Engine engine;
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}
public class PetrolEngine implements Engine {
   public void start() {...}
}
public class DieselEngine implements Engine {
   public void start() {...}
}
```
