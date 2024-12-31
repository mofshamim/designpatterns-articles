A class or module should have only one reason to change. This means that a class should have only one responsibility or job to do, and all its methods should be related to that job. This makes the code easier to understand, modify, and test.


**Benefits:**
- Code will be easier to understand
- Better maintainability
- Testability
- Parallel Development
- Reusability

**How to implement SRP**
Project Requirement:

- The application should be able to Add and Delete employee
- With every add and delete one email should be sent
- In case of exception, a text log file will be generated

Solution 1: Without SRP Principles
We can send an email and create a log of both Add & Delete methods separately. But if any issues arise we have to solve both methods which is not a good practice.

```csharp
public class Employee
    {
        public void Add()
        {
            try
            {
                Console.WriteLine("Add Employee");
                // Then write code for sending email
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
            }
        }

        public void Delete()
        {
            try
            {
                Console.WriteLine("Delete Employee");
                // Then write code for sending email
            }
            catch (Exception ex) 
            {
                Console.WriteLine(ex.ToString()); 
            }
        }
    }
```

**Solution 2:  Using SRP Principles**

- So we create Ilogger and IMailer interfaces and implement these interfaces with two classes. 
- Mailer class takes responsibility for sending email
- The logger class takes responsibility for creating the log

Instance create from employee constructor and then calling email-sender & logger method using those instance


```csharp
public interface IMailer
 {
    void SendEmail();
 }

 public class Mailer : IMailer
    {
        public void SendEmail()
        {
            //write code for sending email.
        }
    }
```

```csharp
 public interface ILogger
    {
        void LogException();
    }
    public class Logger : ILogger
    {
        public void LogException()
        {
            //write code for saving log exceptions.
        }
    }
```

```csharp
interface IEmployee
    {
        void Add();
        void Delete();
    }
    public class Employee: IEmployee
    {
        ILogger _logger;
        IMailer _mailer;
        public Employee()
        {
            _logger = new Logger();
            _mailer = new Mailer();
        }   
        public void Add()
        {
            try
            {
                Console.WriteLine("Add Employee");
                _mailer.SendEmail();
            }
            catch (Exception ex)
            {
                _logger.LogException();
            }
        }
        public void Delete()
        {
            try
            {
                Console.WriteLine("Add Employee");
                _mailer.SendEmail();
            }
            catch (Exception ex)
            {
                _logger.LogException();
            }
        }
    }
```

