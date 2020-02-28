Documentation: (ref)[https://www.arduino.cc/reference/en/#structure]
(Tutorials)[https://www.arduino.cc/en/Tutorial/HomePage?from=Main.Tutorials]
## setup()
The setup() function is called when a sketch starts. Use it to initialize variables, pin modes, start using libraries, etc. The setup() function will only run once, after each powerup or reset of the Arduino board.

```c
int buttonPin = 3;

void setup() {
  Serial.begin(9600);
  pinMode(buttonPin, INPUT);
}

void loop() {
  // ...
}

```

## loop()
After creating a setup() function, which initializes and sets the initial values, the loop() function does precisely what its name suggests, and loops consecutively, allowing your program to change and respond. Use it to actively control the Arduino board.

```c
int buttonPin = 3;

// setup initializes serial and the button pin
void setup() {
  Serial.begin(9600);
  pinMode(buttonPin, INPUT);
}

// loop checks the button pin each time,
// and will send serial if it is pressed
void loop() {
  if (digitalRead(buttonPin) == HIGH) {
    Serial.write('H');
  }
  else {
    Serial.write('L');
  }

  delay(1000);
}
```


## pinMode()

Configures the specified pin to behave either as an input or an output. See the Digital Pins page for details on the functionality of the pins.
As of Arduino 1.0.1, it is possible to enable the internal pullup resistors with the mode INPUT_PULLUP. Additionally, the INPUT mode explicitly disables the internal pullups.

### Syntax
pinMode(pin, mode)

- pin: the Arduino pin number to set the mode of.

- mode: mode: INPUT, OUTPUT, or INPUT_PULLUP.

```c
void setup() {
  pinMode(13, OUTPUT);      // sets the digital pin 13 as output
  pinMode(13, INPUT);       //sets the digital pin 13 as input.
  pinMode(13, INPUT_PULLUP);  //sets the digital pin 13 as input with internal pull up resistor activated.
}
```



## Hardware Interrupt

In arduino there are two types of interrupt,
- External Interrput.

- Pin Change Interrupt.

### External Interrupt
These interrupt are interpreted by hardware and are very fast. These interrupts can be set to trigger on the event of RISING or FALLING or LOW levels. External Interrupt are attached to 
- pin 2

- Pin 3

To configure interrupt there is a handy function in arduino **attachInterrupt()** that takes three parameter 
- interrupt number: Normally you should use digitalPinToInterrupt(pin) to translate the actual digital pin to the specific interrupt number.

- ISR: Interrupt Service Routine function

- mode:  defines when the interrupt should be triggered. Four constants are predefined as valid values. 
```
```
- LOW to trigger the interrupt whenever the pin is low,

- CHANGE to trigger the interrupt whenever the pin changes value

- RISING to trigger when the pin goes from low to high,

- FALLING for when the pin goes from high to low.
```c
int pin = 2; //define interrupt pin to 2
volatile int state = LOW; // To make sure variables shared between an ISR
//the main program are updated correctly,declare them as volatile.

void setup() {
   pinMode(13, OUTPUT); //set pin 13 as output
   attachInterrupt(digitalPinToInterrupt(pin), blink, CHANGE);
   //interrupt at pin 2 blink ISR when pin to change the value
} 
void loop() { 
   digitalWrite(13, state); //pin 13 equal the state value
} 

void blink() { 
   //ISR function
   state = !state; //toggle the state when the interrupt occurs
}
```

