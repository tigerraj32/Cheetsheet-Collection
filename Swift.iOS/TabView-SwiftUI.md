# TabView

SwiftUI provides the **TabView** that does exactly same as **UITabViewController** in UIKit.

![](resources/tabview1.png)
```swift
struct TabView1: View {
    var body: some View {
        Text("Tabview 1")
    }
}

struct TabView2: View {
    var body: some View {
        Text("Tabview 2")
    }
}

struct ContentView: View {
    var body: some View {
        TabView {
            TabView1()
                .tabItem {
                    Image(systemName: "list.dash")
                    Text("Tab 1")
            }.tag(0)
            
            TabView2()
                .tabItem {
                    Image(systemName: "square.and.pencil")
                    Text("Tab 2")
            }.tag(1)
        }
    }
}
```

