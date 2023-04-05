
The wrappers that begin with "@" in SwiftUI are called property wrappers. Property wrappers are a language feature introduced in Swift that allow you to define custom behavior for properties, such as adding validation, transformation, or observation logic, by encapsulating it within a separate type. SwiftUI makes extensive use of property wrappers to provide powerful and convenient ways to manage data and behavior in views. Here is a list of some common property wrappers available in Swift:


- `@State`: Used to declare a mutable state property in a view. This property wrapper allows the view to store and manage mutable data that can change over time, and automatically updates the view when the data changes.

- `@Binding`: Used to create a two-way connection between a view and its underlying data. This property wrapper allows a view to read and modify the value of a piece of data, and automatically updates the view and the data when changes occur.

- `@ObservedObject and @EnvironmentObject`: Used to manage external data sources in a view. These property wrappers allow a view to observe changes in external data sources, such as objects that conform to the ObservableObject protocol, and automatically update the view when changes occur.

- `@Published`: Used to automatically publish changes to a property. This property wrapper allows a property to automatically notify observers when its value changes, which is useful for implementing reactive programming patterns.

- `@StateObject`: Used to manage the lifecycle of an object in a view. This property wrapper allows a view to create and manage an object that should persist across the lifecycle of the view, such as a network manager or a data model.

- `@Environment`: Used to access values from the environment in a view. This property wrapper allows a view to access values such as color schemes, font sizes, and accessibility settings that are provided by the system or the app's environment.

- `@FetchRequest`: Used to fetch data from a Core Data store in SwiftUI views. This property wrapper simplifies the process of fetching data from a Core Data store and automatically updating the view when changes occur.

- `@AppStorage and @SceneStorage`: Used to store and retrieve user preferences or app-specific data in a SwiftUI app. These property wrappers provide a simple way to store and retrieve data that persists across app launches or scene changes.

- `@ScaledMetric`: Used to create views that adapt to the user's accessibility settings, such as Dynamic Type or Larger Text accessibility setting. This property wrapper allows a view to automatically adjust its appearance based on the user's preferred text size.

- `@BindingSlice`: Used to create a binding to a portion of an array in SwiftUI. This property wrapper allows you to create bindings to specific elements or subranges of an array, making it easier to work with arrays in SwiftUI views.

- `@Namespace`: Used to synchronize the layout of views across different containers, such as transitions between views in different screens or view hierarchies.

- `@FocusState`: Used to manage the focus state of a view in SwiftUI. This property wrapper allows you to manage the focus state of views, such as text fields or buttons, and respond to changes in focus.

- `@ScaledToFit and @ScaledToFill`: Introduced in SwiftUI 3.0, these property wrappers allow you to scale an image or other view to fit or fill its available space while maintaining its aspect ratio. These property wrappers are useful for creating responsive and adaptable layouts.

- `@GestureState and @GestureStateObject`: Introduced in SwiftUI 2.0, these property wrappers allow you to manage the state of gestures in a view. @GestureState is used for simple gestures, while @GestureStateObject is used for more complex gestures that require custom state management.

- `@FetchRequestResult`: Introduced in Core Data 3.0, this property wrapper allows you to fetch data from a Core Data store and automatically update a SwiftUI view when changes occur. It provides a more efficient way to fetch Core Data objects compared to @FetchRequest.

- `@FocusBinding`: Introduced in SwiftUI 2.0, this property wrapper allows you to manage the focus state of a view in a more flexible way, such as setting the focus to a specific view or clearing the focus from a view.

- `@GlobalActor`: Introduced in Swift 5.5, this property wrapper allows you to mark a property as belonging to a specific concurrency context or actor. It helps with managing concurrent access to shared resources and is useful in concurrent programming with Swift's concurrency features.

- `@AsyncLet`: Introduced in Swift 5.5, this property wrapper allows you to manage asynchronous tasks in a SwiftUI view. It provides a convenient way to handle asynchronous data loading, such as fetching data from a web service, in SwiftUI views.

- `@ViewBuilder`: Introduced in Swift 5.4, this property wrapper allows you to define a function or closure that returns multiple SwiftUI views as a single view. It's commonly used in SwiftUI APIs that accept multiple child views, such as VStack, HStack, and ZStack.

- `@MainActor`: Introduced in Swift 5.5, this property wrapper is used to specify that a function or property should be accessed on the main (UI) thread. It helps with managing concurrency and ensuring that UI-related code is executed on the main thread, which is necessary for updating UI elements.

- `@StateObject`: Introduced in SwiftUI 2.0, this property wrapper is similar to @State, but it manages the lifecycle of an object independently of the view. It's useful for managing the state of an object that should persist across view updates, such as a data model or a network manager.

- `@AppStorage`: Introduced in SwiftUI 2.0, this property wrapper provides a simple way to store and retrieve user defaults in a SwiftUI app. It automatically handles reading and writing data to the user defaults store, and it's useful for managing app-wide settings or user preferences.

- `@SceneStorage`: Introduced in SwiftUI 2.0, this property wrapper allows you to store and retrieve state information for individual scenes in a multi-scene SwiftUI app. It's useful for managing state that should be preserved when a scene goes inactive or gets terminated, such as user input or app state.

- `@Environment`: This property wrapper is not specific to a particular version of Swift or SwiftUI, but it's worth mentioning as it's a fundamental part of SwiftUI. It allows you to access environment values, such as the current device orientation, accessibility settings, or user interface style, in your views. Examples of @Environment property wrappers include @EnvironmentObject, @Environment(\.colorScheme), and @Environment(\.locale).


- `@StateFlow`: Introduced in Swift 5.5, this property wrapper allows you to create a flow of state changes that can be observed by multiple observers. It's useful for managing state in a reactive way, where changes to the state trigger updates in other parts of your code.

- `@StateObjectBinding`: Introduced in SwiftUI 3.0, this property wrapper allows you to create a binding to a property that's managed by a @StateObject. It provides a convenient way to access and modify the state of an object that's managed by a SwiftUI view.

- `@Namespace`: Introduced in SwiftUI 2.0, this property wrapper allows you to define a namespace for view animations. It's useful for synchronizing animations between multiple views or components in a SwiftUI layout.

- `@ScaledMetric`: Introduced in SwiftUI 3.0, this property wrapper allows you to define a metric that scales dynamically based on the accessibility settings of the user's device. It's useful for creating adaptive layouts that adjust to the user's preferred text size or other accessibility settings.

- `@Clamping`: This is not a built-in property wrapper in Swift, but it's a common custom property wrapper used to enforce a minimum and maximum value for a property. It's useful for constraining the value of a property within a specified range.

- `@Binding`: This is a fundamental property wrapper in SwiftUI that allows you to create a two-way binding between a property in a child view and a property in a parent view. It's used for passing data between views and keeping them in sync with each other.
