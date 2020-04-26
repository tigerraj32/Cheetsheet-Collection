## Associated Types

Associated Types are powerful way of making generics in protocol. It marks a hole in protocol that must be filled by
whatever type conforms to those protocol.

Let’s start with a simple example: an ItemStoring protocol that can store items in an array. What type those items are 
depends on whatever conforms to the protocol, but we can still use them inside the protocol and any extensions.

Here’s the basic protocol:

```swift
protocol ItemStoring {
    associatedtype DataType

    var items: [DataType] { get set}
    mutating func add(item: DataType)
}
```
That mutating method is probably going to be the same for all conforming types, so we can write a protocol extension that
provides a default implementation:

```swift
extension ItemStoring {
    mutating func add(item: DataType) {
        items.append(item)
    }
}
```

Finally we can create a NameDatabase struct that conforms to the ItemStoring protocol like this:

```swift
struct NameDatabase: ItemStoring {
    //typealias DataType = String
    var items = [String]()
}
```

Swift is smart, though: the typealias line explicitly tells Swift that ItemType is a string, but Swift is able to figure
that out because items is an array of strings and therefore ItemType must be a string.
