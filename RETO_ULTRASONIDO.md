#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 6, 5, 4, 3, 2);  // RS, E, D4, D5, D6, D7

int TRIG = 10;
int ECO = 9;
long DURACION;
float DISTANCIA;

void setup() {
  lcd.begin(16, 2);      // inicializa LCD
  pinMode(TRIG, OUTPUT);
  pinMode(ECO, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Generar pulso de 10us
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  // Medir duración del eco
  DURACION = pulseIn(ECO, HIGH);

  // Calcular distancia en cm
  DISTANCIA = DURACION / 58.2;

  // Mostrar en LCD
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.setCursor(0, 1);
  lcd.print(DISTANCIA);
  lcd.print(" cm");

  // Mostrar en Serial también
  Serial.print("Distancia: ");
  Serial.print(DISTANCIA);
  Serial.println(" cm");

  delay(1000);
}



<img width="1106" height="560" alt="image" src="https://github.com/user-attachments/assets/2d48f41d-eafd-48d2-b1a8-6ca7ab1a21a6" />
