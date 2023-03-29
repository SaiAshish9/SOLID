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

```
