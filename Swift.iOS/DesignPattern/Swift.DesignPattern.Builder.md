
# Builder Pattern

Builder pattern is a way of creating object by seperating the construction process of complex object in much simpleer way and used builderType to initialize the object. There are multiple way we can implement **builder pattern**, but that all depends on the use case.

## Problem we are trying to solve

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


## Implementation 1: Chain Builder pattern

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

## Implementation 2: Now let's create builder to URLSessionDataTask

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

