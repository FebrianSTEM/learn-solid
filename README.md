# S.O.L.I.D. Principles
SOLID is a set of five object-oriented design principles that help in writing maintainable, scalable, and robust code. These principles guide developers in structuring software architecturally, improving code quality, and making the system easier to extend without modifying existing code.

- [Single Responsibility Principle](#SRP)
- [Open-Closed Principle](#ORP)
- [Liskov Substitution Principle](#LSP)
- [Interface Seggragation Principle](#ISP)
- [Dependency Inversion Principle](#DIP)

---


## Single Responsibility Principle (SRP)
The basic of single responsibility principle is to let class only have one responsibilitiy.

> **"Responsibilities should be limited to one per class."**

Examples: 

let' say we have class Employee such below, it consists of three method, with extra responsibility method named Save to save data employee.

```csharp

public class Employee
{
    // Own responsibility
    public int CalculateSalary()
    {
        return 1000;
    }

    // Own responsibility    
    public string GetDepartment()
    {
        return "IT";
    }
    
    // Extra responsibility
    public void Save()
    {
      // Save Employee to the database   
    }
}
```

By follow SRP we could refactor above by separate the Save method into new classes that will directly access to database.

```csharp
public class Employee
{
    public int CalculateSalary()
    {
        return 1000;
    }

    public string GetDepartment()
    {
        return "IT";
    }
}

public class EmployeeRepository
{
    public void Save()
    {
      // Save Employee to the database   
    }
}
```

When a class has only one responsibility, it become easier to change and test. if a class has multiple responsibilities, changing one may impact others and more testing effort will be required.

---

## <a name=OCP> Open-Closed Principle (OCP) </a>

> **"Software entities should be open for extension but closed for modification."**

The **Open-Closed Principle (OCP)** is one of the SOLID principles that states that a class should be **open for extension** but **closed for modification**. This means that we should be able to extend a class's functionality without modifying its existing code.

### Example

Let's consider a scenario where we need to calculate an employee's bonus. Initially, we might implement it as follows:

```csharp
public class Employee
{
    private string employeeType { get; set; }

    public double EmployeeBonus(string employeeType, double amount)
    {
        if (employeeType == "Permanent")
        {
            return amount * 1.2;
        }
        else
        {
            return amount * 1.0;
        }
    }
}
```

### Problem with This Approach

If a new requirement arises to introduce different bonus calculations for **interns** and **contract employees**, we would need to **modify** the `EmployeeBonus` method. This violates OCP because we are modifying existing code instead of extending it.

### Refactoring Using OCP and SRP

To follow OCP, we **refactor** the code by creating a **base class** with an abstract method `CalculateBonus`. Each employee type will have its own class that derives from the base class and implements its own bonus calculation.

```csharp
// Base class (Closed for modification, open for extension)
public abstract class Employee
{
    public abstract double CalculateBonus(double amount);
}

// Concrete implementations (extensions)
public class PermanentEmployee : Employee
{
    public override double CalculateBonus(double amount) => amount * 2;
}

public class ContractEmployee : Employee
{
    public override double CalculateBonus(double amount) => amount * 1.5;
}

public class Intern : Employee
{
    public override double CalculateBonus(double amount) => amount * 1.0;
}
```

---

# <a name=LSP>Liskov Substitution Principle (LSP)</a>

> **“Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program”**

Example : 

Please pay attention to code below, what if the requirement for Contract Employee is no bonus applicable for them. 

In ContractEmployee Class the CalculateBonus Method must be rewrite to override the base class method, so it would not return 100;

```csharp
//Base class
public class Employee
{
    public virtual int CalculateSalary()
    {
        return 1000;    
    }
    
    public virtual int CalculateBonus()
    {
        return 100;    
    }
}

//Derived Class
public class PermanentEmployee : Employee
{
    public override int CalculateSalary()
    {
        return 1300;
    }
}

//Derived Class
public class ContractEmployee : Employee
{
    public override int CalculateSalary()
    {
        return 800;
    }
    
    // Must be override
    public override int CalculateBonus()
    {
        throw new NotImplementedException();
    }
}
```

So how to resolved it ?

```csharp
// Base Class
public abstract class Employee
{
    public abstract int CalculateSalary();
}

// Interface for employees who receive bonuses
public interface IBonusEligible
{
    int CalculateBonus();
}

public class PermanentEmployee : Employee, IBonusEligible
{
    public override int CalculateSalary() => 1300;

    public int CalculateBonus() => 100;
}

public class ContractEmployee : Employee
{
    public override int CalculateSalary() => 800;
}
```

---

# <a name=ISP>Interface Seggragation Principle (ISP)</a>

> **“Clients should not be forced to depend on interfaces they do not use”**

---

# <a name=DIP>InterfaceDependency Inversion Principle (DIP)</a>


> **“High-level modules should not depend on low-level modules. Both should depend on abstractions”**

---
