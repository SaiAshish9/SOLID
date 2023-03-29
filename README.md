```
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

```
