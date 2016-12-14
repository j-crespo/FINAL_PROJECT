# FINAL_PROJECT

FINAL CODE

#include <Servo.h>

Servo myservo;

//#include "pitches.h"

int inputPin = 8;               // input pin - for PIR sensor
int servoPin = 7;               // pin for servo
int pos = 0;                    // servo position
int ledPin = 6;                 // pin for the LED
int tonePin = 5;                // pin for the speaker
int pirState = LOW;             // we start, assuming no motion detected
int val = 0;                    // variable for reading the pin status

void setup() {
  //MOTION SENSOR
  pinMode(inputPin, INPUT);
  pinMode(servoPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(tonePin, OUTPUT);

  Serial.begin(9600);

  //SERVO
  myservo.attach(7);  // attaches the servo on pin 7 to the servo object

}

void loop() {
  val = digitalRead(inputPin);  // read input value
  if (val == HIGH) {            // check if the input is HIGH


    if (pirState == LOW) {
      // we have just turned on
      Serial.println("Motion detected!");
      // We only want to print on the output change, not state
      pirState = HIGH;

      // Play tone
      tone(tonePin, 50, 1000);

      // Blink LED
      digitalWrite(ledPin, HIGH);   // turn the LED on (HIGH is the voltage level)
      delay(1000);                       // wait for a second
      digitalWrite(ledPin, LOW);    // turn the LED off by making the voltage LOW
      delay(1000);                       // wait for a second

      // Switch Servo
      myservo.write(0);
      delay(250);
      myservo.write(400);
      delay(250);
      myservo.write(0);

    }
    digitalWrite(ledPin, HIGH);   // turn the LED on (HIGH is the voltage level)
    delay(1000);                       // wait for a second
    digitalWrite(ledPin, LOW);    // turn the LED off by making the voltage LOW
    delay(1000);                       // wait for a second

  } else {
    digitalWrite(ledPin, LOW); // turn LED OFF

    if (pirState == HIGH) {
      // we have just turned of
      Serial.println("Motion ended!");
      // We only want to print on the output change, not state
      pirState = LOW;
    }
  }
}
