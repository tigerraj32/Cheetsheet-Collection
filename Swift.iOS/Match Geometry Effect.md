# Match Geometry Effect

From **iOS 14** apple include new modifier `matchedGeometryEffect`. This modifier helps developer to create some amazing view animation with few lines of code. All we need to do is describe the appeareance of two views  and use `.matchGeometryEffect` modifier to tell these two view are same. The modifier will then compute the difference betweem those two views and automativally animates the size or position changes. 

### Namespace
We require `@Namespace` to tell swiftui that two view are same that need to considered during view transaction.

## Example 1

### Shape Morphing

**Without Match Geomerty Effect**

In the below code we `toggle` state variable to change the position and shape of object by using `.cornerRadius` and `.offset` modifier. 

```swift

    @State var toggle: Bool = false
    var body: some View {
        
        VStack{
            Rectangle()
                .fill(Color.green)
                .frame(width:  100, height: 100)
                .cornerRadius(toggle ? 50 : 0)
                .offset(x: toggle ? 100 : -100)
                .animation(.default)
                .onTapGesture {
                    self.toggle.toggle()
                }
        }
        .frame(width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height)
        .background(Color.orange)
        .ignoresSafeArea()
    }
        
```
![](./../resources/match-geometry-effect.1.gif)

<br>

**With Match Geomerty Effect**

With the `matchedGeometryEffect` modifier, you no longer need to figure out these differences. All you need to do is describe two views: 
- one represents the start state and 
- the other is for the final state. 

`matchedGeometryEffect` will automatically interpolate the size and position between the views.


```swift
@State var toggle: Bool = false
    @Namespace var animation
    var body: some View {
        VStack{
            if toggle {
                Circle()
                    .fill(Color.green)
                    .matchedGeometryEffect(id: "animation", in: animation)
                    .frame(width:  100, height: 100)
                    .offset(x: 100)
                    .onTapGesture {
                        withAnimation(.default) {
                            self.toggle.toggle()
                        }
                    }
                
            }else{
                Rectangle()
                    .fill(Color.green)
                    .matchedGeometryEffect(id: "animation", in: animation)
                    .frame(width:  100, height: 100)
                    .offset(x: -100)
                    .onTapGesture {
                        withAnimation(.default) {
                            self.toggle.toggle()
                        }
                        
                    }
            }
        }
        .frame(width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height)
        .background(Color.orange)
        .ignoresSafeArea()
```
**Special Note**

- Note 1

In the above code if you comment out `.matchedGeometryEffect` you would get following result.

> .matchedGeometryEffect(id: "animation", in: animation)

![](./../resources/match-geometry-effect.2.gif)

- Note 2

We al need to ay attention on where to put the  `.matchedGeometryEffect` modifier. If you would have used  `.matchedGeometryEffect` modifier after size of position setup. View transaction won't work properly as we expect.

```swift
    .frame(width:  100, height: 100)
    .offset(x: -100)
    .matchedGeometryEffect(id: "animation", in: animation)
```

![](./../resources/match-geometry-effect.3.gif)


- Note 3 : Element vs Container

It is not recommended to use Matched Geometry Effect on containers like VStack, HStack and ZStack. Instead, you should use it on elements like shapes, texts and buttons. We will see the sample code in Example 2 below


## Example 2

**Element vs Container**

Let's first try add `.matchedGeometryEffect` modifier in container itself and see the result.  In below code we applied the `.matchedGeometryEffect` modifier to `VStack`. The final output kind of produce animation but not as we want.

```swift
@State var toggle: Bool = false
    @Namespace var animation
    
    var body: some View {
        
        ZStack{
            if !toggle {
                ScrollView{
                    VStack{
                        Text("Hello")
                            .foregroundColor(.white)
                    }
                    .matchedGeometryEffect(id: "transaction", in: animation)
                    .frame(width: 100, height: 100, alignment: .center)
                    .background(
                        Rectangle()
                            .fill(Color.orange)
                    )
                            
                }
            }else{
                VStack {
                    Text("Hello")
                        .foregroundColor(.white)
                }
                .matchedGeometryEffect(id: "transaction", in: animation)
                .frame(width: UIScreen.main.bounds.width, height: 400, alignment: .center)
                .background(
                    Rectangle()
                        .fill(Color.orange)
                )
            }
        }
        .animation(.spring(response: 0.5, dampingFraction: 0.7))
        .onTapGesture {
            self.toggle.toggle()
        }
```
<img src="./../resources/match-geometry-effect.4.gif" alt="drawing" width="200"/>

<br><br>
Now let's apply `.matchedGeometryEffect` modifier to `Text` and `Shape` rather that to `VStack`.  The outpu produce smooth transaction for both background and text.

```swift

 @State var toggle: Bool = false
    @Namespace var animation
    
    var body: some View {
        ZStack{
            if !toggle {
                ScrollView{
                    VStack {
                        Text("Hello")
                            .foregroundColor(.white)
                            .matchedGeometryEffect(id: "text", in: animation)
                    }
                    .frame(width: 100, height: 100)
                    .background(
                        Rectangle()
                            .fill(Color.orange)
                            .matchedGeometryEffect(id: "background", in: animation)
                    )
                }
            }else{
                VStack {
                    Text("Hello")
                        .foregroundColor(.white)
                        .matchedGeometryEffect(id: "text", in: animation)
                }
                .frame(width: UIScreen.main.bounds.width, height: 400, alignment: .center)
                .background(
                    Rectangle()
                        .fill(Color.orange)
                        .matchedGeometryEffect(id: "background", in: animation)
                )
            }
        }
        .animation(.spring(response: 0.5, dampingFraction: 0.7))
        .onTapGesture {
            self.toggle.toggle()
        }
    }

```
<img src="./../resources/match-geometry-effect.6.gif" alt="drawing" width="200"/>

