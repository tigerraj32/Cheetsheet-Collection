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


## Builder Pattern

Builder pattern is a way of creating object by seperating the construction process of complex object in much simpleer way and used builderType to initialize the object. There are multiple way we can implement **builder pattern**, but that all depends on the use case.

### Problem we are trying to solve

In below code we declare struct with bunch of properties. 

```swift
struct BurgerOld {
    var name: String
    var patties: Int
    var bacon: Bool
    var cheese: Bool
    var pickles: Bool
    var ketchup: Bool
    var mustard: Bool
    var lettuce: Bool
    var tomato: Bool
    init(name: String, patties: Int, bacon: Bool, cheese: Bool,
         pickles:Bool,ketchup: Bool,mustard: Bool,lettuce: Bool, tomato: Bool) {
        self.name = name
        self.patties = patties
        self.bacon = bacon
        self.cheese = cheese
        self.pickles = pickles
        self.ketchup = ketchup
        self.mustard = mustard
        self.lettuce = lettuce
        self.tomato = tomato
    }
    
}

```

and to initialize such struct we need to write lots of code as following

```swift 
// Create Hamburger
var hamburger = BurgerOld(name: "Hamburger", patties: 1, bacon: false, cheese: false, pickles: false, ketchup: false, mustard: false, lettuce: false, tomato: false)
// Create Cheeseburger
var cheeseburger = BurgerOld(name: "Cheeseburger", patties: 1 , bacon: false, cheese: false, pickles: false, ketchup: false, mustard: false, lettuce: false, tomato: false)

```


### Implementation 1: Chain Builder pattern

Let's take an example to create request url via builder patter. Here we declare components as private var to avoid accidental initialization of its value. It can only be alter by any of the set method. Here every set method will return updated instance of object.


```swift
import UIKit

class URLBuilder {
    private var components: URLComponents
    init() {
        self.components = URLComponents()
    }
    func set(scheme: String) -> URLBuilder {
        self.components.scheme = scheme
        return self
    }

    func set(host: String) -> URLBuilder {
        self.components.host = host
        return self
    }

    func set(port: Int) -> URLBuilder {
        self.components.port = port
        return self
    }

    func set(path: String) -> URLBuilder {
        var path = path
        if !path.hasPrefix("/") {
            path = "/" + path
        }
        self.components.path = path
        return self
    }

    func addQueryItem(name: String, value: String) -> URLBuilder  {
        if self.components.queryItems == nil {
            self.components.queryItems = []
        }
        self.components.queryItems?.append(URLQueryItem(name: name, value: value))
        return self
    }

    func build() -> URL? {
        return self.components.url
    }
}

let url = URLBuilder()
    .set(scheme: "https")
    .set(host: "localhost")
    .set(path: "api/v1")
    .addQueryItem(name: "sort", value: "name")
    .addQueryItem(name: "order", value: "asc")
    .build()

print(url)

```

Let's look at one more example to demonstrate builder pattern.

```swift
class RequestBuilder {
    private(set) var baseURL: URL!
    private(set) var endpoint: String = ""
    private(set) var method: Method = .GET
    private(set) var headers: [String: String] = [:]
    private(set) var parameters: [String: String] = [:]
    
    public func setBaseUrl(_ string: String) {
        baseURL = URL(string: string)!
    }
    public func setEndpoint(_ value: String) {
        endpoint = value
    }
    public func setMethod(_ value: Method) {
        method = value
    }
    public func addHeader(_ key: String, _ value: String) {
        headers[key] = value
    }
    public func addParameter(_ key: String, _ value: String) {
        parameters[key] = value
    }

    public func build() -> URLRequest {
        assert(baseURL != nil)
        let url = baseURL.appendingPathComponent(endpoint)
        var components = URLComponents(string: url.absoluteString)

        components?.queryItems = parameters.compactMap { URLQueryItem(name: $0.key, value: $0.value) }

        var urlRequest = URLRequest(url: components!.url!)
        urlRequest.httpMethod = method.rawValue

        for header in headers {
            urlRequest.addValue(header.value, forHTTPHeaderField: header.key)
        }

        return urlRequest
    }
}

```

