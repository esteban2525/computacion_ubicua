<img width="407" height="265" alt="image" src="https://github.com/user-attachments/assets/9a3789a9-99f3-4c11-bbbe-a9b7ea7b7a3b" />



int LED = 3;        // LED en pin 3
int BRILLO;
int POT = 0;        // potenciometro en pin A0

void setup(){
  pinMode(LED, OUTPUT);   // pin 3 como salida
  // las entradas analogicas no requieren inicializacion
}

void loop(){
  BRILLO = analogRead(POT) / 4; // valor leido de entrada analogica divido por 4
  analogWrite(LED, BRILLO); // brillo del LED proporcional al giro del potenciometro
}
