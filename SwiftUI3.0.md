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

SwiftUI `list` now have `.task` modifier to invoke async operation to load data. 

```swift
struct States: Decodable, Identifiable {
    let id: String
    let district: [District]
}
struct District: Decodable, Identifiable {
    let id: String
    let eng: String
    let nep: String
}


@available(iOS 15.0, *)
struct ContentView: View {
    
    @State private var states: [States] = []
    var body: some View {
        List {
            ForEach(states) { state in
                Section(header: Text(state.id)) {
                    ForEach(state.district) { district in
                        VStack(alignment: .leading){
                            Text(district.eng)
                                .font(.headline)
                            Text(district.nep)
                                .font(.subheadline)
                        }
                    }
                }
            }
        }
        .task{
            async {
                self.states = try await self.loadStateAndDistrictOfNepal()
            }
        }
        
    }
    
    private func loadStateAndDistrictOfNepal() async throws -> [States]{
        // I am forced unwrapping but you should make sure to unwrap is safely
        let photosURL = URL(string: "https://gist.githubusercontent.com/tigerraj32/246d5bbd10350fc075da1ce68a095d8a/raw/838f19276fc3d4331ad9e27f44f1036ae182b8bd/nepal-pradesh-district.json")!
        let (data, _) = try await URLSession.shared.data(from: photosURL)
        do{
            let states = try JSONDecoder().decode([States].self, from: data)
            return states
        }catch let error {
            print(error)
            return []
        }
    }
    
}

```

### Pull to refresh

SwiftUI `List` now also have `.refreshable` modifier that adds pull to refresh functionality easily.

```swift
List{
    ......
}
.refreshable {
   async {
      self.states = try await self.loadStateAndDistrictOfNepal()
      }
}
```
