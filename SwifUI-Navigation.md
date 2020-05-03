#  All about Navigation in SwiftUI


## NavigationView : Creating navigation stack

Following is the way to define top level navigation stack
```swift
var body: some View {
    NavigationView{
        Text("Hello, World!")
    }
}
```

### Giving title to navigation stack 

```swift
var body: some View {
    NavigationView{
        Text("Hello, World!")
        .navigationBarTitle("Navigation")
    }
    //.navigationBarTitle("Navigation") //-X not here
}
```

Navigation title is chained under text or other view under NavigationView that's because  it is the place where the main view lies and need title.

### Navigation title display mode

To customize tile size  **.navigationBarTitle** can take  **displayMode** as argument  that can take values of
- automatic 
- inline (use for detail)
- large (default)

```swift
.navigationBarTitle("Navigation", displayMode: .inline)
```

## Navigatoin Link:  Push and pop views

Navigation link is the way to link to new view. The **move to next view ** is automatically marked blue to mark  we can interact it link link.
```swift 
var body: some View {
    NavigationView{
        NavigationLink(destination: Text("New View")) {
            Text("Move to next View")
        }
        .navigationBarTitle("Navigation", displayMode: .large)
    }
}
```

If you want to use image as navigation link then the default tint color create a problem showing whole image blue.  To resolve this issue we will require **renderingMode** navigation link with image

```swift
NavigationLink(destination: Text("New View")) {
    //Text("Move to next View")
    Image("arrows")
    .renderingMode(.original)
}
```

### Custom view for navigation destination

```swift
struct ResultView: View {
    var choice: String
    var body: some View{
        Text("You choose \(choice)")
    }
}

NavigationView{
    VStack(spacing: 30){
        Text("Choose head or tail")
        NavigationLink(destination: ResultView(choice: "Head")) {
            Text("Head")
            //Image("arrows").renderingMode(.original)
        }
        
        NavigationLink(destination: ResultView(choice: "Tail")) {
            Text("Tail")
            //Image("arrows").renderingMode(.original)
        }
    }
    .navigationBarTitle("Navigation", displayMode: .large)
}
```

## Programatic Navigation

### isActive

```swift
@State var flag: Bool = false

NavigationLink(destination: Text("Detail View"), isActive: $flag) {
       EmptyView()
   }
   
   Button("Show Detail") {
       self.flag = true
   }
```

###  Navigation tag
```swift
struct ContentView: View {
    @State var option: String? = nil
    
    var body: some View {
        NavigationView{
            VStack(spacing: 30){
                
                NavigationLink(destination: Text("First View"), tag: "First", selection: $option) {
                    EmptyView()
                }
                NavigationLink(destination: Text("Second View"), tag: "Second", selection: $option) {
                    EmptyView()
                }
                
                Button("Show First") {
                    self.option = "First"
                }
                Button("Show Second") {
                    self.option = "Second"
                }
            }
            .navigationBarTitle("Navigation", displayMode: .large)
        }
    }
}

```

## Automatic pop out

```swift
struct ContentView: View {
    @State var flag: Bool = false
    
    var body: some View {
        NavigationView{
            VStack(spacing: 30){
                NavigationLink(destination: Text("Detail View"), isActive: $flag) {
                    EmptyView()
                }
                
                Button("Show Detail") {
                    self.flag = true
                    DispatchQueue.main.asyncAfter(deadline: .now() + 2.0) {
                        self.flag = false
                    }
                }
            }
            .navigationBarTitle("Navigation", displayMode: .large)
        }
    }
}
```

## EnviromentalObject:  Sharing data across the entire Navigation Stack 

```swift
lass User: ObservableObject {
    @Published var count: Int = 0
}

struct UserView: View {
    @EnvironmentObject var user: User
    
    var body: some View{
        VStack{
            Text("You choose \(user.count)")
            Button("Increase"){
                self.user.count += 1
            }
        }
    }
}

struct ContentView: View {
    @ObservedObject var user: User = User()
    
    var body: some View {
        NavigationView{
            VStack(spacing: 30){
                Text("You choose \(self.user.count)")
                NavigationLink(destination: UserView()) {
                   Text("Go to User View")
                }
            }
            .navigationBarTitle("Navigation", displayMode: .large)
        }.environmentObject(user)
    }
}
```

