# Introduction

The Combine framework provides a declarative Swift API for processing values over time. These values can represent many kinds of asynchronous events. 
Combine declares publishers to expose values that can change over time, and subscribers to receive those values from the publishers.

- **The Publisher**

   The publisher protocol declares a type that can deliver a sequence of values over time. Publishers have operators to act on the values received from upstream 
   publishers and republish them.

- **The Subscriber**

  A Subscriber instance receives a stream of elements from a Publisher.
  
  
### Basic intro to combine framework.
  
  ![](../resources/combine_intro.png)

  
  In above example we are demonstrating a 
  
  - Timer publisher that publish a value each 10 ms.
  - Publisher operator that receives each streams of input and modifies, then republish the modified values
  - And finally a subscriber that receives the stream of input.
 
 Lets breakdown the above example in more simple way
 
 **Step 1**
 
 Let's create a publisher that publish values every second in main loop.
  
  ```swift
   var subscripton = Timer.publish(every: 1, on: .main, in: .common)
   
   ```
   
**Step 2**

Now let's create a subscriber that subscribe to published values. Here we use sink as a subscriber. 

```swift
 var subscripton = Timer.publish(every: 1, on: .main, in: .common)
   .sink { (output) in
        print("finished stream with output \(output)")
    } receiveValue: { (value) in
        print("Received Value: \(value)")
    }
```
After this we will periodically receive the values every 1 second. `sink` have two completion handler. First one will executed when the subscription ends. 
Second one will execute every time the subscriber receives the new values. If you wish to publish the value for limited amount of time say for first 25 second then we can schedule the runloop to do so.

```swift
RunLoop.main.schedule(after: .init(Date(timeIntervalSinceNow: 25))) {
    print("---- Cancel -----")
    subscripton.cancel()
}
```

**Step 3** 

Now we will implement operators over the publisher to modify or filter input stream before it is delivered to subscriber.

- `scan` can take initial value, modifies that and produce the next value. Lest say we want to count the number of second elapsed instead of sending the timer instanc to subscriber

```swift
.scan(0, { (count, _) in
        return count + 1
 })
```

- `filter` can filter the input stream before it reach to subscriber. Here we want to republish only if the steam is > 5 and < 20. All other input are ignored.

```swift
.filter({$0 > 5 && $0 < 15})
```

- `throttle` can helps us ignore certain upstream values and emit upstream value at specified time instance. For eg. if we modified the publisher to publish every 10 ms, but we want to receive the only after each second.

```swift
var subscripton = Timer.publish(every: 0.1, on: .main, in: .common)
    .autoconnect()
    .print("Streams")
    .throttle(for: .seconds(1), scheduler: DispatchQueue.main, latest: true)
```





