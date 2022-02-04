# Typescript

Typescript is a superset of javascript, 

    - Type script Provides 
      - Strong typing : Make app more preditable.
      - Object Oriented features
      - Compile time error
      - Great tooling

TypeScript ----> Transpile -----> Javascript

## Install Typescipt

    npm install -g typescript@latest

    tsc --version

 ## Sample uses

Create file **main.ts** and write down followin code
 ```
 function log(message){
    console.log(message);
    }

var message = "Hello world"

log(message);
 ```   


Transpile ts code. This will create **main.js** file

    tsc main.ts

To run main.js

    node main.js


## Types in typescript

```
let a: number;
let b: boolean;
let c: string;
let d: any;
let e: number[] = [1,2,3];
let f: any[] = [1, 'a', 2, 'b']

//Enum
 const ColorRed = 0;
 const ColorGreen = 1;
 const ColorBlue = 2

 //instead of writing series of constants we can use enum
 
 enum Color {red, green, blue}
 //enum Color {red =0, green=1, blue=2}
 let color = Color.blue
```


## Type assersion / Type casting

Helps get code editor intelligence.

    let message = 'abc'
    let uppercase = message.toUpperCase()

In case we have not declare variable type

    let message1;
    message1 = 'abc';

    let uppercase1 = (<string> message1).toUpperCase();
    let uppercase2 = (message1 as string).toUpperCase();


## Arrow Function

If we have function 

    let log = function(message){
        console.log(message);
    }

In **typescript**

    let log = (message) => {
        console.log(message)
    }
    
    
if we have single line of code inside function

    let log = (message) => console.log(message)

For multiple parameter

        let sum = (x,y) => {
            return x + y
    }

For more that two variable it will be not appropriate to pass all the aregument in function. Instead we can make it

    let add = (point) => {
        return point.x + point.y
    }

    add({
        x:10, 
        y:20
    })


## Interface

    interface Point {
        x: number,
        y: number
    }

    let summation = (point: Point) => {
        return point.x + point.y
    }

## Class

    class  Point {
        x: number; //field
        y: number;

        draw(){
            console.log('x: ' + this.x + ', y: ' + this.y);
        }

       
    }

    let point = new Point();
    point.x = 3;
    point.y  5;
    point.draw();


## Access Modifier

if we have class properties marked as private  then

    class  Point {
        private x: number;
        private y: number;

        constructor(x: number, y:number){
            this.x = x;
            this.y = y;
        }

        draw(){
            //method
            console.log('x: ' + this.x + ', y: ' + this.y);
        }

    
    }

We can reduce this kind of method declaration using inline access modifier. Here constructor will automatically create two new private properties x and y.

    class  Point {
        
        constructor(private x: number, private y:number){ }

        draw(){
            //method
            console.log('x: ' + this.x + ', y: ' + this.y);
        }
    }

