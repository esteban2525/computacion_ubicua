//librerias necesarias
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

//configuración pantalla OLED
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

int rele = 8;       // Pin del relé 
int humedad = 0;    // Valor analógico inicial
int porcentaje = 0; // Valor en porcentaje
bool bomba = false; // Estado lógico de la bomba (ON/OFF)

// Calibración para conversión a porcentaje
int seco = 1023;   // Valor en suelo totalmente seco
int humedo = 300;  // Valor en suelo muy húmedo o en agua

void setup() {
  Serial.begin(9600);
  pinMode(rele, OUTPUT); //pin relé como salida
  digitalWrite(rele, HIGH);  // Mantiene el relé apagado al inicio

  // Inicializar pantalla OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("Error: no se encuentra la pantalla OLED");
    for (;;); // se detiene
  }

//Mostrar en pantalla mensaje de inicio
  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
  display.setTextSize(1);
  display.setCursor(10, 10);
  display.println("Sistema iniciado...");
  display.display();
  delay(1500);
}

void loop() {
  humedad = analogRead(A0); //lee el valor de la humedad por medio del sensor

  // Calcular porcentaje (inverso: más humedad = menor valor)
  porcentaje = map(humedad, seco, humedo, 0, 100);
  porcentaje = constrain(porcentaje, 0, 100);

  // --- Control del relé y bomba ---
  if (humedad >= 700) {
    digitalWrite(rele, LOW);   // Activa el relé
    bomba = true;              // Bomba encendida
    Serial.println("Suelo seco -> bomba ENCENDIDA");
  }
  else if (humedad <= 650) {
    digitalWrite(rele, HIGH);  // Desactiva el relé
    bomba = false;             // Bomba apagada
    Serial.println("Suelo húmedo -> bomba APAGADA");
  }
  else {
    Serial.println("Suelo intermedio -> sin cambio");
  }

  // --- Mostrar en pantalla OLED ---
  display.clearDisplay();

  // Título
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println("Humedad del suelo");

  // Porcentaje
  display.setTextSize(2);
  display.setCursor(25, 20);
  display.print(porcentaje);
  display.println("%");

  // Ícono de gota 💧
  drawWaterDrop(100, 40);

  // Estado de la bomba
  display.setTextSize(1);
  display.setCursor(0, 50);
  if (bomba) {
    display.println("Bomba: ON");
  } else {
    display.println("Bomba: OFF");
  }

  display.display();

  // Monitor serie
  Serial.print("Lectura: ");
  Serial.print(humedad);
  Serial.print("  |  ");
  Serial.print("Humedad: ");
  Serial.print(porcentaje);
  Serial.print("%  |  ");
  Serial.println(bomba ? "BOMBA ON" : "BOMBA OFF");

  delay(700);//demora de 7 segs entre lecturas
}

// --- Dibuja una gota de agua ---
void drawWaterDrop(int x, int y) {
  display.fillCircle(x, y, 6, SSD1306_WHITE);   // parte inferior redonda
  display.fillTriangle(x - 6, y, x, y - 12, x + 6, y, SSD1306_WHITE); // parte superior tipo gota
}









