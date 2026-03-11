# automatic-light-control-system

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd_1(32, 16, 2);

const int led = 9;
const int sensorPin = A0;

int sensorValue = 0;
int targetBrightness = 0;
int currentBrightness = 0;

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

  // Convert light level to wanted LED brightness
  targetBrightness = map(sensorValue, 0, 1023, 255, 0);
  targetBrightness = constrain(targetBrightness, 0, 255);

  // Smooth fade toward target brightness
  if (currentBrightness < targetBrightness)
  {
    currentBrightness++;
  }
  else if (currentBrightness > targetBrightness)
  {
    currentBrightness--;
  }

  analogWrite(led, currentBrightness);

  Serial.print("Sensor = ");
  Serial.print(sensorValue);
  Serial.print("  Target = ");
  Serial.print(targetBrightness);
  Serial.print("  Current = ");
  Serial.println(currentBrightness);

  lcd_1.setCursor(0,0);
  lcd_1.print("Light: ");
  lcd_1.print(sensorValue);
  lcd_1.print("    ");

  lcd_1.setCursor(0,1);
  lcd_1.print("LED: ");
  lcd_1.print(currentBrightness);
  lcd_1.print("     ");

  delay(10);
}
