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