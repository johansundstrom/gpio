# GPIO
Samlat om GPIO och Arduinokompatibla µController's

## Fördefinierade konstanter
* ```HIGH```		// Digital 1
* ```LOW```		// Digiltal 0
* ```LED_BUILTIN```	// Definierar pin för inbyggd LED
* ```true```		// motsvarar 1 eller Sant
* ```false```		// motsvarar 0 eller falskt
* ```D0```		// int pin *D0* motsvarar GPIO16
* ```D1```		// int pin *D1* motsvarar GPIO5
* ```D2```		// int pin *D2* motsvarar GPIO4
* osv...


## PIN vs GPIO
* D3(arduino board märkning) = pin 0(GPIO0)
Följande är samma kod
```c
int pin = 0; 		// GPIO0 = pin D3(märkning på kortet)
int pin = D3; 		// pin D3(märkning på kortet) = GPIO0
```


## Pinmode INPUT
```c
pinMode(pin, INPUT);		// Gör pin till input
pinMode(pin, INPUT_PULLUP);	// Gör pin till input med intern pullup på 10k 

value = digitalRead(pin);	// Läs input pin
```

## Pinmode OUTPUT
```c
pinMode(pin, OUTPUT);		// Gör pin till output - 12mA driv (Source) ~20mA jord (Sink)

digitalWrite(pin, value);	// Skriv _value_ till pin
```

## Interupt
```c
const int knapp = D2;		// pin D2 --> GND
const int lampa = D3;		// pin D3 --> LED --> 220R --> GND
volatile byte state = LOW;	// initialt state = 0

void setup() {
	pinMode(knapp, INPUT_PULLUP);
	pinMode(lampa, OUTPUT);
   
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

## Lång/kort knapptryck
Ett tryck på button mindre än longPressTime ger LED1, annars LED2
```c
int LED1 = D1;			// pin D1 --> LED --> 220R --> GND
int LED2 = D2;			// pin D2 --> LED --> 220R --> GND
int button = D3;		// VCC --> 10K --> pin D3 --> SW --> GND

boolean LED1State = false;
boolean LED2State = false;

long buttonTimer = 0;
long longPressTime = 1000; // 1S

boolean buttonActive = false;
boolean longPressActive = false;

void setup() {
	pinMode(LED1, OUTPUT);
	pinMode(LED2, OUTPUT);
	pinMode(button, INPUT);
}

void loop() {
	if (digitalRead(button) == LOW) {
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

## Trigga på kantvåg
För att eliminera knappstuds kan man trigga på hög-låg övergång

```c
const int knapp = D11;	// VCC --> SW --> D2 --> 10K --> GND
const int LED = 13;	// 13 --> LED --> 220R --> GND
const int debounceInterval = 50;

function setup() {
	buttonStateRising = digitalRead(knapp);
}

function loop() {
	if ((buttonStateRising == HIGH) && (lastButtonStateRising == LOW)) {
		if (millis() - millisPrevious >= debounceInterval) {  // if debounce interval expired
			DigitalWrite(LED, HIGH);
		}
		millisPrevious = millis();
	}
	lastButtonStateRising = buttonStateRising;
}
```
