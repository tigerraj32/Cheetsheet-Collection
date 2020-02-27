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