## Navigation Bar Item:

```swift
struct ContentView: View {
    @State var count: Int = 0
    
    var body: some View {
        NavigationView{
            Text("Count: \(self.count)")
                .navigationBarTitle("Navigation", displayMode: .inline)
                .navigationBarItems(
                    leading:
                    Button("Substract 1"){
                        self.count -= 1
                    },
                    
                    trailing:
                    HStack{
                        Button("Add 1"){
                            self.count += 1
                        }
                        
                        Button("Add 2"){
                            self.count += 2
                        }
                    }
            )
        }
    }
} 
```

## Hiding navigation bar and status bar

We need to hide the  navigation bar inside the navigation view and hide status bar outside the navvigation view.

```swift
struct ContentView: View {
    @State var fullScreen: Bool = false
    
    var body: some View {
        NavigationView{
            Button("Toggle Full Screen"){
                self.fullScreen.toggle()
            }
            .navigationBarTitle("Navigation", displayMode: .inline)
        .navigationBarHidden(fullScreen)
        }
    .statusBar(hidden: fullScreen)
    }
}
```

## Custumizing Navigation bar using UIKIt Apparance

There are two ways to do customization

- Apply apparance customization to whole navbar

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
       // Override point for customization after application launch.
       let apparance = UINavigationBarAppearance()
       apparance.configureWithOpaqueBackground()
       apparance.backgroundColor = .systemRed
       
       let attrs: [NSAttributedString.Key: Any] = [
           .foregroundColor: UIColor.red,
           .font: UIFont.monospacedSystemFont(ofSize: 30.0, weight: .black)
       ]
       
       apparance.largeTitleTextAttributes = attrs
       UINavigationBar.appearance().scrollEdgeAppearance = apparance
       
       return true
   }
```
- Apply apparance customization to single view.

Create a init method in SWiftUI view with following

```swift

init(){
    let apparance = UINavigationBarAppearance()
      //apparance.configureWithOpaqueBackground()
      apparance.backgroundColor = .systemRed
      
      let attrs: [NSAttributedString.Key: Any] = [
          .foregroundColor: UIColor.red,
          .font: UIFont.monospacedSystemFont(ofSize: 30.0, weight: .black)
      ]
      
      apparance.largeTitleTextAttributes = attrs
      UINavigationBar.appearance().scrollEdgeAppearance = apparance
      
}


```


## Creating single stack navigation.

In iPad app when we use navigation controller the default is split view style navigation. i.e link in left and detail in right. Simillarly when we rotate the iPhone application  to landscape it is taken as wide screen device and navigation view will behave exactly like in ipad. This is what we don't expect in iphone. To resolve this we can use  **StackNavigationViewStyle**

```swift

struct ContentView: View {
    var body: some View {
        NavigationView{
            NavigationLink(destination: Text("Detail View")) {
                Text("Show Detail")
            }
            .navigationBarTitle("Navigation", displayMode: .large)
        } .navigationViewStyle(StackNavigationViewStyle())
    }
}
```

## Navigation in watchOS

SwiftUI is follow **write once apply every any where**  not **write once run any where**.  Saying that when we use the sample navigation view code for watchOS it  won't work because watchOS have no Navigation View. It just accept **NavigationLink**. 

``` swift
struct ContentView: View {
    var body: some View {
        
        NavigationLink(destination: Text("Detail")){
             Text("Hello, World!")
        }
       
    }
}
```
But if you want to share same code across iOS and watchOS then we will need to write little wrapper around it without any performance impact.

```swift

#if os(watchOS)
struct NavigationView <Content: View>: View {
    let content: ()-> Content
    
    init(@ViewBuilder content: @escaping ()-> Content) {
        self.content = content
    }
    
    var body: some View{
        VStack(spacing: 0){
            content()
        }
    }
    
}

#endif

struct ContentView: View {
    var body: some View {
        NavigationView{
            NavigationLink(destination: Text("Detail")){
                  Text("Hello, World!")
             }
            
        }
        
    }
}

```
