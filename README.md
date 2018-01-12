# GPIO
Samlat om GPIO och Arduinokompatibla µController's

## Fördefinierade constanter
* ```HIGH```                  // Digital 1
* ```LOW```                   // Digiltal 0
* ```LED_BUILTIN```           // Definierar pin för inbyggd LED
* ```true```                  // motsvarar 1 eller Sant
* ```false```                 // motsvarar 0 eller falskt

## Pinmode INPUT
```c
pinMode(pin, INPUT);          // Gör pin till input
pinMode(pin, INPUT_PULLUP);   // Gör pin till input med intern pullup på 10k 

value = digitalRead(pin);     // Läs input pin
```

## Pinmode OUTPUT
```c
pinMode(pin, OUTPUT);         // Gör pin till output - 12mA driv (Source) ~20mA jord (Sink)

digitalWrite(pin, value);     // Skriv _value_ till pin
```

## Interupt
```c
const int knapp = 2;                // D2
const int lampa = 3;                // D3
volatile byte state = LOW;          // initialt state = 0

void setup() {
   pinMode(knapp, INPUT_PULLUP);    // switch mellan D0 och jord
   pinMode(lampa, OUTPUT);          // LED och ~220R mellan D1 och jord
   
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

## Long Press
```c
int LED1 = 12;
int LED2 = 13;
int button = 3;

boolean LED1State = false;
boolean LED2State = false;

long buttonTimer = 0;
long longPressTime = 250;

boolean buttonActive = false;
boolean longPressActive = false;

void setup() {
	pinMode(LED1, OUTPUT);
	pinMode(LED2, OUTPUT);
	pinMode(button, INPUT);
}

void loop() {

	if (digitalRead(button) == HIGH) {

		if (buttonActive == false) {
			buttonActive = true;
			buttonTimer = millis();
		}

		if ((millis() - buttonTimer > longPressTime) && (longPressActive == false)) {
			longPressActive = true;
			LED1State = !LED1State;
			digitalWrite(LED1, LED1State);
		}

	} else {

		if (buttonActive == true) {
			if (longPressActive == true) {
				longPressActive = false;
			} else {
				LED2State = !LED2State;
				digitalWrite(LED2, LED2State);
			}
			buttonActive = false;
		}
	}
}
```
