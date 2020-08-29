[More on URLImage](https://github.com/dmytro-anokhin/url-image/blob/master/Sources/URLImage/URLImage.swift)

[Video](https://www.youtube.com/watch?v=volfJt7mupo)

```swift

class ImageLoader:ObservableObject {
    @Published var image: UIImage?
    
    init(from url: String) {
        guard  let url = URL(string: url) else {
            print("Invalid URL")
            return
        }
        
        let request = URLRequest(url: url)
        URLSession.shared.dataTask(with: request) {[weak self] (data, response, error) in
            guard let self = self else {return}
            if let error = error {
                print(error.localizedDescription)
                return
            }
            
            guard let data = data else {
                print("No data received")
                return
            }
            
            DispatchQueue.main.async {
                self.image = UIImage(data: data)
            }
            
        }.resume()
        
        
    }
}
struct URLImage: View {
    
    @ObservedObject private var imageLoader: ImageLoader
    let placeholder:UIImage = UIImage(systemName: "person.fill")!
    init(with url: String) {
        self.imageLoader = ImageLoader(from: url)
    }
    
    var body: some View {
        
        Image(uiImage: self.imageLoader.image ?? placeholder)
            .resizable()
            .scaledToFit()
            .frame(width: 100, height: 100)
    }
    
    
}

struct ContentView: View {
    var body: some View {
        List{
            ForEach(1...100, id: \.self){ row in
                URLImage(with: "https://yt3.ggpht.com/a/AGF-l7_v0UbboMfMZloDsoQFL_Uqn_p5iKdLULAeew=s900-c-k-c0xffffffff-no-rj-mo")
                .frame(width: 200, height: 300)
            }
        }
        
    }
}


```
