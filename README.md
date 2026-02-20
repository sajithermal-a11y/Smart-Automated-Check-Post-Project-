# Smart-Automated-Check-Post-Project-
In this video, we demonstrate how to build a smart vehicle detection and automatic barrier system using the Arduino Uno and an ultrasonic sensor.
#include <Servo.h>

Servo gate;

const int trigPin = 9;
const int echoPin = 10;
const int servoPin = 6;

long duration;
int distance;

int openAngle = 90;
int closeAngle = 0;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  gate.attach(servoPin);
  gate.write(closeAngle);

  Serial.begin(9600);
}

void loop() {

  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.println(distance);

  // Vehicle detected within 15 cm
  if (distance > 0 && distance < 15) {
    openGate();
    delay(5000);   // Gate open for 5 seconds
    closeGate();
  }

  delay(200);
}

void openGate() {
  for (int pos = closeAngle; pos <= openAngle; pos++) {
    gate.write(pos);
    delay(15);   // Smooth movement
  }
}

void closeGate() {
  for (int pos = openAngle; pos >= closeAngle; pos--) {
    gate.write(pos);
    delay(15);
  }
}
