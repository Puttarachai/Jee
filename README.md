#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);   //Module IIC/I2C Interface บางรุ่นอาจจะใช้ 0x3f

#define trigPin D5
#define echoPin D6

void setup() {
  Serial.begin (9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lcd.begin();
  lcd.backlight();
  lcd.home();
  lcd.print("Distance ");
}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10); // Added this line
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;

  lcd.setCursor(0, 1);
  if (distance >= 200 || distance <= 0){
    Serial.println("Out of range");
    lcd.print("Out of range");
  } else {
    Serial.print(distance);
    Serial.println(" cm");
    lcd.print(distance);
    lcd.print(" CM       ");
  }
  delay(500);
}
