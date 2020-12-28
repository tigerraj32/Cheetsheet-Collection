#  All about Navigation in SwiftUI
refrence resource: [link](https://www.youtube.com/watch?v=nA6Jo6YnL9g)


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

## Customize nagigation bar and  title (color)
[Refrence Docs](https://schwiftyui.com/swiftui/customizing-your-navigationviews-bar-in-swiftui/)

For now ther no direct support to customize navigation bar and title in SwiftUI. But, there are multiple ways to customize navigation bar and title.

### Globally change the navigation bar and title color

Since we are using the `UINavigationBar` apparance this will globally change the navigation views.

```swift
struct ContentView: View {
    init() {
        UINavigationBar.appearance().backgroundColor = .blue
    }
    
    var body: some View {
        NavigationView {
            NavigationLink(destination:
                Text("First detail")) {
                    Text("Go to first detail")
            }
        }
    }
}
```

### Change navigation bar per view

We can change the appareance of NavigationBar in `onAppear` of each view to give color of navigation bar for each view

```swift 

struct FirstTabView: View {
    var body: some View {
        NavigationView {
            Text("First detail")
        }
        .onAppear {
            UINavigationBar.appearance().backgroundColor = .green
        }
    }
}

struct SecondTabView: View {
    var body: some View {
        NavigationView {
            Text("Second detail")
        }
        .onAppear {
            UINavigationBar.appearance().backgroundColor = .blue
        }
    }
        
}

```

If we want to apply changes before the view load then we can maake it in 'viewDidLoad`

```swift
extension UINavigationController {
    override open func viewDidLoad() {
        super.viewDidLoad()

        let standardAppearance = UINavigationBarAppearance()
        standardAppearance.backgroundColor = .red

        let compactAppearance = UINavigationBarAppearance()
        compactAppearance.backgroundColor = .green

        let scrollEdgeAppearance = UINavigationBarAppearance()
        scrollEdgeAppearance.backgroundColor = .blue

        navigationBar.standardAppearance = standardAppearance
        navigationBar.compactAppearance = compactAppearance
        navigationBar.scrollEdgeAppearance = scrollEdgeAppearance
    }
}
```

### Resuable modifier to  apply changes to navigation bar and title 

Create new file `NavigationBarColorModifier`

```swift
import SwiftUI
import UIKit

struct NavigationBarColorModifier: ViewModifier {
    init(backgroundColor: UIColor, tintColor: UIColor) {
        let coloredAppearance = UINavigationBarAppearance()
        coloredAppearance.configureWithOpaqueBackground()
        coloredAppearance.backgroundColor = backgroundColor
        coloredAppearance.titleTextAttributes = [.foregroundColor: tintColor]
        coloredAppearance.largeTitleTextAttributes = [.foregroundColor: tintColor]
                       
        UINavigationBar.appearance().standardAppearance = coloredAppearance
        UINavigationBar.appearance().scrollEdgeAppearance = coloredAppearance
        UINavigationBar.appearance().compactAppearance = coloredAppearance
        UINavigationBar.appearance().tintColor = tintColor
      }
    
    func body(content: Content) -> some View {
        content
      }

}


extension View {
  func navigationBarColor(backgroundColor: UIColor, tintColor: UIColor) -> some View {
    self.modifier(NavigationBarColorModifier(backgroundColor: backgroundColor, tintColor: tintColor))
  }
}

```

Now we can apply modifier as below

```swift 
NavigationView {
            Text("First detail")
        }
        .navigationBarColor(backgroundColor: .blue, tintColor: .white)
```

## Navigatoin Link

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

## Programatic Navigation (Push)

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
# Programatic Navigation (Pop)
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
## Programatic POP or Dismiss

You can programmatically pop back by using dismiss method on EnvironmentValue called **presentationMode**.

```swift
struct Detail: View {
    @Environment(\.presentationMode) var presentation
    var body: some View{
        VStack{
            Text("Detail Page")
            Button("Go Back"){
                self.presentation.wrappedValue.dismiss()
            }
        }
    }
}
struct ContentView: View {
    var body: some View {
        NavigationView{
            VStack{
               Text("Master View")
                NavigationLink(destination: Detail()) {
                    Text("Go to Detail")
                }
            }
        }
    }
}

```

## Programatic POP: Pop to root view

If you have stact of views and want to pop to root view  from last stack, then we have two option. 
Setting the view modifier **isDetailLink** to **false** on a **NavigationLink** is the key to getting pop-to-root to work. **isDetailLink** is **true** by **default** and is adaptive to the containing View. On iPad landscape for example, a Split view is separated and **isDetailLink** ensures the destination view will be shown on the right-hand side. Setting **isDetailLink** to false consequently means that the destination view will always be pushed onto the navigation stack; thus can always be popped off.

 ### Passing State variable of root view as binding variable to subsequent child views.


```swift

struct Detail2: View {
    @Binding var showRootView: Bool
    var body: some View{
        VStack{
            Text("Detail Page 2")
            Button("Go Home"){
                self.showRootView = false
            }
        }
    }
}
struct Detail1: View {
    @Binding var showRootView: Bool
    var body: some View{
        VStack{
            Text("Detail Page")
            
            NavigationLink(destination: Detail2(showRootView: $showRootView)) {
               Text("Go to Detail 2")
           }
            .isDetailLink(false)
           
        }.navigationBarTitle("Detail 1", displayMode: .inline)
    }
}


struct ContentView: View {
    @State var showRootView: Bool = false
    var body: some View {
        NavigationView{
            VStack{
               Text("Master View")
                NavigationLink(destination: Detail1(showRootView: $showRootView), isActive: $showRootView) {
                    Text("Go to Detai 1")
                }.isDetailLink(false)
            }
            .navigationBarTitle("Navigation")
        }
    }
}

```

### Using Envigomental Variable

- Step 1: create Observable class

```swift
final class NavigationFlowObject: ObservableObject {
    static let root =  false
    static let child = true
    @Published var currentView: Bool = NavigationFlowObject.root
}
```
- Step 2: Create an instance of NavigationFlowObject Object and pass it in envorimentObject.
  
  We can pass observedObject to child view via enviroment in two ways.  First via enviromentObject in ContentView like below

```swift
struct ContentView: View {
    @ObservedObject var navigationFlow: NavigationFlowObject = NavigationFlowObject()
    var body: some View {
        NavigationView{
            VStack{
                
                Text("Master View")
                NavigationLink(destination: Detail1(), isActive:self.$navigationFlow.currentView) { //Binding variable
                    Text("Go to Detai 1")
                }.isDetailLink(false)
            }
            .navigationBarTitle("Navigation")
        }
        .environmentObject(navigationFlow)
        
    }
}
```


Second by passsing enviromentObject in **RootView** at **SceneDelegate**

```swift
    // In SceneDelegate
    let contentView = ContentView()environmentObject(NavigationFlowObject())
     if let windowScene = scene as? UIWindowScene {
            let window = UIWindow(windowScene: windowScene)
            window.rootViewController = UIHostingController(rootView: contentView).environmentObject(NavigationFlowObject())
            self.window = window
            window.makeKeyAndVisible()
        }

// In ContentView (Master View) 
 struct ContentView: View {
    @EnvironmentObject var navigationFlow: NavigationFlowObject
    var body: some View {
        NavigationView{
            VStack{
                
              Text("Master View")
              NavigationLink(destination: Detail1(), isActive:self.$navigationFlow.currentView) { //Binding variable
                    EmptyView()
                }.isDetailLink(false)
                
                Button("Go To Detail 1"){
                    self.navigationFlow.currentView = NavigationFlowObject.child
                }
            }
            .navigationBarTitle("Navigation")
            
            .onReceive(self.navigationFlow.$currentView) { (rootViewActive) in // Publisher
                print(rootViewActive)
             }

        }
        
    }
}      
```
We can listen to change in Navigation FLow  **currentView** in **onReceive**

-  Step 3: Create Detail  View

```swift
struct Detail2: View {
    @EnvironmentObject var navigationFlow: NavigationFlowObject
    var body: some View{
        VStack{
            Text("Detail Page 2")
            Button("Go Home"){
                self.navigationFlow.currentView = NavigationFlowObject.root
            }
        }
    }
}
struct Detail1: View {
   
    var body: some View{
        VStack{
            Text("Detail Page")
            
            NavigationLink(destination: Detail2()) {
               Text("Go to Detail 2")
           }
            .isDetailLink(false)
           
        }.navigationBarTitle("Detail 1", displayMode: .inline)
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


# Action Sheet
```swift
struct ContentView: View {
    @State private var showActionSheet = false
    var body: some View {
        
        VStack{
           Button("Show action sheet") {
                self.showActionSheet = true
            }
        }.actionSheet(isPresented: $showActionSheet) { () -> ActionSheet in
            ActionSheet(title: Text("Action Title"), message: Text("Action Message"), buttons: [
                .cancel({
                    print("Cancel Pressed")
                }),
                .default(Text("Action 1"), action: {
                    print("Action 1")
                }),
                .default(Text("Action 2"), action: {
                    print("Action 2")
                }),
                .destructive(Text("Destructive"), action: {
                      print("Destructive")
                })
                
                    ]
            )
        }
    }
}
```
