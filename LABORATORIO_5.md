<img width="403" height="360" alt="image" src="https://github.com/user-attachments/assets/b4ba8429-c81c-4e00-9561-92561dd09ed3" />


#include <DHT.h>
#include <DHT_U.h>

int SENSOR = 2;
int TEMPERATURA;
int HUMEDAD;   

DHT dht(SENSOR, DHT22);

void setup(){
Serial.begin(9600);
dht.begin();
}

void loop(){
float TEMPERATURA = dht.readTemperature();
float HUMEDAD = dht.readHumidity();

Serial.print("Temperatura: ");
Serial.print(TEMPERATURA);
Serial.print("  Humedad: ");
Serial.println(HUMEDAD);

delay(500);
}
