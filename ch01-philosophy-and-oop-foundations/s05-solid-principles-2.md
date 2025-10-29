# SOLID Principles II
The **SOLID principles** are five software design rules that help developers create systems that are easy to maintain, extend, and scale.  
Here we’ll focus on three of them:

## Liskov Substitution Principle (LSP)

### Definition
> Subtypes must be substitutable for their base types without changing the correctness of the program.

That means if `B` is a subclass of `A`, you should be able to use `B` anywhere you use `A` — and the behavior should remain consistent and logical.

### Bad Example
~~~python
class Bird:
    def fly(self):
        print("I can fly!")

class Penguin(Bird):
    def fly(self):
        raise Exception("I can't fly!")  # violates LSP
~~~

### Good Example
~~~python
class Bird:
    def make_sound(self):
        print("Chirp chirp")

class FlyingBird(Bird):
    def fly(self):
        print("I can fly!")

class Penguin(Bird):
    def swim(self):
        print("I can swim!")
~~~
Now we’ve separated `FlyingBird` and `Penguin` logically.  
Each subclass maintains its own consistent behavior, and `Penguin` no longer violates expectations about flying.

### Real-World Analogy
Imagine a **car rental system** where every vehicle is expected to have a `drive()` method.  
If you add a `Boat` subclass that can only `sail()`, substituting it for a `Car` would make no sense — you’d violate LSP.  
Instead, separate `Drivable` and `Sailable` abstractions.

### Key Takeaway
-   Subclasses must **preserve** the behavior promised by the parent class.
-   Inheritance should model **real “is-a” relationships**, not just code reuse.

## Interface Segregation Principle (ISP)

### Definition
> Clients should not be forced to depend on interfaces they do not use.

Instead of creating one large interface with many unrelated methods, split it into smaller, more focused ones.

### Bad Example
~~~python
class MultiFunctionPrinter:
    def print(self): pass
    def scan(self): pass
    def fax(self): pass

class BasicPrinter(MultiFunctionPrinter):
    def print(self):
        print("Printing...")
    
    def scan(self):
        raise Exception("Cannot scan!")  # violates ISP
    
    def fax(self):
        raise Exception("Cannot fax!")   # violates ISP
~~~

`BasicPrinter` shouldn’t have to implement `scan()` and `fax()` if it can’t perform those actions.

### Good Example
~~~python
class Printer:
    def print(self): pass

class Scanner:
    def scan(self): pass

class Fax:
    def fax(self): pass

class BasicPrinter(Printer):
    def print(self):
        print("Printing...")

class OfficeMachine(Printer, Scanner, Fax):
    def print(self): pass
    def scan(self): pass
    def fax(self): pass
~~~
Each device now implements only the methods it actually needs.  
This makes the design cleaner, easier to maintain, and more flexible.

### Real-World Analogy
Imagine a **universal remote** that has buttons for controlling TVs, AC units, and washing machines.  
If you only own a TV, you shouldn’t need to worry about the washing machine buttons.  
Each device should have its **own interface**.

### Key Takeaway
-   Create **multiple small, specific** interfaces rather than one large, general one.
-   Each class should depend **only on what it uses.**

## Dependency Inversion Principle (DIP)

### Definition
> High-level modules should not depend on low-level modules. Both should depend on abstractions.  
Abstractions should not depend on details; details should depend on abstractions.

### Bad Example
~~~python
class MySQLDatabase:
    def connect(self):
        print("Connecting to MySQL...")

class DataHandler:
    def __init__(self):
        self.db = MySQLDatabase()  # tightly coupled to MySQL

    def read_data(self):
        self.db.connect()
        print("Reading data from MySQL...")
~~~
Here, `DataHandler` is **tightly coupled** to `MySQLDatabase`.  
If we later want to switch to `PostgreSQL`, we’d have to modify the `DataHandler` class — violating DIP.

### Good Example
~~~python
class Database:
    def connect(self): pass

class MySQLDatabase(Database):
    def connect(self):
        print("Connecting to MySQL...")

class PostgreSQLDatabase(Database):
    def connect(self):
        print("Connecting to PostgreSQL...")

class DataHandler:
    def __init__(self, db: Database):
        self.db = db  # depends on abstraction, not implementation

    def read_data(self):
        self.db.connect()
        print("Reading data from the database...")

# Usage
mysql = MySQLDatabase()
handler = DataHandler(mysql)
handler.read_data()
~~~
Now, the high-level `DataHandler` doesn’t care which database is used.  
It only depends on the **abstract** `Database` interface.

### Real-World Analogy
Think of a **power outlet** and **devices**.  
The outlet provides a standard interface (socket shape, voltage).  
You can plug in any device — a lamp, charger, or TV — as long as it matches the interface.  
The outlet doesn’t depend on any specific appliance.  
That’s dependency inversion in real life.

### Key Takeaway
-   High-level code should rely on **interfaces or abstractions**, not concrete implementations.
-   This allows for **easy swapping**, **testing**, and **future-proofing**.

### Summary Table

| Principle | What It Means | Real-World Analogy |
|--|--|--|
| **Liskov Substitution** | Subclasses should behave like their base classes | A boat pretending to be a car |
| **Interface Segregation** | Don’t force classes to implement unused methods | Toaster shouldn’t need blender controls |
| **Dependency Inversion** | Depend on abstractions, not details | Power outlet that supports any device |

These principles are not just theoretical — they are **practical tools** to make your software modular, adaptable, and robust.

When you apply:
-   **LSP**, your inheritance hierarchy behaves correctly.
-   **ISP**, your interfaces are clean and relevant.
-   **DIP**, your system becomes loosely coupled and easier to maintain.

In short, they’re the backbone of **object-oriented design done right.**