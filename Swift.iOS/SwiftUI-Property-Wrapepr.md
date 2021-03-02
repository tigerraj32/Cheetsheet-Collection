
## @Namespace

Creates an animation namespace to allow matched geometry effects, which can be shared by other views. 
`match geometry effect` tells swiftui that these two view are same, which results in smooth animations

eg.

```swift
struct SampleView: View {
    @Namespace private var animation
    
    @State private var isFlipped = false
    var body: some View {
        VStack {
            if isFlipped {
                Circle()
                    .fill(Color.red)
                    .frame(width: 100, height: 100, alignment: .center)
                    .matchedGeometryEffect(id: "Circle", in: animation)
                
                Text("Circle Shape")
                    .font(.headline)
                    .matchedGeometryEffect(id: "Text", in: animation)
            }else{
                Text("Circle Shape")
                    .font(.headline)
                    .matchedGeometryEffect(id: "Text", in: animation)
                
                Circle()
                    .fill(Color.red)
                    .frame(width: 100, height: 100, alignment: .center)
                    .matchedGeometryEffect(id: "Circle", in: animation)
                
               
            }
           
        }.onTapGesture {
            withAnimation(){
                self.isFlipped.toggle()
            }
            
        }
    }
}
```
