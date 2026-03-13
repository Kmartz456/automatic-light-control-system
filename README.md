#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd_1(0x20, 16, 2);

const int led = 9; // Lamp controlled by light
const int sensorPin = A0; 

int sensorValue = 0;
int threshold = 500;   

void setup()
{
  pinMode(led, OUTPUT);
  Serial.begin(9600);

  lcd_1.init();
  lcd_1.backlight();

  lcd_1.setCursor(0,0);
  lcd_1.print("Light System");
  delay(1000);
  lcd_1.clear();
}

void loop()
{
  sensorValue = analogRead(sensorPin);

  lcd_1.setCursor(0,0);
  lcd_1.print("Environment:");
D
  if(sensorValue > threshold)
  {
    // Bright environment
    analogWrite(led, 0);  // LED OFF

    lcd_1.setCursor(0,1);
    lcd_1.print("Day      ");
  }
  else
  {
    // Dark environment
    analogWrite(led, 255); // LED ON

    lcd_1.setCursor(0,1);
    lcd_1.print("Night    ");
  }

  delay(200);
}
