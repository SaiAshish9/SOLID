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


Design principles are guidelines which helps us improve the quality of our application. They are preferred solutions for commonly occurring problems which when not dealt properly leads to a bad design.

We will be discussing on some of the design principles for Object oriented programming which every developer should be aware of. These Design principles go beyond the core object oriented concepts like Inheritance, Encapsulation, Abstraction and Polymorphism.

In this article i’ll be discussing some of the foundational design principle which I had used in my web development journey to solve some common problems.

Some of these Design Principles are,
“Encapsulate what varies”
“Favor composition”
“Program to interfaces”
“Loose coupling”
SOLID principles
Design Patterns vs Design Principles
Design Principles are general guidelines that can guide your class structure and relationships. On the other hand, Design Patterns are proven solutions that solve commonly reoccurring problems. Having said that, most of the practical implementations of these design principles are mostly done using one or more design patterns.

1. “Encapsulate what varies”
Considered as the foundational design principles, this principle is found at work in almost all design patterns.

This principle suggests, Identify the aspects of your applications that vary and separate them from what stays the same. If a component or module in your application is bound to change frequently, then it’s a good practice to separate this part of code from the stable ones so that later we can extend or alter the part that varies without affecting those that don’t vary.

Most design patterns like Strategy, Adapter, Facade, Decorator, Observer, etc follow this principle.

For example, Assume we are designing an app for a company that provides online service of household equipments. In our applications core we have this method processServiceRequest(), the purpose of which is to create instance of a OnlineService class based on the serviceType and process the request. This is a heavy duty method as shown below

public void processServiceRequest(String serviceType) {
     OnlineService service;
     if(service.equals("AirConditioner"))
       service = new ACService();
     else if(service.equals("WashingMachine"))
       service = new WMService();
     else if(service.equals("Refrigerator"))
       service = new RFService();
     else
       service = new GeneralService();
     service.getinfo();
     service.assignServiceRequest();
     service.assignAgent();
     service.processRequest();
}
Here, the type of service is a functionality which is bound to change at anytime. We might remove some services or add new and every such change in implementations would require to change this piece of code.

So, by the guidelines of “Encapsulate what varies” we need to find the code which is bound to vary and separate it so that any error in the same would not effect the important piece of code.

We can remove the code that creates instances and create a class which would work as a factory class that is only there to provide required type of instances. We are going to follow the Factory design pattern here and we will be having only one method getOnlineService() in this class which would do our job

public class OnlineServiceFactory {
   public OnlineService getOnlineService(String type) {
       OnlineService service;
       if(service.equals("AirConditioner"))
           service = new ACService();
       else if(service.equals("WashingMachine"))
           service = new WMService();
       else if(service.equals("Refrigerator"))
           service = new RFService();
       else
           service = new GeneralService();
       return service;
   }
}
now we can refactor our previous code as,

public void processServiceRequest(String serviceType) {
       OnlineService service = new OnlineServiceFactory().getOnlineService(serviceType);
       service.getinfo();
       service.assignServiceRequest();
       service.assignAgent();
       service.processRequest();
}
Now any changes with the service types wont affect the rest of the code.

2. “Favor Composition over Inheritance”
Object oriented programming provides 2 types of relationships between classes and its instances. has-a (composition) and is-a (inheritance). This design principle essentially suggests us that “has-a relationship should be preferred over is-a relationship”.

Most developers including me tend to lean over inheritance as their first resort in most cases to avoid code duplication and maintain reusability. Though a good practice, sometimes Inheritance when overused makes our code more rigid and not extensible.

Let’s take the example from the previous case, we have used is-a relationship to manage the OnlineService class with inherited service implementation classes like ACService, WMService, RFService, etc. Consider that the company decides to expand their business massively by adding more services like BikeService, CarService, TVService, LaptopService and many other household services. The above implementation would still hold good with inheritance model. But the complicated part here is that now they have planned to include multiple service models together like say a request for having multiple kitchen appliances serviced along with AC. This would lead to creating new classes for separate combination like ACAndFridgeService, ACAndTVServices. Which is not feasible and is literally a bad design. And more the combination more vulnerable this design would be to break.

Hence, we would want to go with a has-a relationship implementation. Where in each and every service in itself is a single class and based on the request we could combine those together and build our ultimate request.

We could use Builder design pattern here and return a compound object with each service requested by the user.


3. “Loose Coupling”
Loose coupling is a principle which suggests that “components interacting with each others should be independent of each others, relying on the knowledge of other component as little as possible”.

This is one principle we follow in a microservice architecture. The services interacting with each others are independent of each others. The interaction is strictly based on the data contracts.

Implementation of loose coupling would vary from scenario to scenario based on the problem we are trying to solve. But for our example let’s take a simple real time scenario.

Imagine we are designing a frontend application for a non monolithic architecture. We have a form for sign up which would get the user input, perform client side validations, sends request to the server via rest apis, get the response and based on the response it decides to show success or failure messages to the user.


