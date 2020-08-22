# Design Pattern


Design pattern is a way of solving common programming problems. Design pattern can be broadly classify into 3 type

- **Creational Pattern:** Creational patterns support the creation of objects
- **Structural Pattern:** Structural patterns concern types and object compositions
- **Behaviour Pattern:** Behavioral patterns communicate between types


<br>

## Creational Pattern
Creational patterns are design patterns that deal with how an object is created. These patterns create objects in a manner suitable for a particular situation.

There are two basic ideas behind creational patterns. The first is encapsulating the knowledge of which concrete types should be created and the second is hiding how the instances of these types are created.

- **Abstract factory pattern:** This provides an interface for creating related objects without specifying the concrete type
- **Builder pattern:** This separates the construction of a complex object from its representation, so the same process can be used to create similar types
- **Factory method pattern:** This creates objects without exposing the underlying logic of how the object (or which type of object) is created
- **Prototype pattern:** This creates an object by cloning an existing one 
- **Singleton pattern:** This allows one (and only one) instance of a class for the lifetime of an applications


<br><br>

## Singleton Pattern

The single pattern is a way of creating object in such a way that it restrics the instantiatoin of class to a single instance for the life time of application. Singleton pattern can only be applied to refrence type i.e. **class object**

### When to use ?

Singleton pattern can be used when we need to make instance of class that can be accesssible from any part of application and modify the object at any point within the application.

### Implementation

```swift

class MyInsttance {
    static let shared = MyInsttance()
    var count: Int = 0
    
    private init(){}
}

let a = MyInsttance.shared
let b = MyInsttance.shared
let c = MyInsttance.shared

c.count = 3

print(a.count, b.count, c.count) // 3,3,3 all instance will refrence to same value.

```