### Implementation 2: Now let's create builder to URLSessionDataTask

```swift
class TaskBuilder {
    private(set) var request: URLRequest!
    
    public func setRequest(_ request: URLRequest) {
        self.request = request
    }
    
    public func build() -> URLSessionDataTask {
        assert(request != nil)
        return URLSession.shared.dataTask(with: request) {
            (data, response, error) in
            
            guard let data = data,
                   let response = response as? HTTPURLResponse,
                   (200 ..< 300) ~= response.statusCode,
                   error == nil else {
                       return
               }
               
            if let responseObject = (try? JSONSerialization.jsonObject(with: data)) as? [String: Any] {
               print(responseObject)
            }
        }
    }
}
```

Implementation

```swift
let requestBuilder = RequestBuilder()
requestBuilder.setBaseUrl(RequestBaseUrls.gitHub)
requestBuilder.setEndpoint(RequestEndpoints.searchRepositories)
requestBuilder.setMethod(.GET)
requestBuilder.addHeader("Content-Type", "application/json")
requestBuilder.addParameter("q", "Builder Design Pattern")
let request = requestBuilder.build()

let taskBuilder = TaskBuilder()
taskBuilder.setRequest(request)
let task = taskBuilder.build()

task.resume()

```



## Factory Pattern

Factory pattern involves a Factory that creates variety of objects but hide the logic behind object creation. Such object are related to each other either by sharing common parent class or conform to same protocol. The Factory object contains all the logic that allows it to instantiate the correct product to return to the caller.


### Implementation1:
```swift 

// Parent Class
class Toy {
  func play() { }
}

// Child Classes (Products)
class Train: Toy {
  override func play() {
    print("Choo Choo!")
  }
}

class Trampoline: Toy {
  override func play() {
    print("Jump High")
  }
}

class Ball: Toy {
  override func play() {
    print("Bounce")
  }
}

class NintendoSwitch: Toy {
  override func play() {
    print("Save Hyrule!")
  }
}

// 1
protocol ToyProducing {
  static func produceToy(for age: Int) -> Toy
}

class CanadianToyFactory {
  // Implement Logic
  static func produceToy(for age: Int) -> Toy {
    switch age {
    case 0...6:
      return Train()
    case 7...9:
      return Trampoline()
    case 10...18:
      return Ball()
    case _ where age > 18:
      return NintendoSwitch()
    default:
      return Ball()
    }
  }
}



// Using the toy factory
let toy = CanadianToyFactory.produceToy(for: 2)
toy.play()

class FrenchToyFactory: ToyProducing {
  
  // Implement Unique Logic
  static func produceToy(for age: Int) -> Toy {
    switch age {
    case 0...7:
      return Train()
    case 8...15:
      return Ball()
    case _ where age > 16:
      return NintendoSwitch()
    default:
      return Ball()
    }
  }
}

```

Depending on the country  the age group are different. So **CanadianToyFactory** and  **FrenchToyFactory** are two factory method that create the object depending on country  and age.



### Implementation 2:
Below is one more example where we create a factory method to get subjects for each semester.

```swift

class Syllabus {
    var subjects: [String] = []
}

protocol SyllabusPtotocol{
    func setSubjects()->Syllabus
}


class Semester1 : Syllabus, SyllabusPtotocol {
    func setSubjects() -> Syllabus {
        let subject = Syllabus()
        subject.subjects =  ["English, Math, C Programing"]
        return subject
        
    }
}

class Semester2 : Syllabus, SyllabusPtotocol {
    func setSubjects() -> Syllabus {
        let subject = Syllabus()
        subject.subjects =  ["Nepali, Appliedn Math, C++ Programing"]
        return subject
    }
}

enum SemesterType:Int{
    case semester1
    case semester2
}

class SyllabusFactory {
    static func createSyllabus(semester: SemesterType) -> Syllabus{
        switch semester {
        case .semester1:
            return Semester1().setSubjects()
        case .semester2:
            return Semester2().setSubjects()
        }
    }
}

let syllabus = SyllabusFactory.createSyllabus(semester: .semester1)
print(syllabus.subjects)

```

