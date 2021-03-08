
## Enum Alert Builder in SwiftUI


```swift
enum AlertType: Identifiable {
    case ok(title: String, message: String? =  nil)
    case oneButton(title: String, message: String? =  nil,dismissButton: Alert.Button)
    case twoButton(title: String, message: String? =  nil,primaryButton: Alert.Button, secondaryButton: Alert.Button)
    
    var id: String{
        switch self {
        case .ok:
            return "ok"
        case .oneButton:
            return "oneButton"
        case .twoButton:
            return "twoButton"
            
        }
    }
    
    var alert:Alert {
        switch self {
        case .ok(title: let title, message: let message):
            return Alert(title: Text(title), message: message != nil ? Text(message!) : nil)
            
            
        case .oneButton(title: let title, message: let message, dismissButton: let dismissButton):
            return Alert(title: Text(title), message: message != nil ? Text(message!) : nil, dismissButton: dismissButton)
        case .twoButton(title: let title, message: let message, primaryButton: let primaryButton, secondaryButton: let secondaryButton):
            return Alert(title: Text(title), message: message != nil ? Text(message!) : nil, primaryButton: primaryButton, secondaryButton: secondaryButton)
        }
    }
}

struct FirstView: View {
    @State private var alertType: AlertType? = nil
    var body: some View {
        VStack{
            Button("Title Only"){
                alertType = .ok(title: "This is an alert")
            }
            
            Button("Title an Message"){
                alertType = .ok(title: "This is an alert", message: "This is a message ")
            }
            
            Button("Single Button with Title and message"){
                alertType = AlertType.oneButton(title: "This is an alert",
                                                message: "This is a message",
                                                dismissButton: Alert.Button.default(Text("OK")){
                                                    print("OK button pressed")
                                                }
                )
            }
            
            Button("Two Button with title and message"){
                alertType = AlertType.twoButton(title: "This is an alert",
                                                message: "This is message", primaryButton: .destructive(Text("OK")){
                                                    print("OK button pressed")
                                                }, secondaryButton: .cancel())
            }
            
        }.padding()
        .alert(item: $alertType) { alertType in
            alertType.alert
        }
    }
    
}
```
