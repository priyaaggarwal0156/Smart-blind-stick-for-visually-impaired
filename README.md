# Smart-blind-stick-for-visually-impaired
This repository consist of code smart blind stick for visually impaired project.
Code is as follow:-

// ---------- Pin Definitions ----------
#define trigPin 9
#define echoPin 10
#define buzzerPin 8
#define gasSensorPin A0

// ---------- Variables ----------
long duration;
int distance;
int gasValue;

// Gas threshold (adjust after testing)
int gasThreshold = 1100;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);

  Serial.begin(9600);   // For debugging
}

void loop() {

  // ---------- Ultrasonic Distance Measurement ----------
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;   // Distance in cm


  // ---------- Gas Sensor Reading ----------
  gasValue = analogRead(gasSensorPin);

  // Debugging values
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm | Gas Value: ");
  Serial.println(gasValue);

  // ---------- Gas Alert (Highest Priority) ----------
  if (gasValue > gasThreshold) {
    digitalWrite(buzzerPin, HIGH);  // Continuous sound
    delay(100);
    return;   // Skip obstacle logic
  }

  // ---------- Obstacle Alert ----------
  if (distance > 0 && distance <= 30) {
    // Very close obstacle
    digitalWrite(buzzerPin, HIGH);
    delay(100);
    digitalWrite(buzzerPin, LOW);
    delay(100);
  }
  else if (distance > 30 && distance <= 70) {
    // Moderate distance
    digitalWrite(buzzerPin, HIGH);
    delay(300);
    digitalWrite(buzzerPin, LOW);
    delay(300);
  }
  else {
    // Safe distance
    digitalWrite(buzzerPin, LOW);
  }
}