<br><br>


## Example 3: Some Fun with Geometry Effect

```swift
 @State var toggle: Bool = false
    @Namespace var animation
    
    var body: some View {
        VStack{
            if toggle {
                HStack{
                    Circle()
                        .fill(Color.pink)
                        .matchedGeometryEffect(id: "circle1", in: animation)
                        .frame(width: 30, height: 30)
                    Spacer()
                    Circle()
                        .fill(Color.blue)
                        .matchedGeometryEffect(id: "circle2", in: animation)
                        .frame(width: 30, height: 30)
                }
                    
            }else {
                HStack{
                    Circle()
                        .fill(Color.orange)
                        .matchedGeometryEffect(id: "circle2", in: animation)
                        .frame(width: 30, height: 30)
                    Spacer()
                    Circle()
                        .fill(Color.green)
                        .matchedGeometryEffect(id: "circle1", in: animation)
                        .frame(width: 30, height: 30)
                }
            }
        }
        .frame(width: 100)
        .animation(.linear)
        .onTapGesture {
            self.toggle.toggle()
        }
    }
```

![](./../resources/match-geometry-effect.5.gif)



# Practical Implementation Of Match Geometry Effect

## Example 4 : Placeholder Transaction in Login Input Field

Credit: [Kevsoft](https://www.youtube.com/watch?v=KPYN6sjWvlA&t=313s) + my input :)

![](./../resources/match-geometry-effect.7.gif)


Let's try to create input item that can be used in login or registration or any other form. Here we want to animate the `TextField` placeholder transaction from bottom to top when we start editing and revert back if end editing. 

Some of other animation handling with match geometry effect

- Placeholder of `TextField` transactoin from bottom to up
- `TextField` selection highlight

In this eample we create `ZStack` where we place `Text` (this act as placeholder for TextField) and `TextField` with no placeholder value.  So this `Text` and `TextField` on top of each other.  Because of no placeholder text in `TextField` we see `Text` behind the `TextField`. When we select the `TextField` for editing  we remove the background `Text` and add it to VStack above `TextField`

```swift
struct LoginInputView: View {
    
    var image: String
    var title: String
    @Binding var value: String
    @State var editingMode: Bool = false
    
    var animation: Namespace.ID
    var flag:Bool {
        return editingMode || (value != "")
    }
    
    var body: some View{
        VStack{
            HStack(alignment: .bottom){
                Image(systemName: image)
                    .font(.system(size: 22))
                    .foregroundColor(.primary)
                    .frame(width: 35)
                
                VStack(alignment: .leading, spacing: 6){
                    //value != ""
                    if flag {
                      
                        Text(title)
                            .font(.caption)
                            .fontWeight(.heavy)
                            .foregroundColor(.gray)
                            .matchedGeometryEffect(id: title, in: animation)
                    }
                    
                    
                    ZStack(alignment:.leading){
                        //value == ""
                        if !flag{
                           
                            Text(title)
                                .font(.caption)
                                .fontWeight(.heavy)
                                .foregroundColor(.gray)
                                .matchedGeometryEffect(id: title, in: animation)
                        }
                        
                        if title == "PASSWORD"{
                            
                            SecureField("", text: $value)
                                .keyboardType(.default)
                        }else{
                            TextField("", text: $value, onEditingChanged: { flag in
                                editingMode = flag
                            })
                        }
                        
                    }
                }
            }
            if !editingMode {
                Divider()
            }
            
            
        }
        .padding(.horizontal)
        .padding(.vertical, 10)
        .background(Color.white.opacity(editingMode ? 1: 0))
        .cornerRadius(10)
        .shadow(color: Color.black.opacity(0.1), radius: 5, x: 5, y: 5)
        .shadow(color: Color.black.opacity(0.05), radius: 5, x: -5, y: -5)
        .padding(.horizontal)
        .padding(.top)
        .animation(.linear)
    }
}

```

To use LoginInputView

```swift
@State var email: String = ""
    @State var password: String = ""
    @Namespace var animation

    var body: some View {
          LoginInputView(image: "envelope", title: "EMAIL", value: $email, animation: animation)
    }
```

One thing to notice here in LoginInputView is, we have a computed property `flag` that actually plays the logic behind controlling the animation. 

>  var flag:Bool {
        return editingMode || (value != "")
    }

Further more it depends on two other `State` properties.

- editingMode (let say A)
- (value != "") (checking for some text in value)

The above formula is made using `k-map` with two variable. 'k-map` make it really easy to handle multiple use case scenerio when there is two or mote than two variable.

|Variable A (EditinMode)|Variable B (value != "")|Expected Output C (animation)|
|--|--|--|
|false|false| false |
|false|true| true |
|true|false| true |
|true|fatruelse| true |

This gives the formula 
> C = A + B //+ = Or Operator

You can generate the formula from
 - [K-MAP](https://www.charlie-coleman.com/experiments/kmap/)
 - [32*8](http://www.32x8.com/index.html)
 - Or Manually using K-Map logic

We want to animate the input to new view layout when either `editingMode` or `value != ""` or both is true else we have a normal view

