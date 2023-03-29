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

Dependency Inversion Principle	

High-level modules should not depend on low-level modules, both should depend on abstractions.
```
