## Simple blink program

This blink program uses delay of 1000ms = 1s to turn on and off led connected to pin 13.
```c
#define ledPin 13
//const int ledPin =  LED_BUILTIN; 

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
 
  digitalWrite(ledPin, HIGH);
  delay(1000); // Wait for 1000 millisecond(s)
  digitalWrite(ledPin, LOW);
  delay(1000); // Wait for 1000 millisecond(s)
}
```

### without using delay() 
```c
#define ledPin 13
//const int ledPin =  LED_BUILTIN; 
unsigned long blinkInterval = 1000; //in ms
unsigned long t1 = 0;
bool ledStatus = LOW;

void setup()
{
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void blink(){
  	unsigned  long t2 = millis();
   	if((t2 - t1) >= blinkInterval){
  		t1 = t2;
    	ledStatus = !ledStatus;
    	digitalWrite(ledPin, ledStatus);
    }
}
void loop()
{
 blink();
}

```


This code demonstrate how the read switch with debounce time implemented using delay effect the led blinking. Normally when there is no input CPU uses will be  < 1%, but when user presss the switch then CPU uses goes to 100 % and led also don't blink. Because we have halted the code by delay.

```c

//#define ledPin 13
//const int ledPin =  LED_BUILTIN; 


struct Output {
	unsigned int ledPin;
  	unsigned int blinkerLed;
} output = {13, 12};


struct Input {
	unsigned int manualSwitch;

} input = {2};

struct Status {
	bool ledPin;
    unsigned int counter;
    
} status = {LOW, 0}; 

struct Timestamp {
  	unsigned int blinkInterval;
  	unsigned long debounceTime; //ms
	unsigned long now;
  	unsigned long lastBlinker;
} timestamp = {200,300, 0 , 0};



void inputConfig(){
	pinMode(input.manualSwitch, INPUT_PULLUP );
}

void outputConfig(){
	pinMode(output.ledPin, OUTPUT);
  	pinMode(output.blinkerLed, OUTPUT);
}


void setup()
{
  inputConfig();
  outputConfig();
  Serial.begin(9600);
}


void blink(){
  	timestamp.now = millis();
  
   	if((timestamp.now - timestamp.lastBlinker) >= timestamp.blinkInterval){
  		timestamp.lastBlinker = timestamp.now;
    	status.ledPin = !status.ledPin;
    	digitalWrite(output.ledPin, status.ledPin);
    }
}

void countManual() {
  
  bool val = digitalRead(input.manualSwitch);
  if (val == LOW){
    delay(1000);
    bool newVal = digitalRead(input.manualSwitch);
    if (val == newVal){
    	status.counter += 1; 
      	Serial.println(status.counter);
    }
  	
  }
  
  
  
}

void loop()
{
  blink(); 
  countManual();
}

```