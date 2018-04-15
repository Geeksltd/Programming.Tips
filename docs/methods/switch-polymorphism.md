# Switch Statements vs Polymorphism

It’s also hard to make a switch statement that does one thing. By their nature, switch statements always do N things. Unfortunately, we can’t always avoid switch statements. Imagine the following example in an Employee class:

```c#
public decimal CalculatePay()
{     
     switch (this.EmployeeType)
     {       
              case COMMISSIONED:
                   return CalculateCommissionedPay();
              case HOURLY:
                  return CalculateHourlyPay();
              case SALARIED:
                  return CalculateSalariedPay();
              default:
                 throw new NotSupportedException();
    }
}
```

Basically it is saying that depending on the type of the employee, run a different logic. Now if in your application this was the only thing that depends on the employee type, this code is acceptable.

However, if you find that a switch statement on an employee’s type is being repeated, for example for:

- `IsPayday(Date date)`
- `DeliverPay(decimal pay)`
- ...

Then it’s a smell that something is missing. When you find yourself **repeating switch statements on a particular type of value**, you should change that to use polymorphism instead.

In the above example you will need to create a specialist employee class per employee type, that inherits from an abstract employee. The base Employee class will define the `CalculatePay()`, `IsPayday()`, and `DeliverPay()` methods as abstract (without implementation). And each of the child classes, ie `CommissionedEmployee`, `HourlyEmployee` and `SalariedEmployee` will implement it.

