
## Wait for async task to complete's

Swift privide a handly api **DispatchGroup** that allows us to  monitor group of task as single unit.
For simplicity below we have


 ```swift
let dg = DispatchGroup()
dg.enter()
print("async task start")
DispatchQueue.main.async {
    //... time taking operation
    dg.leave()
}
dg.wait()
print("async task complete")
```

If you have series of async task and need to monitor for all the async task to complete
```swift
print("<<<<< Start >>>>>")

let dg = DispatchGroup()

for i in 1...5{
    print("Start For: \(i)")
    dg.enter()
    DispatchQueue.main.asyncAfter(deadline: .now() + Double(i)) {
       print("End For: \(i)")
        dg.leave()
    }
}
dg.notify(queue:  .main) {
    print("Completed all task")
}

print("<<<<< End >>>>>")
```

> Output

```
<<<<< Start >>>>>
Start For: 1
Start For: 2
Start For: 3
Start For: 4
Start For: 5
<<<<< End >>>>>
End For: 1
End For: 2
End For: 3
End For: 4
End For: 5
Completed all task
```

### notify()
This function schedules a notification block to be submitted to the specified queue when all blocks associated with the 
dispatch group have completed. If the group is empty (no block objects are associated with the dispatch group), the 
notification block object is submitted immediately. When the notification block is submitted, the group is empty


## Multiple ways of delaying async calls.

**Option 1.**

```swift
DispatchQueue.main.asyncAfter(deadline: .now() + 5.0) {
    self.yourFuncHere()
}
//Your function here    
func yourFuncHere() {

}
```

**Option 2.**

```swift
perform(#selector(yourFuncHere2), with: nil, afterDelay: 5.0)

//Your function here  
@objc func yourFuncHere2() {
    print("this is...")
}
```

**Option 3.**

```swift
Timer.scheduledTimer(timeInterval: 5.0, target: self, selector: #selector(yourFuncHere3), userInfo: nil, repeats: false)

//Your function here  
@objc func yourFuncHere3() {

}
```

**Option 4.**

```swift
sleep(5)
```

