
# Result Type in Swift

Result types in swift gives us easier way of handling success and failure in complex code such asynchronous api calls.

Swift’s Result type is implemented as an enum that has two cases: success and failure. Both are implemented using generics so they can have an associated value of your choosing, but failure must be something that conforms to Swift’s Error type.

        enum Result<Success, Failure> where Failure : Error

Let's try with simple example

### To represent failure

```swift
enum CustomError: String, Error {
    case error1 = "error case 1"
    case error2 = "error case 2"
}
```

### To represent success

```swift
struct Model{
    let id: UUID = UUID()
    let name: String
}
```

### Using Result Types in asynchronous call

```swift
/**
 param = 1 to represent success case
 param =  other than 1 to represent failure case
 */

func asyncMethod(param: Int, completion:@escaping  (Result<Model, CustomError>)->Void){
    if param == 1{
        let model = Model(name: "rajan")
        completion(.success(model))
    }else {
        completion(.failure(.error1))
    }
}

```

### Implementation

```swift
asyncMethod(param: 1) { (result) in
    switch result {
    case .success(let model):
        print(model) //Model(id: 29ECCF11-4D97-4F47-86F2-505A3AC56838, name: "rajan")

    case .failure(let error):
        print(error.rawValue) //error case 1
    }
}
  
```