Here, we can split the functionality into two based on its ownership. Ideally, anything specific to the client side should be decoupled from the server side operations. That is, the chunk of code responsible for form display, client side validations and displaying messages to the user should have nothing to do with the part of code which makes api requests, processes the response and return status.


4. “Program to interfaces, Not Implementations”
This design principle guides us to make use of abstract types not concrete ones. “Program to interfaces” actually mean program to a super type like an interface or abstract class in java. We are implementing polymorphism by programming to a super type so that the actual runtime object isn’t locked into your code.

Assume a database accessor layer in your application which is used to perform CRUD operations on your DB. Let’s consider that we implement a Service class which calls the DatabaseClient class (However practically we should have a DataAccessor class between Service and DatabaseClient). The DatabaseClient is concrete class programmed to access postgres DB. The DatabaseClient is a heavy duty class with all helper methods required to access the DB. Assume that the client decides to switch to a NoSQL database like MongoDB or add it as a secondary database for some specific purposes. This would lead to rewriting the DatabaseClient which would complicate things.

Solution? As this principle states, any such modules should have an abstract super type like an interface. The basic methods should be made available in the interface. Specific implementations should be implement the interface.


ServiceClass service = new ServiceClass();
if(dbType == "POSTGRES") {
    service.db = new PostgresDBClient();
} else if(dbType == "MYSQL") {
    service.db = new MySQLDBClient();
}
5. SOLID Principles
SOLID is a set of 5 principles which forms the acronym.

a.) Single responsibility principle:

A class should have only one reason to change. This principle suggests that the responsibilities of a class should be limited. It’s not a clear guideline because we don’t have a specific instruction on which basis to judge this. However, the basic idea when designing using this principle should be to check if the methods inside a class are cohesive i.e. do they really need to be together?

For example, a class which is required to create a rectangle which gets the area and draw the same on the screen.


This implementation violates the Single responsibility principle as calculating the area and drawing are separate actions and can be decoupled.

b.) Open closed principle:

This principle states that our design should be open for extension but closed for modifications. But what if we want to add a new implementation in our design? We can approach this in 2 ways.

We extend the existing functionality to a new class and add the implementations there
Use composition to accept new behaviours
Imagine that we have a class for auditioning singers.

public class Singers {
   String name;
   int age;
   String ratings;
   String typeOfSinging;
}
However, we decide to increase the scope of the contest to dancing as well, then we cannot have this concrete implementation. What we could do is, according to this principle, have our implementation abstracted and extended ass below.

public abstract class Contestants {
   String name;
   int age;
   String ratings;
}
public class Singer implements Contestants {
   String name;
   int age;
   String ratings;
   String typeOfMusic;
   public void sing() {
     ...
   }
}
public class Dancer implements Contestants {
   String name;
   int age;
   String ratings;
   String typeOfDance;
   public void dance() {
     ...
   }
}
By introducing inheritance, we solve the above complexity using Open closed principle.

c.) Liskov’s substitution principle:

Sometimes inheritance can break the system when we substitute our derived class with the parent class. Liskov’s principle essentially suggests that, “You should always be able to substitute subtypes for their base class.”

For example, If we have a ChildClass that extends a ParentClass, then our system should not break when our parent type instance is substituted with child class.

ClassA classA = new ClassA();
classA.doSomething();  // should work fine
ClassA = new ClassB();
classA.doSomething();  // should work fine
d.) Interface segregation principle:

This principle suggests that an “interface should always be cohesive”. That is, the components of the interface should be highly relatable. Any component that is not related to each other must be separated and segregated.

e.) Dependency inversion principle:

The Dependency inversion principle is used to decouple software modules. It states that “the high level modules should not depend on the low level modules. Instead both should depend on abstraction”.

For example, consider a Notifier class which sends SMS notification from SMSMessenger class as below.


Now assume that we want to add 2 more notification modes email and whatsapp. Since the Notifier class is tightly coupled to the SMSMessenger class, we cannot implement new messengers without changing the existing code.

How do we solve this? As this principle states, we need to decouple the dependency of the low level module SMSMessenger over high level module Notifier and have both modules depend on an abstraction. In this case, on abstraction of Messenger types.

We can do that as below.


Conclusion:
The design principles which covered in this article are kind of the fundamental and foundational one’s. There are few more such principles like DRY, AHA, KISS which I wanted to cover but I don’t feel could be explained in more specific way since they are mostly situational and varies based on the problems we are trying to solve.

DRY stands for Don’t Repeat Yourself which states that “Every piece of knowledge must have a single, unambiguous, authoritative representation within a system”. In other words it mean that every module must be unique in itself.
AHA stands for Avoid Hasty Abstractions which basically suggests us that sometimes abstractions can lead to a rigid codebase. And when it does, its better to avoid it.
KISS stands for Keep It Simple Stupid which as the acronym suggests that our implementation should be as simple as possible. Anything introducing complications in the implementation should be decoupled using any of the above principles.
Source:
https://en.wikipedia.org/wiki/SOLID
https://www.linkedin.com/learning/advanced-design-patterns-design-principles
```
