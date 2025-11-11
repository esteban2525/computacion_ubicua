<img width="417" height="417" alt="image" src="https://github.com/user-attachments/assets/eea5d9ee-7012-4eee-b3af-e3b40af0198f" />


#include <LiquidCrystal.h>
#include <DHT.h>

// Configuración del LCD
// RS=2, E=3, D4=4, D5=5, D6=6, D7=7
LiquidCrystal lcd(2, 3, 4, 5, 6, 7);

// Configuración del DHT11 en pin 10
#define DHTPIN 10     
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// Configuración del LM35
#define LM35PIN A0    

void setup() {
  lcd.begin(16, 2);  
  dht.begin();
}

void loop() {
  // Leer datos del DHT11
  float humedad = dht.readHumidity();
  float tempDHT = dht.readTemperature();  // <-- temperatura del DHT11

  // Leer temperatura del LM35
  int valorLM35 = analogRead(LM35PIN);
  float voltaje = (valorLM35 * 5.0) / 1023.0;  
  float tempLM35 = voltaje * 100.0;            

  // Limpiar pantalla antes de mostrar
  lcd.clear();

  // Línea 1: Temperatura del DHT11
  lcd.setCursor(0, 0);
  lcd.print("DHT11: ");
  lcd.print(tempDHT, 1);   
  lcd.print("C");

  // Línea 2: Temperatura del LM35
  lcd.setCursor(0, 1);
  lcd.print("LM35 : ");
  lcd.print(tempLM35, 1);  
  lcd.print("C");

  delay(2000); 
}
