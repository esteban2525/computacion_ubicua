#define IN1  4 // adelante D0
#define IN2  5 // atrás D1
#define IN3  6 // izquierda D2
#define IN4  7 // derecha D3

void setup() {
  // Pines del motor como salida
  pinMode(IN1, INPUT);
  pinMode(IN2, INPUT);
  pinMode(IN3, INPUT);
  pinMode(IN4, INPUT);

  // Pines puente como salida (D8 y D9 si quieres PWM, aquí no)
  pinMode(8, OUTPUT); // Motor A
  pinMode(9, OUTPUT); // Motor B
  pinMode(10, OUTPUT); // Motor A reversa
  pinMode(11, OUTPUT); // Motor B reversa
}

void loop() {
  // Leer control
  bool adelante   = digitalRead(IN1);
  bool atras      = digitalRead(IN2);
  bool izquierda  = digitalRead(IN3);
  bool derecha    = digitalRead(IN4);

  if (adelante) {
    // Ambos motores adelante
    digitalWrite(8, HIGH); digitalWrite(9, HIGH);
    digitalWrite(10, LOW); digitalWrite(11, LOW);
  }
  else if (atras) {
    // Ambos motores atrás
    digitalWrite(8, LOW); digitalWrite(9, LOW);
    digitalWrite(10, HIGH); digitalWrite(11, HIGH);
  }
  else if (izquierda) {
    // Giro tipo tanque a la izquierda (un motor adelante, otro atrás)
    digitalWrite(8, LOW);  digitalWrite(9, HIGH);
    digitalWrite(10, HIGH);digitalWrite(11, LOW);
  }
  else if (derecha) {
    // Giro tipo tanque a la derecha (un motor adelante, otro atrás)
    digitalWrite(8, HIGH); digitalWrite(9, LOW);
    digitalWrite(10, LOW); digitalWrite(11, HIGH);
  }
  else {
    // Detener
    digitalWrite(8, LOW); digitalWrite(9, LOW);
    digitalWrite(10, LOW); digitalWrite(11, LOW);
  }
}                                                     PROYECTO CARRITO

-------------------------------------------------------------------------------------------
 
<img width="1480" height="834" alt="image" src="https://github.com/user-attachments/assets/70418d11-abd7-42d4-a757-6878ca42f64d" />

<img width="470" height="834" alt="image" src="https://github.com/user-attachments/assets/91161f32-d9ea-4a55-8de1-ea1e88199e8e" />

<img width="460" height="776" alt="image" src="https://github.com/user-attachments/assets/927b9a55-6d2e-4a29-a90b-347cdbab97c3" />
