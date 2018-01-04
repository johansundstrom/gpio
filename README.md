# GPIO
Samlat om GPIO och Arduinokompatibla µController's

## Fördefinierade constanter
* ```HIGH```                  // Digital 1
* ```LOW```                   // DIgiltal 0
* ```LED_BUILTIN```           // Definierar pin för inbyggd LED
* ```true```                  // motsvarar 1 eller Sant
* ```false```                 // motsvarar 0 eller falskt

## Pinmode INPUT
```c
pinMode(pin, INPUT);          // set pin to input
pinMode(pin, INPUT_PULLUP);   // set pin to input with 10k internally PU

value = digitalRead(pin);     // read the input pin
```

## Pinmode OUTPUT
```c
pinMode(pin, OUTPUT);         // set pin to output - 12mA driv (Source) ~20mA jord (Sink)

digitalWrite(pin, value);     // sets the LED to the button's value
```

## Interupt
```c
const int knapp = 2;                // D2 (GPIO16)
const int lampa = 3;                // D3 (GPIO5)
volatile byte state = LOW;          // initialt state = 0

void setup() {
   pinMode(knapp, INPUT_PULLUP);    // switch mellan D0 och jord
   pinMode(lampa, OUTPUT);          // LED och 220R mellan D1 och jord
   
   // CHANGE - trigg på varje förändring [ LOW | CHANGE  RISING | FALLING | HIGH ]
   attachInterrupt(digitalPinToInterrupt(knapp), ISR, CHANGE);
}

void loop() {
   digitalWrite(lampa, state);
}

void ISR() {
   state = !state
}
```
