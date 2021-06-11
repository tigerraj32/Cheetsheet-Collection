# What's new in SwiftUI 3.0


## Image

### Asynchronously load image from url

**AsyncImage:** A view that asynchronously loads and displays an image.
<br>
This view uses the shared URLSession instance to load an image from the specified URL, and then display it. 

> Syntax: init(url:scale:content:placeholder:). 

``` swift
let url = URL(string: "https://www.bollywoodhungama.com/wp-content/uploads/2021/02/Pooja-Hegde-Wearing-Anita-Dongre.jpg")
var body: some View {

    AsyncImage(url: url) { image in
                          image.scaledToFill()
                          image.resizable()
                         } placeholder: {
        ProgressView()
        .progressViewStyle(.circular)
    }
    .frame(maxWidth: .infinity, maxHeight: .infinity)


}
```

To gain more control over loading process we can even use loading state

```swift
AsyncImage(url: URL(string: "https://example.com/icon.png")) { phase in
    if let image = phase.image {
        image // Displays the loaded image.
    } else if phase.error != nil {
        Color.red // Indicates an error.
    } else {
        Color.blue // Acts as a placeholder.
    }
}
```

## List

### Asynchronously loading the content  in swiftUI list
