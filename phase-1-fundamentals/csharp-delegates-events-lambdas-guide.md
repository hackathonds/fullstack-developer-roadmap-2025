# 📘 Complete Guide to Delegates, Events, Lambdas & Expression-Bodied Members in C#
## Extended Edition with Advanced Scenarios, Real-Time Projects & Case Studies

---

## 📑 Table of Contents

1. [Delegates](#1-delegates)
2. [Events](#2-events)
3. [Lambda Expressions](#3-lambda-expressions)
4. [Expression-Bodied Members](#4-expression-bodied-members)
5. [How They Work Together](#5-how-they-work-together)
6. [Interview Questions & Answers](#6-interview-questions--answers)
7. [Advanced Scenarios](#7-advanced-scenarios)
8. [Real-Time Projects with Solutions](#8-real-time-projects-with-solutions)
9. [Case Studies](#9-case-studies)
10. [Best Practices & Common Pitfalls](#10-best-practices--common-pitfalls)
11. [Quick Reference Cheat Sheet](#11-quick-reference-cheat-sheet)

---

## 1.  DELEGATES

### 📌 What is a Delegate? 

A **delegate** is a type-safe function pointer that holds a reference to a method.  It defines the signature (return type and parameters) of methods it can reference.

> **Simple Analogy:** Think of a delegate as a "job description" - any employee (method) that matches that description can be hired (assigned) for the job.

### 📌 Basic Syntax

```csharp
// Step 1: Declare a delegate type
public delegate int MathOperation(int a, int b);

// Step 2: Create methods that match the signature
public static int Add(int x, int y) => x + y;
public static int Subtract(int x, int y) => x - y;

// Step 3: Instantiate and use the delegate
MathOperation operation = Add;
int result = operation(10, 5);  // result = 15

// Change the method reference
operation = Subtract;
result = operation(10, 5);  // result = 5
```

### 📌 Types of Delegates

#### 1. Single-Cast Delegate
Holds reference to a single method. 

```csharp
public delegate void PrintDelegate(string message);

PrintDelegate print = Console.WriteLine;
print("Hello World");
```

#### 2. Multi-Cast Delegate
Holds references to multiple methods (using `+` or `+=`).

```csharp
public delegate void NotifyDelegate(string message);

void SendEmail(string msg) => Console.WriteLine($"Email: {msg}");
void SendSMS(string msg) => Console. WriteLine($"SMS: {msg}");
void LogToFile(string msg) => Console.WriteLine($"Log: {msg}");

// Combine delegates
NotifyDelegate notify = SendEmail;
notify += SendSMS;
notify += LogToFile;

notify("Order Placed!");
// Output:
// Email: Order Placed!
// SMS: Order Placed!
// Log: Order Placed! 

// Remove a method
notify -= SendSMS;
```

#### 3. Built-in Generic Delegates

C# provides pre-defined generic delegates so you don't need to create custom ones:

| Delegate | Description | Syntax |
|----------|-------------|--------|
| `Action` | No return value (void) | `Action<T1, T2, ...>` |
| `Func` | Has return value | `Func<T1, T2, .. ., TResult>` |
| `Predicate` | Returns bool | `Predicate<T>` |

```csharp
// Action - void return type
Action<string> greet = name => Console.WriteLine($"Hello, {name}!");
greet("Alice");

// Func - returns a value (last type parameter is return type)
Func<int, int, int> add = (a, b) => a + b;
int sum = add(5, 3);  // 8

Func<string, int> getLength = s => s.Length;
int len = getLength("Hello");  // 5

// Predicate - returns bool
Predicate<int> isEven = n => n % 2 == 0;
bool result = isEven(4);  // true

// Using Predicate with List
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
List<int> evenNumbers = numbers.FindAll(isEven);  // [2, 4, 6]
```

### 📌 Delegate Invocation Methods

```csharp
public delegate int Calculator(int a, int b);
Calculator calc = (a, b) => a + b;

// Method 1: Direct invocation
int result1 = calc(5, 3);

// Method 2: Invoke method
int result2 = calc.Invoke(5, 3);

// Method 3: Null-safe invocation (C# 6+)
int?  result3 = calc?. Invoke(5, 3);

// Method 4: DynamicInvoke (slower, uses reflection)
object result4 = calc.DynamicInvoke(5, 3);
```

### 📌 Covariance and Contravariance in Delegates

```csharp
// Covariance (out) - return types
public delegate Animal AnimalFactory();

Dog CreateDog() => new Dog();  // Dog inherits Animal
AnimalFactory factory = CreateDog;  // ✅ Covariance allows this

// Contravariance (in) - parameter types
public delegate void AnimalHandler(Dog dog);

void HandleAnimal(Animal animal) => Console. WriteLine(animal.Name);
AnimalHandler handler = HandleAnimal;  // ✅ Contravariance allows this
```

### 📌 Real-World Use Case: Callback Pattern

```csharp
public class FileDownloader
{
    public void DownloadFile(string url, Action<bool, string> onComplete)
    {
        try
        {
            // Simulate download
            Thread.Sleep(2000);
            onComplete(true, "Download completed successfully!");
        }
        catch (Exception ex)
        {
            onComplete(false, ex.Message);
        }
    }
}

// Usage
var downloader = new FileDownloader();
downloader.DownloadFile("http://example.com/file.zip", (success, message) =>
{
    if (success)
        Console.WriteLine($"✅ {message}");
    else
        Console. WriteLine($"❌ Error: {message}");
});
```

---

## 2. EVENTS

### 📌 What is an Event? 

An **event** is a mechanism for communication between objects.  It's built on top of delegates but adds encapsulation - subscribers can only add/remove handlers, not invoke the event directly.

> **Simple Analogy:** An event is like a newsletter subscription.  You can subscribe or unsubscribe, but only the publisher can send the newsletter. 

### 📌 Event vs Delegate

| Feature | Delegate | Event |
|---------|----------|-------|
| External Invocation | ✅ Yes | ❌ No (only owner class) |
| External Assignment | ✅ Yes (= operator) | ❌ No (only += or -=) |
| Encapsulation | ❌ Less secure | ✅ More secure |
| Purpose | Method reference | Notification mechanism |

### 📌 Basic Event Pattern

```csharp
// Step 1: Define event arguments (optional, for passing data)
public class OrderEventArgs : EventArgs
{
    public string OrderId { get; set; }
    public decimal Amount { get; set; }
    public DateTime OrderDate { get; set; }
}

// Step 2: Publisher class with event
public class OrderProcessor
{
    // Declare the event using EventHandler<T>
    public event EventHandler<OrderEventArgs> OrderPlaced;
    
    // Method to raise the event (protected virtual for inheritance)
    protected virtual void OnOrderPlaced(OrderEventArgs e)
    {
        OrderPlaced?.Invoke(this, e);
    }
    
    public void PlaceOrder(string orderId, decimal amount)
    {
        // Business logic here... 
        Console.WriteLine($"Processing order {orderId}.. .");
        
        // Raise the event
        OnOrderPlaced(new OrderEventArgs
        {
            OrderId = orderId,
            Amount = amount,
            OrderDate = DateTime.Now
        });
    }
}

// Step 3: Subscriber classes
public class EmailService
{
    public void OnOrderPlaced(object sender, OrderEventArgs e)
    {
        Console.WriteLine($"📧 Sending email for Order {e.OrderId} - ${e.Amount}");
    }
}

public class InventoryService
{
    public void OnOrderPlaced(object sender, OrderEventArgs e)
    {
        Console.WriteLine($"📦 Updating inventory for Order {e.OrderId}");
    }
}

public class AnalyticsService
{
    public void OnOrderPlaced(object sender, OrderEventArgs e)
    {
        Console.WriteLine($"📊 Logging analytics for Order {e. OrderId}");
    }
}

// Step 4: Wire up and use
class Program
{
    static void Main()
    {
        var processor = new OrderProcessor();
        var emailService = new EmailService();
        var inventoryService = new InventoryService();
        var analyticsService = new AnalyticsService();
        
        // Subscribe to the event
        processor. OrderPlaced += emailService.OnOrderPlaced;
        processor.OrderPlaced += inventoryService.OnOrderPlaced;
        processor.OrderPlaced += analyticsService.OnOrderPlaced;
        
        // Place an order
        processor.PlaceOrder("ORD-001", 99.99m);
        
        // Output:
        // Processing order ORD-001...
        // 📧 Sending email for Order ORD-001 - $99.99
        // 📦 Updating inventory for Order ORD-001
        // 📊 Logging analytics for Order ORD-001
        
        // Unsubscribe
        processor.OrderPlaced -= analyticsService.OnOrderPlaced;
    }
}
```

### 📌 Custom Event Accessors

```csharp
public class SecurePublisher
{
    private EventHandler<EventArgs> _myEvent;
    private readonly object _lock = new object();
    
    public event EventHandler<EventArgs> MyEvent
    {
        add
        {
            lock (_lock)
            {
                _myEvent += value;
                Console.WriteLine("Handler added");
            }
        }
        remove
        {
            lock (_lock)
            {
                _myEvent -= value;
                Console. WriteLine("Handler removed");
            }
        }
    }
    
    protected virtual void OnMyEvent()
    {
        _myEvent?. Invoke(this, EventArgs.Empty);
    }
}
```

### 📌 Standard . NET Event Pattern

```csharp
// 1. EventArgs class (use EventArgs. Empty for no data)
public class StockPriceChangedEventArgs : EventArgs
{
    public string Symbol { get; }
    public decimal OldPrice { get; }
    public decimal NewPrice { get; }
    public decimal ChangePercent => ((NewPrice - OldPrice) / OldPrice) * 100;
    
    public StockPriceChangedEventArgs(string symbol, decimal oldPrice, decimal newPrice)
    {
        Symbol = symbol;
        OldPrice = oldPrice;
        NewPrice = newPrice;
    }
}

// 2. Publisher
public class StockTicker
{
    private Dictionary<string, decimal> _prices = new();
    
    public event EventHandler<StockPriceChangedEventArgs> PriceChanged;
    
    protected virtual void OnPriceChanged(StockPriceChangedEventArgs e)
    {
        // Thread-safe invocation
        var handler = PriceChanged;
        handler?.Invoke(this, e);
    }
    
    public void UpdatePrice(string symbol, decimal newPrice)
    {
        decimal oldPrice = _prices.GetValueOrDefault(symbol, 0);
        _prices[symbol] = newPrice;
        
        if (oldPrice != newPrice)
        {
            OnPriceChanged(new StockPriceChangedEventArgs(symbol, oldPrice, newPrice));
        }
    }
}

// 3.  Usage
var ticker = new StockTicker();

ticker.PriceChanged += (sender, e) =>
{
    Console.WriteLine($"{e.Symbol}: ${e.OldPrice} → ${e.NewPrice} ({e.ChangePercent:+0.00;-0.00}%)");
};

ticker.UpdatePrice("AAPL", 150.00m);
ticker.UpdatePrice("AAPL", 155.50m);
// Output: AAPL: $150. 00 → $155.50 (+3.67%)
```

### 📌 Weak Event Pattern (Preventing Memory Leaks)

```csharp
// Problem: Strong reference prevents garbage collection
public class Publisher
{
    public event EventHandler MyEvent;
}

public class Subscriber
{
    public Subscriber(Publisher pub)
    {
        pub.MyEvent += HandleEvent;  // Creates strong reference
    }
    
    void HandleEvent(object sender, EventArgs e) { }
    
    // If Subscriber should be garbage collected but Publisher lives longer,
    // the Subscriber won't be collected due to the event subscription! 
}

// Solution: Always unsubscribe when done
public class ProperSubscriber : IDisposable
{
    private Publisher _publisher;
    
    public ProperSubscriber(Publisher pub)
    {
        _publisher = pub;
        _publisher.MyEvent += HandleEvent;
    }
    
    void HandleEvent(object sender, EventArgs e) { }
    
    public void Dispose()
    {
        if (_publisher != null)
        {
            _publisher.MyEvent -= HandleEvent;
            _publisher = null;
        }
    }
}
```

---

## 3.  LAMBDA EXPRESSIONS

### 📌 What is a Lambda Expression? 

A **lambda expression** is an anonymous function (a function without a name) that you can use to create delegates or expression trees. It uses the `=>` operator (called "goes to" or "arrow").

> **Simple Analogy:** A lambda is like a sticky note with quick instructions - you don't need to create a formal document (named method) for simple tasks.

### 📌 Lambda Syntax Evolution

```csharp
// Traditional named method
bool IsEven(int n)
{
    return n % 2 == 0;
}

// Anonymous method (C# 2.0)
Func<int, bool> isEven1 = delegate(int n)
{
    return n % 2 == 0;
};

// Lambda expression (C# 3.0)
Func<int, bool> isEven2 = (int n) => { return n % 2 == 0; };

// Lambda with type inference
Func<int, bool> isEven3 = (n) => { return n % 2 == 0; };

// Lambda with single parameter (no parentheses needed)
Func<int, bool> isEven4 = n => { return n % 2 == 0; };

// Expression lambda (single expression, implicit return)
Func<int, bool> isEven5 = n => n % 2 == 0;
```

### 📌 Types of Lambdas

#### 1. Expression Lambda
Single expression, implicit return:
```csharp
Func<int, int> square = x => x * x;
Func<int, int, int> add = (a, b) => a + b;
Action<string> print = msg => Console.WriteLine(msg);
```

#### 2. Statement Lambda
Multiple statements, explicit return:
```csharp
Func<int, int, int> divide = (a, b) =>
{
    if (b == 0)
        throw new DivideByZeroException();
    return a / b;
};

Action<string> process = msg =>
{
    Console.WriteLine($"Processing: {msg}");
    Console.WriteLine("Done!");
};
```

### 📌 Lambda with LINQ

```csharp
List<Person> people = new List<Person>
{
    new Person { Name = "Alice", Age = 30, City = "NYC" },
    new Person { Name = "Bob", Age = 25, City = "LA" },
    new Person { Name = "Charlie", Age = 35, City = "NYC" },
    new Person { Name = "Diana", Age = 28, City = "Chicago" }
};

// Where - filtering
var adults = people.Where(p => p.Age >= 30);

// Select - projection
var names = people.Select(p => p.Name);

// OrderBy / OrderByDescending
var orderedByAge = people.OrderBy(p => p.Age);
var orderedByAgeDesc = people.OrderByDescending(p => p.Age);

// First / FirstOrDefault
var firstNYC = people.FirstOrDefault(p => p.City == "NYC");

// Any / All
bool hasMinors = people.Any(p => p.Age < 18);
bool allAdults = people. All(p => p.Age >= 18);

// Count
int nycCount = people.Count(p => p. City == "NYC");

// GroupBy
var byCity = people.GroupBy(p => p.City);

// Complex query
var result = people
    . Where(p => p.Age >= 25)
    .OrderBy(p => p.Name)
    .Select(p => new { p.Name, p.Age })
    .ToList();
```

### 📌 Captured Variables (Closures)

Lambdas can "capture" variables from the enclosing scope:

```csharp
// Basic closure
int multiplier = 3;
Func<int, int> multiply = x => x * multiplier;
Console.WriteLine(multiply(5));  // 15

multiplier = 5;
Console.WriteLine(multiply(5));  // 25 (uses current value!)

// ⚠️ Common pitfall with loops
var actions = new List<Action>();
for (int i = 0; i < 3; i++)
{
    actions.Add(() => Console.WriteLine(i));
}
foreach (var action in actions)
    action();  // Prints: 3, 3, 3 (not 0, 1, 2!)

// ✅ Fix: capture the value
var actionsFixed = new List<Action>();
for (int i = 0; i < 3; i++)
{
    int captured = i;  // Capture current value
    actionsFixed.Add(() => Console.WriteLine(captured));
}
foreach (var action in actionsFixed)
    action();  // Prints: 0, 1, 2
```

### 📌 Static Lambdas (C# 9+)

Prevent accidental capture of variables:

```csharp
int factor = 10;

// Regular lambda - can capture
Func<int, int> regular = x => x * factor;  // ✅ OK

// Static lambda - cannot capture
// Func<int, int> staticLambda = static x => x * factor;  // ❌ Error! 

Func<int, int> staticLambda = static x => x * 2;  // ✅ OK
```

### 📌 Lambda Attributes (C# 10+)

```csharp
// Adding attributes to lambda parameters and return values
var parse = [return: MaybeNull] (string s) => int.TryParse(s, out var n) ? n : null;

var validate = ([NotNull] string input) => ! string.IsNullOrEmpty(input);
```

### 📌 Natural Type for Lambdas (C# 10+)

```csharp
// Before C# 10 - must specify delegate type
Func<int, int> square1 = x => x * x;

// C# 10+ - var can infer delegate type
var square2 = (int x) => x * x;  // Inferred as Func<int, int>

// With explicit return type
var parse = (string s) => int.Parse(s);  // Func<string, int>
var parseExplicit = int (string s) => int.Parse(s);  // Explicit return type
```

---

## 4.  EXPRESSION-BODIED MEMBERS

### 📌 What are Expression-Bodied Members?

Expression-bodied members provide a concise syntax for members that contain only a single expression.  They use the `=>` operator. 

### 📌 Supported Members

```csharp
public class Person
{
    private string _firstName;
    private string _lastName;
    private DateTime _birthDate;
    
    // 1.  Expression-bodied constructor (C# 7.0+)
    public Person(string firstName, string lastName) 
        => (_firstName, _lastName) = (firstName, lastName);
    
    // 2. Expression-bodied finalizer/destructor (C# 7.0+)
    ~Person() => Console.WriteLine($"Finalizing {FullName}");
    
    // 3. Expression-bodied read-only property (C# 6.0+)
    public string FullName => $"{_firstName} {_lastName}";
    
    // 4. Expression-bodied property with get and set (C# 7.0+)
    public string FirstName
    {
        get => _firstName;
        set => _firstName = value ??  throw new ArgumentNullException(nameof(value));
    }
    
    // 5. Expression-bodied method (C# 6.0+)
    public int GetAge() => DateTime.Now.Year - _birthDate.Year;
    
    // 6. Expression-bodied indexer (C# 7.0+)
    private string[] _nicknames = new string[10];
    public string this[int index]
    {
        get => _nicknames[index];
        set => _nicknames[index] = value;
    }
    
    // 7. Expression-bodied operator (C# 6.0+)
    public static Person operator +(Person a, Person b) 
        => new Person(a._firstName, b._lastName);
    
    // 8. Expression-bodied conversion operator
    public static implicit operator string(Person p) => p.FullName;
}
```

### 📌 When to Use vs Traditional Syntax

```csharp
// ✅ Good use - single, clear expression
public double Area => Math.PI * Radius * Radius;

public string GetGreeting(string name) => $"Hello, {name}!";

public bool IsValid => !string.IsNullOrEmpty(Name) && Age > 0;

// ❌ Avoid - complex logic (use traditional syntax)
public string GetStatus()  // Better as block body
{
    if (IsActive)
    {
        if (HasPermission)
            return "Active with permission";
        return "Active without permission";
    }
    return "Inactive";
}

// ❌ Avoid - side effects in properties
public int Count => _items.Count;  // ✅ OK - no side effects
public int Count => ComputeAndLog();  // ⚠️ Avoid - has side effects
```

### 📌 Expression-Bodied vs Auto-Properties

```csharp
public class Example
{
    // Auto-property - has backing field, can be set
    public string Name { get; set; }
    
    // Expression-bodied property - computed, read-only
    public int NameLength => Name?.Length ?? 0;
    
    // Expression-bodied property - always recalculated
    public DateTime CurrentTime => DateTime.Now;  // Different value each access! 
    
    // Auto-property - stored once
    public DateTime CreatedAt { get; } = DateTime.Now;  // Same value always
}
```

---

## 5.  HOW THEY WORK TOGETHER

### 📌 Complete Real-World Example

```csharp
using System;
using System.Collections.Generic;
using System. Linq;

// Event Args
public class TaskCompletedEventArgs : EventArgs
{
    public string TaskName { get; }
    public TimeSpan Duration { get; }
    public bool Success { get; }
    
    public TaskCompletedEventArgs(string name, TimeSpan duration, bool success)
        => (TaskName, Duration, Success) = (name, duration, success);
}

// Task Processor with Events
public class TaskProcessor
{
    private readonly List<Func<string, bool>> _validators = new();
    
    // Event declaration
    public event EventHandler<TaskCompletedEventArgs> TaskCompleted;
    
    // Expression-bodied event raiser
    protected virtual void OnTaskCompleted(TaskCompletedEventArgs e) 
        => TaskCompleted?.Invoke(this, e);
    
    // Add validator using Func delegate
    public void AddValidator(Func<string, bool> validator) 
        => _validators.Add(validator);
    
    // Process with callback (Action delegate)
    public void ProcessTask(string taskName, Action<string> onProcess)
    {
        var startTime = DateTime.Now;
        
        // Validate using all validators (lambdas)
        bool isValid = _validators.All(v => v(taskName));
        
        if (isValid)
        {
            onProcess(taskName);
        }
        
        // Raise event
        OnTaskCompleted(new TaskCompletedEventArgs(
            taskName, 
            DateTime.Now - startTime, 
            isValid));
    }
}

// Subscriber with expression-bodied members
public class TaskLogger
{
    private readonly List<string> _logs = new();
    
    // Expression-bodied property
    public int LogCount => _logs. Count;
    
    // Expression-bodied indexer
    public string this[int index] => _logs[index];
    
    // Event handler
    public void OnTaskCompleted(object sender, TaskCompletedEventArgs e)
    {
        var status = e.Success ?  "✅" : "❌";
        var log = $"{status} {e.TaskName} - {e.Duration. TotalMilliseconds}ms";
        _logs. Add(log);
        Console.WriteLine(log);
    }
}

// Usage
class Program
{
    static void Main()
    {
        var processor = new TaskProcessor();
        var logger = new TaskLogger();
        
        // Subscribe to event
        processor.TaskCompleted += logger. OnTaskCompleted;
        
        // Add validators using lambdas
        processor.AddValidator(name => ! string.IsNullOrEmpty(name));
        processor.AddValidator(name => name.Length <= 50);
        processor.AddValidator(name => ! name.Contains("invalid"));
        
        // Process with lambda callback
        processor. ProcessTask("ImportData", task => 
        {
            Console.WriteLine($"Processing: {task}");
            Thread.Sleep(100);  // Simulate work
        });
        
        processor.ProcessTask("invalid_task", task => 
            Console.WriteLine($"Processing: {task}"));
        
        // Check logs
        Console. WriteLine($"\nTotal logs: {logger. LogCount}");
    }
}
```

### 📌 Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        C# DELEGATES ECOSYSTEM                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   DELEGATE (Foundation)                                              │
│   ├── Type-safe method pointer                                       │
│   ├── Can hold single or multiple methods                            │
│   └── Built-in: Action, Func, Predicate                              │
│            │                                                         │
│            ▼                                                         │
│   EVENT (Built on Delegates)                                         │
│   ├── Publisher-Subscriber pattern                                   │
│   ├── Encapsulation (can't invoke externally)                        │
│   └── Uses EventHandler<T> delegate                                  │
│            │                                                         │
│            ▼                                                         │
│   LAMBDA (Syntactic Sugar for Delegates)                             │
│   ├── Anonymous inline functions                                     │
│   ├── Expression & Statement forms                                   │
│   └── Powers LINQ queries                                            │
│            │                                                         │
│            ▼                                                         │
│   EXPRESSION-BODIED MEMBERS                                          │
│   ├── Concise syntax for single expressions                          │
│   ├── Methods, Properties, Constructors, etc.                        │
│   └── Uses same => arrow operator                                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 6.  INTERVIEW QUESTIONS & ANSWERS

### 🔰 BEGINNER LEVEL

---

**Q1: What is a delegate in C#? **

**Answer:** A delegate is a type-safe function pointer that holds a reference to one or more methods with a specific signature. It defines the return type and parameter types of methods it can reference.

```csharp
public delegate int Calculator(int a, int b);
Calculator add = (x, y) => x + y;
int result = add(5, 3);  // 8
```

---

**Q2: What is the difference between Action and Func delegates?**

**Answer:**
- `Action` - Returns void, can have 0-16 input parameters
- `Func` - Must return a value, last type parameter is the return type

```csharp
Action<string> log = msg => Console. WriteLine(msg);  // void
Func<int, int, int> add = (a, b) => a + b;  // returns int
```

---

**Q3: What is a lambda expression?**

**Answer:** A lambda expression is an anonymous function that uses the `=>` operator. It provides a concise way to represent a method inline.

```csharp
// Syntax: (parameters) => expression or { statements }
Func<int, bool> isEven = n => n % 2 == 0;
```

---

**Q4: What is the difference between an event and a delegate?**

**Answer:**
| Aspect | Delegate | Event |
|--------|----------|-------|
| External Invocation | ✅ Allowed | ❌ Not allowed |
| Assignment | = operator allowed | Only += or -= |
| Purpose | Method reference | Notification pattern |

---

**Q5: What are expression-bodied members?**

**Answer:** Expression-bodied members use `=>` to define members with a single expression, making code more concise. 

```csharp
public string FullName => $"{FirstName} {LastName}";
public int GetAge() => DateTime. Now.Year - BirthYear;
```

---

### 🔷 INTERMEDIATE LEVEL

---

**Q6: What is a multicast delegate?  How does it work?**

**Answer:** A multicast delegate holds references to multiple methods.  When invoked, all methods are called in order.  Use `+=` to add and `-=` to remove methods.

```csharp
Action<string> notify = null;
notify += msg => Console.WriteLine($"Email: {msg}");
notify += msg => Console.WriteLine($"SMS: {msg}");
notify("Hello!");
// Both methods execute
```

**Important:** For multicast delegates with return values, only the last method's return value is captured.

---

**Q7: What are closures in C#?  What is variable capture?**

**Answer:** A closure occurs when a lambda captures variables from its enclosing scope. The lambda holds a reference to the variable, not a copy of its value.

```csharp
int multiplier = 5;
Func<int, int> multiply = x => x * multiplier;
Console.WriteLine(multiply(3));  // 15

multiplier = 10;
Console.WriteLine(multiply(3));  // 30 (uses current value!)
```

---

**Q8: Explain the standard event pattern in . NET.**

**Answer:**
1. Create `EventArgs` subclass for event data
2. Declare event using `EventHandler<TEventArgs>`
3. Create protected virtual `OnEventName` method
4.  Subscribers use `+=` to subscribe

```csharp
public class OrderEventArgs : EventArgs
{
    public string OrderId { get; set; }
}

public class OrderService
{
    public event EventHandler<OrderEventArgs> OrderPlaced;
    
    protected virtual void OnOrderPlaced(OrderEventArgs e)
        => OrderPlaced?. Invoke(this, e);
}
```

---

**Q9: What is the difference between Predicate<T> and Func<T, bool>?**

**Answer:** Functionally identical - both take T and return bool. `Predicate<T>` is older (used in List methods), `Func<T, bool>` is more modern and flexible.

```csharp
Predicate<int> pred = n => n > 0;
Func<int, bool> func = n => n > 0;

var list = new List<int> { -1, 0, 1, 2 };
list.FindAll(pred);  // Predicate
list. Where(func);    // Func (LINQ)
```

---

**Q10: How do you prevent memory leaks with events?**

**Answer:** Always unsubscribe from events when the subscriber is no longer needed.  Implement `IDisposable` for proper cleanup.

```csharp
public class Subscriber : IDisposable
{
    private Publisher _publisher;
    
    public Subscriber(Publisher pub)
    {
        _publisher = pub;
        _publisher.DataReceived += OnDataReceived;
    }
    
    public void Dispose()
    {
        if (_publisher != null)
        {
            _publisher.DataReceived -= OnDataReceived;
            _publisher = null;
        }
    }
}
```

---

### 🔶 ADVANCED LEVEL

---

**Q11: Explain covariance and contravariance in delegates.**

**Answer:**
- **Covariance** (return types): A method can return a more derived type
- **Contravariance** (parameters): A method can accept a less derived type

```csharp
// Covariance - Dog is Animal
delegate Animal AnimalFactory();
Dog CreateDog() => new Dog();
AnimalFactory factory = CreateDog;  // ✅ Allowed

// Contravariance
delegate void DogHandler(Dog dog);
void HandleAnimal(Animal a) => Console.WriteLine(a);
DogHandler handler = HandleAnimal;  // ✅ Allowed
```

---

**Q12: What is the difference between Invoke() and DynamicInvoke()?**

**Answer:**
| Method | Performance | Type Safety | Use Case |
|--------|-------------|-------------|----------|
| `Invoke()` | Fast (compile-time) | ✅ Type-safe | Normal usage |
| `DynamicInvoke()` | Slow (reflection) | ❌ Runtime errors | Dynamic scenarios |

```csharp
Func<int, int, int> add = (a, b) => a + b;

int r1 = add. Invoke(5, 3);  // Fast, type-safe
object r2 = add.DynamicInvoke(5, 3);  // Slow, uses reflection
```

---

**Q13: How do static lambdas (C# 9+) work and why use them?**

**Answer:** Static lambdas prevent accidental variable capture, improving performance by avoiding closure allocation. 

```csharp
int factor = 10;

// Regular - creates closure, heap allocation
Func<int, int> regular = x => x * factor;

// Static - no capture allowed, stack allocation
Func<int, int> staticLambda = static x => x * 2;

// This would cause compile error:
// Func<int, int> error = static x => x * factor; // ❌
```

---

**Q14: Explain expression trees and how they differ from delegates.**

**Answer:**
- **Delegate**: Compiled IL code that executes directly
- **Expression Tree**: Data structure representing code, can be analyzed/modified at runtime

```csharp
// Delegate - compiled code
Func<int, bool> funcDelegate = n => n > 5;

// Expression tree - inspectable data structure
Expression<Func<int, bool>> exprTree = n => n > 5;

// Can analyze the expression tree
BinaryExpression body = (BinaryExpression)exprTree.Body;
ParameterExpression param = (ParameterExpression)body.Left;
ConstantExpression constant = (ConstantExpression)body. Right;
// Used by LINQ providers (EF Core) to translate to SQL
```

---

**Q15: How would you implement a custom event accessor?**

**Answer:** Custom accessors give you control over subscription behavior (logging, thread safety, etc.). 

```csharp
public class ThreadSafePublisher
{
    private EventHandler _myEvent;
    private readonly object _lock = new object();
    
    public event EventHandler MyEvent
    {
        add
        {
            lock (_lock)
            {
                _myEvent += value;
                Console.WriteLine($"Subscriber added.  Count: {_myEvent?. GetInvocationList().Length}");
            }
        }
        remove
        {
            lock (_lock)
            {
                _myEvent -= value;
            }
        }
    }
}
```

---

**Q16: What are the performance implications of using closures?**

**Answer:** Closures cause heap allocations because captured variables are moved to a compiler-generated class. 

```csharp
// No closure - stack allocated
Func<int, int> noClosure = static x => x * 2;

// With closure - heap allocated
int factor = 5;
Func<int, int> withClosure = x => x * factor;

// Compiler generates approximately:
class DisplayClass
{
    public int factor;
    public int Method(int x) => x * factor;
}
```

**Optimization:** Use `static` lambdas when possible, or pass values as parameters. 

---

**Q17: How do you handle exceptions in multicast delegates?**

**Answer:** By default, if one handler throws, subsequent handlers don't execute. Handle manually:

```csharp
public void SafeInvoke(EventHandler handler, object sender, EventArgs e)
{
    if (handler == null) return;
    
    foreach (var d in handler.GetInvocationList())
    {
        try
        {
            ((EventHandler)d)(sender, e);
        }
        catch (Exception ex)
        {
            Console. WriteLine($"Handler failed: {ex.Message}");
            // Log, continue to next handler
        }
    }
}
```

---

### 🔴 EXPERT/SENIOR LEVEL

---

**Q18: Design a custom event aggregator/message bus using delegates.**

**Answer:**

```csharp
public interface IEventAggregator
{
    void Subscribe<TEvent>(Action<TEvent> handler);
    void Unsubscribe<TEvent>(Action<TEvent> handler);
    void Publish<TEvent>(TEvent eventData);
}

public class EventAggregator : IEventAggregator
{
    private readonly Dictionary<Type, List<Delegate>> _handlers = new();
    private readonly object _lock = new object();
    
    public void Subscribe<TEvent>(Action<TEvent> handler)
    {
        lock (_lock)
        {
            var type = typeof(TEvent);
            if (!_handlers.ContainsKey(type))
                _handlers[type] = new List<Delegate>();
            _handlers[type].Add(handler);
        }
    }
    
    public void Unsubscribe<TEvent>(Action<TEvent> handler)
    {
        lock (_lock)
        {
            var type = typeof(TEvent);
            if (_handlers.ContainsKey(type))
                _handlers[type]. Remove(handler);
        }
    }
    
    public void Publish<TEvent>(TEvent eventData)
    {
        List<Delegate> handlers;
        lock (_lock)
        {
            var type = typeof(TEvent);
            if (! _handlers.ContainsKey(type)) return;
            handlers = _handlers[type].ToList();  // Copy for thread safety
        }
        
        foreach (var handler in handlers)
        {
            try
            {
                ((Action<TEvent>)handler)(eventData);
            }
            catch (Exception ex)
            {
                // Log exception, continue
            }
        }
    }
}

// Usage
var aggregator = new EventAggregator();
aggregator.Subscribe<OrderPlaced>(e => Console.WriteLine($"Order: {e. OrderId}"));
aggregator. Publish(new OrderPlaced { OrderId = "123" });
```

---

**Q19: How would you implement async events? **

**Answer:**

```csharp
// Async event handler delegate
public delegate Task AsyncEventHandler<TEventArgs>(object sender, TEventArgs e);

public class AsyncPublisher
{
    public event AsyncEventHandler<DataEventArgs> DataReceivedAsync;
    
    protected virtual async Task OnDataReceivedAsync(DataEventArgs e)
    {
        var handler = DataReceivedAsync;
        if (handler == null) return;
        
        // Get all handlers
        var handlers = handler.GetInvocationList()
            .Cast<AsyncEventHandler<DataEventArgs>>();
        
        // Execute all handlers concurrently
        await Task.WhenAll(handlers.Select(h => h(this, e)));
        
        // Or sequentially:
        // foreach (var h in handlers)
        //     await h(this, e);
    }
    
    public async Task ProcessDataAsync()
    {
        // Process... 
        await OnDataReceivedAsync(new DataEventArgs { Data = "result" });
    }
}

// Usage
publisher.DataReceivedAsync += async (sender, e) =>
{
    await Task.Delay(100);
    Console. WriteLine($"Processed: {e.Data}");
};
```

---

**Q20: Explain how LINQ providers use Expression<TDelegate> to translate code.**

**Answer:** Expression trees allow LINQ providers to analyze code structure and translate it to other languages (SQL, etc.).

```csharp
// This is a delegate - executes as . NET code
Func<User, bool> funcFilter = u => u.Age > 18;

// This is an expression tree - data structure representing the code
Expression<Func<User, bool>> exprFilter = u => u.Age > 18;

// Entity Framework can analyze the expression tree:
dbContext.Users.Where(exprFilter);  // Translated to: WHERE Age > 18

// The expression tree contains:
// - BinaryExpression (>)
//   - MemberAccess (u. Age)
//   - Constant (18)

// Custom translation example:
public string TranslateToSql(Expression<Func<User, bool>> expr)
{
    var body = (BinaryExpression)expr.Body;
    var left = ((MemberExpression)body.Left).Member. Name;
    var right = ((ConstantExpression)body.Right).Value;
    var op = body.NodeType == ExpressionType. GreaterThan ?  ">" : "=";
    
    return $"WHERE {left} {op} {right}";
}
```

---

## 7. ADVANCED SCENARIOS

### 🚀 Scenario 1: Generic Pipeline Pattern with Delegates

Build a flexible data processing pipeline using delegates:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace PipelinePattern
{
    /// <summary>
    /// Generic pipeline that processes data through multiple stages
    /// </summary>
    public class Pipeline<TInput, TOutput>
    {
        private readonly List<Func<object, object>> _stages = new();
        
        private Pipeline() { }
        
        public static Pipeline<TInput, TInput> Create() => new Pipeline<TInput, TInput>();
        
        /// <summary>
        /// Add a transformation stage to the pipeline
        /// </summary>
        public Pipeline<TInput, TNewOutput> AddStage<TNewOutput>(Func<TOutput, TNewOutput> transform)
        {
            var newPipeline = new Pipeline<TInput, TNewOutput>();
            newPipeline._stages.AddRange(_stages);
            newPipeline._stages.Add(input => transform((TOutput)input));
            return newPipeline;
        }
        
        /// <summary>
        /// Add a conditional stage that only executes if predicate is true
        /// </summary>
        public Pipeline<TInput, TOutput> AddConditionalStage(
            Predicate<TOutput> condition, 
            Func<TOutput, TOutput> transform)
        {
            var newPipeline = new Pipeline<TInput, TOutput>();
            newPipeline._stages. AddRange(_stages);
            newPipeline._stages.Add(input =>
            {
                var typed = (TOutput)input;
                return condition(typed) ? transform(typed) : typed;
            });
            return newPipeline;
        }
        
        /// <summary>
        /// Add a validation stage that throws if validation fails
        /// </summary>
        public Pipeline<TInput, TOutput> AddValidation(
            Predicate<TOutput> validator, 
            string errorMessage)
        {
            var newPipeline = new Pipeline<TInput, TOutput>();
            newPipeline._stages.AddRange(_stages);
            newPipeline._stages.Add(input =>
            {
                var typed = (TOutput)input;
                if (!validator(typed))
                    throw new ValidationException(errorMessage);
                return typed;
            });
            return newPipeline;
        }
        
        /// <summary>
        /// Execute the pipeline with given input
        /// </summary>
        public TOutput Execute(TInput input)
        {
            object current = input;
            foreach (var stage in _stages)
            {
                current = stage(current);
            }
            return (TOutput)current;
        }
        
        /// <summary>
        /// Execute pipeline with error handling
        /// </summary>
        public PipelineResult<TOutput> ExecuteSafe(TInput input)
        {
            try
            {
                return PipelineResult<TOutput>.Success(Execute(input));
            }
            catch (Exception ex)
            {
                return PipelineResult<TOutput>. Failure(ex. Message);
            }
        }
    }
    
    public class PipelineResult<T>
    {
        public bool IsSuccess { get; }
        public T Value { get; }
        public string Error { get; }
        
        private PipelineResult(bool success, T value, string error)
        {
            IsSuccess = success;
            Value = value;
            Error = error;
        }
        
        public static PipelineResult<T> Success(T value) => new(true, value, null);
        public static PipelineResult<T> Failure(string error) => new(false, default, error);
    }
    
    public class ValidationException : Exception
    {
        public ValidationException(string message) : base(message) { }
    }
    
    // ==================== USAGE EXAMPLE ====================
    
    public class Order
    {
        public string Id { get; set; }
        public decimal Amount { get; set; }
        public string CustomerEmail { get; set; }
        public decimal Discount { get; set; }
        public decimal Tax { get; set; }
        public decimal FinalAmount { get; set; }
        public bool IsProcessed { get; set; }
    }
    
    public class OrderProcessor
    {
        private readonly Pipeline<Order, Order> _pipeline;
        
        public OrderProcessor()
        {
            _pipeline = Pipeline<Order, Order>.Create()
                // Validation stage
                .AddValidation(
                    o => o.Amount > 0,
                    "Order amount must be positive")
                . AddValidation(
                    o => ! string.IsNullOrEmpty(o. CustomerEmail),
                    "Customer email is required")
                // Apply discount for orders over $100
                . AddConditionalStage(
                    o => o.Amount > 100,
                    o => { o.Discount = o.Amount * 0.1m; return o; })
                // Calculate tax
                .AddStage(o =>
                {
                    o.Tax = (o.Amount - o.Discount) * 0.08m;
                    return o;
                })
                // Calculate final amount
                . AddStage(o =>
                {
                    o.FinalAmount = o. Amount - o. Discount + o.Tax;
                    return o;
                })
                // Mark as processed
                . AddStage(o =>
                {
                    o.IsProcessed = true;
                    return o;
                });
        }
        
        public PipelineResult<Order> ProcessOrder(Order order) 
            => _pipeline.ExecuteSafe(order);
    }
    
    // Test
    class Program
    {
        static void Main()
        {
            var processor = new OrderProcessor();
            
            // Valid order over $100
            var order1 = new Order 
            { 
                Id = "ORD-001", 
                Amount = 150m, 
                CustomerEmail = "customer@email.com" 
            };
            
            var result1 = processor. ProcessOrder(order1);
            if (result1.IsSuccess)
            {
                Console.WriteLine($"Order {result1.Value.Id}:");
                Console. WriteLine($"  Amount: ${result1.Value. Amount}");
                Console. WriteLine($"  Discount: ${result1.Value.Discount}");
                Console.WriteLine($"  Tax: ${result1.Value.Tax:F2}");
                Console.WriteLine($"  Final: ${result1.Value. FinalAmount:F2}");
            }
            
            // Invalid order
            var order2 = new Order { Id = "ORD-002", Amount = -50 };
            var result2 = processor. ProcessOrder(order2);
            if (! result2.IsSuccess)
            {
                Console.WriteLine($"\n❌ Error: {result2. Error}");
            }
        }
    }
}
```

---

### 🚀 Scenario 2: Retry Policy with Exponential Backoff

```csharp
using System;
using System.Threading. Tasks;

namespace RetryPattern
{
    /// <summary>
    /// Configurable retry policy using delegates
    /// </summary>
    public class RetryPolicy
    {
        private int _maxRetries = 3;
        private TimeSpan _initialDelay = TimeSpan.FromSeconds(1);
        private double _backoffMultiplier = 2. 0;
        private Func<Exception, bool> _shouldRetry = _ => true;
        private Action<int, Exception, TimeSpan> _onRetry;
        private Func<int, TimeSpan, TimeSpan> _delayCalculator;
        
        public static RetryPolicy Create() => new RetryPolicy();
        
        /// <summary>
        /// Set maximum number of retry attempts
        /// </summary>
        public RetryPolicy WithMaxRetries(int maxRetries)
        {
            _maxRetries = maxRetries;
            return this;
        }
        
        /// <summary>
        /// Set initial delay between retries
        /// </summary>
        public RetryPolicy WithInitialDelay(TimeSpan delay)
        {
            _initialDelay = delay;
            return this;
        }
        
        /// <summary>
        /// Set exponential backoff multiplier
        /// </summary>
        public RetryPolicy WithExponentialBackoff(double multiplier = 2.0)
        {
            _backoffMultiplier = multiplier;
            _delayCalculator = (attempt, initial) => 
                TimeSpan.FromMilliseconds(initial. TotalMilliseconds * Math.Pow(_backoffMultiplier, attempt - 1));
            return this;
        }
        
        /// <summary>
        /// Set custom delay calculator
        /// </summary>
        public RetryPolicy WithCustomDelay(Func<int, TimeSpan, TimeSpan> calculator)
        {
            _delayCalculator = calculator;
            return this;
        }
        
        /// <summary>
        /// Configure which exceptions should trigger a retry
        /// </summary>
        public RetryPolicy RetryWhen(Func<Exception, bool> predicate)
        {
            _shouldRetry = predicate;
            return this;
        }
        
        /// <summary>
        /// Retry only for specific exception types
        /// </summary>
        public RetryPolicy RetryOn<TException>() where TException : Exception
        {
            var previous = _shouldRetry;
            _shouldRetry = ex => ex is TException && previous(ex);
            return this;
        }
        
        /// <summary>
        /// Callback when a retry occurs
        /// </summary>
        public RetryPolicy OnRetry(Action<int, Exception, TimeSpan> callback)
        {
            _onRetry = callback;
            return this;
        }
        
        /// <summary>
        /// Execute action with retry policy
        /// </summary>
        public async Task<T> ExecuteAsync<T>(Func<Task<T>> action)
        {
            var attempt = 0;
            var delay = _initialDelay;
            
            while (true)
            {
                attempt++;
                try
                {
                    return await action();
                }
                catch (Exception ex)
                {
                    if (attempt >= _maxRetries || !_shouldRetry(ex))
                        throw;
                    
                    if (_delayCalculator != null)
                        delay = _delayCalculator(attempt, _initialDelay);
                    
                    _onRetry?. Invoke(attempt, ex, delay);
                    
                    await Task.Delay(delay);
                }
            }
        }
        
        /// <summary>
        /// Execute void action with retry policy
        /// </summary>
        public async Task ExecuteAsync(Func<Task> action)
        {
            await ExecuteAsync(async () =>
            {
                await action();
                return true;
            });
        }
        
        /// <summary>
        /// Synchronous execution
        /// </summary>
        public T Execute<T>(Func<T> action)
        {
            var attempt = 0;
            var delay = _initialDelay;
            
            while (true)
            {
                attempt++;
                try
                {
                    return action();
                }
                catch (Exception ex)
                {
                    if (attempt >= _maxRetries || !_shouldRetry(ex))
                        throw;
                    
                    if (_delayCalculator != null)
                        delay = _delayCalculator(attempt, _initialDelay);
                    
                    _onRetry?. Invoke(attempt, ex, delay);
                    
                    Task.Delay(delay). Wait();
                }
            }
        }
    }
    
    // ==================== USAGE EXAMPLE ====================
    
    public class HttpClientWrapper
    {
        private readonly RetryPolicy _retryPolicy;
        private int _callCount = 0;
        
        public HttpClientWrapper()
        {
            _retryPolicy = RetryPolicy.Create()
                .WithMaxRetries(5)
                .WithInitialDelay(TimeSpan.FromMilliseconds(500))
                .WithExponentialBackoff(2.0)
                .RetryOn<HttpRequestException>()
                .RetryWhen(ex => ! ex.Message.Contains("404")) // Don't retry on 404
                .OnRetry((attempt, ex, delay) =>
                {
                    Console. WriteLine($"⚠️ Attempt {attempt} failed: {ex.Message}");
                    Console.WriteLine($"   Retrying in {delay. TotalMilliseconds}ms.. .");
                });
        }
        
        public async Task<string> GetDataAsync(string url)
        {
            return await _retryPolicy.ExecuteAsync(async () =>
            {
                _callCount++;
                Console.WriteLine($"📡 Making request #{_callCount} to {url}");
                
                // Simulate intermittent failures
                if (_callCount < 3)
                {
                    throw new HttpRequestException("Connection timeout");
                }
                
                await Task.Delay(100); // Simulate network delay
                return $"Data from {url}";
            });
        }
    }
    
    public class HttpRequestException : Exception
    {
        public HttpRequestException(string message) : base(message) { }
    }
    
    class Program
    {
        static async Task Main()
        {
            var client = new HttpClientWrapper();
            
            try
            {
                var result = await client.GetDataAsync("https://api.example.com/data");
                Console.WriteLine($"\n✅ Success: {result}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"\n❌ Final failure: {ex.Message}");
            }
        }
    }
}
```

---

### 🚀 Scenario 3: Specification Pattern with Expression Trees

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System. Linq.Expressions;

namespace SpecificationPattern
{
    /// <summary>
    /// Base specification interface using expression trees
    /// </summary>
    public interface ISpecification<T>
    {
        Expression<Func<T, bool>> ToExpression();
        bool IsSatisfiedBy(T entity);
    }
    
    /// <summary>
    /// Base specification class with AND, OR, NOT operations
    /// </summary>
    public abstract class Specification<T> : ISpecification<T>
    {
        public abstract Expression<Func<T, bool>> ToExpression();
        
        public bool IsSatisfiedBy(T entity)
        {
            var predicate = ToExpression(). Compile();
            return predicate(entity);
        }
        
        public Specification<T> And(Specification<T> other)
            => new AndSpecification<T>(this, other);
        
        public Specification<T> Or(Specification<T> other)
            => new OrSpecification<T>(this, other);
        
        public Specification<T> Not()
            => new NotSpecification<T>(this);
        
        public static Specification<T> operator &(Specification<T> left, Specification<T> right)
            => left.And(right);
        
        public static Specification<T> operator |(Specification<T> left, Specification<T> right)
            => left.Or(right);
        
        public static Specification<T> operator !(Specification<T> spec)
            => spec.Not();
    }
    
    /// <summary>
    /// Combines two specifications with AND
    /// </summary>
    internal class AndSpecification<T> : Specification<T>
    {
        private readonly Specification<T> _left;
        private readonly Specification<T> _right;
        
        public AndSpecification(Specification<T> left, Specification<T> right)
        {
            _left = left;
            _right = right;
        }
        
        public override Expression<Func<T, bool>> ToExpression()
        {
            var leftExpr = _left. ToExpression();
            var rightExpr = _right.ToExpression();
            
            var parameter = Expression.Parameter(typeof(T));
            
            var body = Expression.AndAlso(
                Expression. Invoke(leftExpr, parameter),
                Expression.Invoke(rightExpr, parameter));
            
            return Expression.Lambda<Func<T, bool>>(body, parameter);
        }
    }
    
    /// <summary>
    /// Combines two specifications with OR
    /// </summary>
    internal class OrSpecification<T> : Specification<T>
    {
        private readonly Specification<T> _left;
        private readonly Specification<T> _right;
        
        public OrSpecification(Specification<T> left, Specification<T> right)
        {
            _left = left;
            _right = right;
        }
        
        public override Expression<Func<T, bool>> ToExpression()
        {
            var leftExpr = _left.ToExpression();
            var rightExpr = _right.ToExpression();
            
            var parameter = Expression. Parameter(typeof(T));
            
            var body = Expression. OrElse(
                Expression.Invoke(leftExpr, parameter),
                Expression. Invoke(rightExpr, parameter));
            
            return Expression. Lambda<Func<T, bool>>(body, parameter);
        }
    }
    
    /// <summary>
    /// Negates a specification
    /// </summary>
    internal class NotSpecification<T> : Specification<T>
    {
        private readonly Specification<T> _specification;
        
        public NotSpecification(Specification<T> specification)
        {
            _specification = specification;
        }
        
        public override Expression<Func<T, bool>> ToExpression()
        {
            var expr = _specification.ToExpression();
            var parameter = Expression.Parameter(typeof(T));
            
            var body = Expression.Not(Expression.Invoke(expr, parameter));
            
            return Expression.Lambda<Func<T, bool>>(body, parameter);
        }
    }
    
    // ==================== DOMAIN EXAMPLE ====================
    
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string Category { get; set; }
        public int StockQuantity { get; set; }
        public bool IsActive { get; set; }
        public DateTime CreatedDate { get; set; }
        public double Rating { get; set; }
    }
    
    // Concrete specifications
    public class ActiveProductSpec : Specification<Product>
    {
        public override Expression<Func<Product, bool>> ToExpression()
            => p => p.IsActive;
    }
    
    public class InStockSpec : Specification<Product>
    {
        public override Expression<Func<Product, bool>> ToExpression()
            => p => p. StockQuantity > 0;
    }
    
    public class PriceRangeSpec : Specification<Product>
    {
        private readonly decimal _min;
        private readonly decimal _max;
        
        public PriceRangeSpec(decimal min, decimal max)
        {
            _min = min;
            _max = max;
        }
        
        public override Expression<Func<Product, bool>> ToExpression()
            => p => p.Price >= _min && p. Price <= _max;
    }
    
    public class CategorySpec : Specification<Product>
    {
        private readonly string _category;
        
        public CategorySpec(string category) => _category = category;
        
        public override Expression<Func<Product, bool>> ToExpression()
            => p => p.Category == _category;
    }
    
    public class MinRatingSpec : Specification<Product>
    {
        private readonly double _minRating;
        
        public MinRatingSpec(double minRating) => _minRating = minRating;
        
        public override Expression<Func<Product, bool>> ToExpression()
            => p => p.Rating >= _minRating;
    }
    
    